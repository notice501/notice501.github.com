---
title: 记一次orm库性能优化，谈谈android的注解处理和代码生成
date: 2017-11-06 20:51:23
tags: orm, Annotation Processing, 注解处理
---

很早的时候因为业务的需要，找不到一款合适的orm框架，当时自己写了一个。很长时间过去了，一直想对这个orm库做一次性能优化，换掉原有的反射的注解处理方式，而是通过代码生成，从而大幅提高性能。

原有的orm库通过反射的注解处理方式来实现。原有的Table注解声明：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Table {

    public String name();

    @Target(ElementType.FIELD)
    @Retention(RetentionPolicy.RUNTIME)
    @interface Column {

        public String name();


        public boolean nullable() default true;

        public boolean isUnionKey() default false;
      
        ......
    }
  ......
}
```

这里简单介绍下注解的知识。通过关键字@interface来定义注解，这里定一个了一个名为Table的注解，Table声明本身上面也有两个注解，这个叫做元注解，一共有四种元注解，分别为：

- `@Retention` 注解的有效范围
  - `SOURCE`，只在源码中可用，一般用来增加代码理解和代码检查等，常见的如Override
  - `CLASS`, 默认选项，在源码和字节码中可用，但仅存于字节码文件中，运行时无法获取
  - `RUNTIME`, 在源码,字节码,运行时均可用
- `@Target` 指定注解修饰的程序元素，如 `TYPE`, `METHOD`, `CONSTRUCTOR`, `FIELD`, `PARAMETER`等，未标注则表示可修饰所有
- `@Inherited` 是否可以被继承，默认为false
- `@Documented` 是否会保存到 Javadoc 文档中

回到上述的表注解定义，可以看到定义了一个Target为TYPE的Table类注解和一个Type为FIELD的Column成员变量注解。

完成以后，我们就可以如下来定一个对象的同时创建一张表：

```java
@Table(name = User.TABLE_NAME)
public class User extends Table{
    public static final String TABLE_NAME = "user";

    public static final String COLUME_ID = "id";
    public static final String COLUME_NAME = "name";
    public static final String COLUME_AGE = "age";
    public static final String COLUME_MALE = "male";

    @Table.Column(name = COLUME_ID, isPrimaryKey = true)
    String id;

    @Table.Column(name = COLUME_NAME, defaultValue = "unknown")
    String name;

    @Table.Column(name = COLUME_AGE)
    int age;

    @Table.Column(name = COLUME_MALE)
    boolean isMale;
}
```

一个简单的User对象，有id，name，age，isMale四个成员变量，通过注解声明表名为有四个colunm的'user'表。

使用orm库让插入和建表一样简单，不用写sql操作：

```java
User user = new User();
user.id = 0;
user.name = "Tom";
user.age =10;
user.save();
```

上面代码定义了一个User对象，并调用了User对象的save方法，save()方法来自父类Table类。这里说下整个建表和插入一个user数据的核心逻辑。

首先建表过程，关于获取列信息的部分如下：

```java
Field[] fields = columnType.getDeclaredFields();
for (Field field : fields) {
	if (field.isAnnotationPresent(Column.class)) {                  
    	Column annotationColumn = field.getAnnotation(Column.class);
        if (annotationColumn == null) {
            return null;
        }
        String columnName = columnAnnotation.name();
        String isPrimaryKey = columnAnnotation.isPrimaryKey();
        .........
        Class<?> type = field.getType();
        SQLiteType columnType = JavaToSQLiteType.getJavaSQLiteType(type);
    }
}
```

为了控制篇幅，不讲具体orm实现，只贴出部分核心代码。首先通过反射获取User里所有的fields，判断field是否有Column注解，有则获取注解上的column信息，包括每个column的columnName，isPrimaryKey等。拿到了每一个列的具体信息，再完成java类型到sqlite类型的转换，表的列信息就全部获取到了。最后将其生成sql代码并执行，这样一张表就建好了。

插入操作也是通过反射来完成，通过反射获取user对象的所有field的值，再通过之前建表时就获取的表名，列名就可以用sql语句完成数据的插入。

可以看到整个orm的核心逻辑都是通过反射来实现的，包括表的创建以及增删改查。这也是这个库1.x版本的性能瓶颈，实测下来插入查询都会比一些流行库慢很多。所以这次优化的核心点就是去反射。也就是要替换原有通过反射来获取用户声明的表结构的部分。实现的方法就是接下来要讲的通过编译时注解进行代码生成，完成表结构获取，增删改查的代码生成。从而去掉大量反射逻辑，大幅提高性能。

首先改变注解定义：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.CLASS)
public @interface Table {

    public String name();

    @Target(ElementType.FIELD)
    @Retention(RetentionPolicy.CLASS)
    @interface Column {

        public String name();

        public boolean nullable() default true;

        public boolean isUnionKey() default false;
      
        ......
    }
  ......
}
```

注意唯一改变的就是@Retention注解，从原有的`RetentionPolicy.RUNTIME`改成`@Retention(RetentionPolicy.CLASS)`。前面已经提到，也就是从运行时注解改为编译时注解，从而在编译时通过注解处理生成代码，完成表信息，增删改查接口的代码生成。

还是说建表和插入操作。建表只需要获取声明的表信息，所以应该提供一个方法来获取表的所有列信息，而插入方法很直白，就是要用于数据插入。那代码生成需要一个initColumnInfoList方法和一个save方法。来看实现。

注解的处理只需继承AbstractProcessor类，主要实现process方法。

```java
@AutoService(Processor.class)
@SupportedOptions({OrmProcessor.TARGET_MODULE_NAME})
public class OrmProcessor extends AbstractProcessor {

    ........

    @Override
    public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {


        handleTableAnnotation(roundEnv);
		....
        return true;
    }

    /**
     * 处理 @Table 注解
     *
     * @param roundEnv
     */
    private void handleTableAnnotation(RoundEnvironment roundEnv) {
        Set<? extends Element> set = roundEnv.getElementsAnnotatedWith(Table.class);
        for (Element element : set) {
            if (isValid(element)) {
                TypeElement typeElement = (TypeElement) element;
                PackageElement packageElement = (PackageElement) element.getEnclosingElement();

                Map<Table.Column, VariableInfo> columnMap = new TreeMap<>(AdapterGenerator.sTableColumnComparator);

                List<VariableElement> variableElementList = ElementFilter.fieldsIn(typeElement.getEnclosedElements());
                for (VariableElement ve : variableElementList) {
                    Table.Column column = ve.getAnnotation(Table.Column.class);
                    if (column == null) {
                        continue;
                    }
                    String variableName = ve.getSimpleName().toString();

                    // 获取属性类型
                    TypeMirror typeMirror = ve.asType();

                    columnMap.put(column, new VariableInfo(variableName, typeMirror));
                }

                String fullName = typeElement.getQualifiedName().toString();
                String className = typeElement.getSimpleName().toString();
                String packageName = packageElement.getQualifiedName().toString();

                ClassInfo classInfo = new ClassInfo(fullName, className, packageName);

                Table table = typeElement.getAnnotation(Table.class);
                String tableName = table.name();

                AdapterGenerator.generateTableAdapter(processingEnv.getFiler(), classInfo, tableName, columnMap);

                tableMap.put(fullName, ADAPTER_PACKAGE + "." + className + TABLE_ADAPTER_POSTFIX);
            }
        }

    }
  
  ....... 

}
```

通过参数roundEnv拿到User表注解中的所有信息，包括表名,列名以及类型。其中`AdapterGenerator.generateTableAdapter`方法将获取到的表信息生成java代码

```java
 public static void generateTableAdapter(Filer filer, ClassInfo classInfo, String tableName,
                                            Map<Table.Column, VariableInfo> columnMap) {

    
       .....

        // initColumnInfoList方法
        MethodSpec.Builder initListBuilder = MethodSpec.methodBuilder("initColumnInfoList")
                .addAnnotation(Override.class)
                .addModifiers(Modifier.PROTECTED)
                .addStatement("columnInfoList = new $T()", arrayListClassName);
        for (Map.Entry<Table.Column, VariableInfo> entry : columnMap.entrySet()) {
            Table.Column column = entry.getKey();
            VariableInfo variableInfo = entry.getValue();

            initListBuilder.addStatement("$T info_$L = new $T($L, $S, $L, $S, $L, $L, $L, $L, $S, $L, $L)",
                    tableColumnInfoClassName,
                    variableInfo.name,
                    tableColumnInfoClassName,
                    variableInfo.typeName.toString() + ".class",
                    column.name(),
                    column.nullable(),
 					......
 
            );
            initListBuilder.addStatement("columnInfoList.add(info_$L)", variableInfo.name);
        }
        MethodSpec initList = initListBuilder.build();

    // save方法
        MethodSpec.Builder saveBuilder = MethodSpec.methodBuilder("save")
                .addAnnotation(Override.class)
                .addModifiers(Modifier.PUBLIC)
                .returns(long.class)
                .addParameter(modelClassName, "model")
                .addStatement("$T contentValues = new $T()", contentValueClassName, contentValueClassName);
        for (Map.Entry<Table.Column, VariableInfo> entry : columnMap.entrySet()) {
            Table.Column column = entry.getKey();
            VariableInfo variableInfo = entry.getValue();
            // 自增字段不主动赋值
            if (!column.isAutoincrement()) {
                saveBuilder.addStatement("contentValues.put($S, $L)", column.name(),
                        getVariableGetter(variableInfo.name, variableInfo.typeName));
            }
        }
        saveBuilder.addStatement("if (database == null) {getDatabase();}");
        saveBuilder.addStatement("return database.insert(tableName, null, contentValues)");
        MethodSpec save = saveBuilder.build();
	......

    }
```

上述代码通过[javapoet](https://github.com/square/javapoet)来生成java代码，遍历传进来的所有column名称以及类型，生成了`initColumnInfoList`和`save`方法。如下：

```java
 @Override
  protected void initColumnInfoList() {
    columnInfoList = new ArrayList();
    TableColumnInfo info_name = new TableColumnInfo(java.lang.String.class, "name", true, "unknown", false, false, false, false, "", new String[]{}, -2147483648);
    columnInfoList.add(info_name);
    TableColumnInfo info_isMale = new TableColumnInfo(boolean.class, "male", true, "", false, false, false, false, "", new String[]{}, -2147483648);
    columnInfoList.add(info_isMale);
    TableColumnInfo info_id = new TableColumnInfo(java.lang.String.class, "id", true, "", true, false, false, false, "", new String[]{}, -2147483648);
    columnInfoList.add(info_id);
    TableColumnInfo info_age = new TableColumnInfo(int.class, "age", true, "", false, false, false, false, "", new String[]{}, -2147483648);
    columnInfoList.add(info_age);
  }

  @Override
  public long save(User model) {
    ContentValues contentValues = new ContentValues();
    contentValues.put("name", model.getName());
    contentValues.put("male", model.isMale());
    contentValues.put("id", model.getId());
    contentValues.put("age", model.getAge());
    if (database == null) {getDatabase();};
    return database.insert(tableName, null, contentValues);
  }
```

initColumnInfoList构造了columnInfoList，有了他自然可以建表了，而save方法很清晰就是常用的一个插入操作了。

当然，生成代码的initColumnInfoList方法还是需要通过反射来调一次。

这样优化后，效率大幅提升，数据表插入效率提升30%，查询效率提升60%。

再总结下，其实大部分依赖反射的实现就都可以通过代码生成来解决。代码生成提高了库本身的复杂度，也增加了使用者的编译速度，但是相比性能的大幅提示来说，还是非常值得的。

写完回头看看觉得有点杂糅，因为既涉及到orm的实现，又涉及到注解处理。有空再单独写篇注解处理吧。
