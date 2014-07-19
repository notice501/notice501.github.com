---
layout: post
title: "每日vim插件--强大的自动补全neocomplete.vim和supertab"
date: 2014-04-28 21:02
comments: true
categories: [vim]
---

#neocomplete.vim

今天介绍两个个必备的vim插件，自动补全插件——[neocomplete.vim](https://github.com/Shougo/neocomplete.vim)和superTab。

neocomplete.vim是来自shougo的作品。该插件维护了当前buffer的一个关键词列表，从而提供强大的关键词补全功能。

该插件是他前作neocomplcache的升级版，速度更快，功能更强大。不过该插件需要[if_lua](http://vimdoc.sourceforge.net/htmldoc/if_lua.html)的支持。

mac下安装：

	brew install macvim --with-cscope --with-lua --HEAD

或者不用macvim（真的不用么？赶紧试试吧）：

	brew install vim --with-lua
	

不需要过多的介绍,看作者给的图：

<!--more-->

### 文件补全

![Original filename completion.](https://f.cloud.github.com/assets/41495/622454/f519f6b8-cf42-11e2-921e-6e34dba148a6.png)
![Include filename completion.](https://f.cloud.github.com/assets/214488/623151/284ad86e-cf5b-11e2-828e-257d31bf0572.png)

### Omni 补全

![Omni completion.](https://f.cloud.github.com/assets/41495/622456/fb2cc0bc-cf42-11e2-94e8-403cdcf5427e.png)

### [vimshell](http://github.com/Shougo/vimshell)补全

![Completion with vimshell(http://github.com/Shougo/vimshell).](https://f.cloud.github.com/assets/41495/622458/01dbc660-cf43-11e2-85f1-326e7432b0a1.png)

### Vim 补全

![Vim completion.](https://f.cloud.github.com/assets/41495/622457/fe90ad5e-cf42-11e2-8e03-8f189b5e26e5.png)
![Vim completion with animation.](https://f.cloud.github.com/assets/214488/623496/94ed19a2-cf68-11e2-8d33-3aad8a39d7c1.gif)

作者还给出了推荐配置，每个配置都有对应的英文注释，我就不一一翻译了：

```vim
"Note: This option must set it in .vimrc(_vimrc).  NOT IN .gvimrc(_gvimrc)!
" Disable AutoComplPop.
let g:acp_enableAtStartup = 0
" Use neocomplete.
let g:neocomplete#enable_at_startup = 1
" Use smartcase.
let g:neocomplete#enable_smart_case = 1
" Set minimum syntax keyword length.
let g:neocomplete#sources#syntax#min_keyword_length = 3
let g:neocomplete#lock_buffer_name_pattern = '\*ku\*'

" Define dictionary.
let g:neocomplete#sources#dictionary#dictionaries = {
    \ 'default' : '',
    \ 'vimshell' : $HOME.'/.vimshell_hist',
    \ 'scheme' : $HOME.'/.gosh_completions'
        \ }

" Define keyword.
if !exists('g:neocomplete#keyword_patterns')
    let g:neocomplete#keyword_patterns = {}
endif
let g:neocomplete#keyword_patterns['default'] = '\h\w*'

" Plugin key-mappings.
inoremap <expr><C-g>     neocomplete#undo_completion()
inoremap <expr><C-l>     neocomplete#complete_common_string()

" Recommended key-mappings.
" <CR>: close popup and save indent.
inoremap <silent> <CR> <C-r>=<SID>my_cr_function()<CR>
function! s:my_cr_function()
  return neocomplete#close_popup() . "\<CR>"
  " For no inserting <CR> key.
  "return pumvisible() ? neocomplete#close_popup() : "\<CR>"
endfunction
" <TAB>: completion.
inoremap <expr><TAB>  pumvisible() ? "\<C-n>" : "\<TAB>"
" <C-h>, <BS>: close popup and delete backword char.
inoremap <expr><C-h> neocomplete#smart_close_popup()."\<C-h>"
inoremap <expr><BS> neocomplete#smart_close_popup()."\<C-h>"
inoremap <expr><C-y>  neocomplete#close_popup()
inoremap <expr><C-e>  neocomplete#cancel_popup()
" Close popup by <Space>.
"inoremap <expr><Space> pumvisible() ? neocomplete#close_popup() : "\<Space>"

" For cursor moving in insert mode(Not recommended)
"inoremap <expr><Left>  neocomplete#close_popup() . "\<Left>"
"inoremap <expr><Right> neocomplete#close_popup() . "\<Right>"
"inoremap <expr><Up>    neocomplete#close_popup() . "\<Up>"
"inoremap <expr><Down>  neocomplete#close_popup() . "\<Down>"
" Or set this.
"let g:neocomplete#enable_cursor_hold_i = 1
" Or set this.
"let g:neocomplete#enable_insert_char_pre = 1

" AutoComplPop like behavior.
"let g:neocomplete#enable_auto_select = 1

" Shell like behavior(not recommended).
"set completeopt+=longest
"let g:neocomplete#enable_auto_select = 1
"let g:neocomplete#disable_auto_complete = 1
"inoremap <expr><TAB>  pumvisible() ? "\<Down>" : "\<C-x>\<C-u>"

" Enable omni completion.
autocmd FileType css setlocal omnifunc=csscomplete#CompleteCSS
autocmd FileType html,markdown setlocal omnifunc=htmlcomplete#CompleteTags
autocmd FileType javascript setlocal omnifunc=javascriptcomplete#CompleteJS
autocmd FileType python setlocal omnifunc=pythoncomplete#Complete
autocmd FileType xml setlocal omnifunc=xmlcomplete#CompleteTags

" Enable heavy omni completion.
if !exists('g:neocomplete#sources#omni#input_patterns')
  let g:neocomplete#sources#omni#input_patterns = {}
endif
"let g:neocomplete#sources#omni#input_patterns.php = '[^. \t]->\h\w*\|\h\w*::'
"let g:neocomplete#sources#omni#input_patterns.c = '[^.[:digit:] *\t]\%(\.\|->\)'
"let g:neocomplete#sources#omni#input_patterns.cpp = '[^.[:digit:] *\t]\%(\.\|->\)\|\h\w*::'

" For perlomni.vim setting.
" https://github.com/c9s/perlomni.vim
let g:neocomplete#sources#omni#input_patterns.perl = '\h\w*->\h\w*\|\h\w*::'
```

没有特殊需求，直接copy就用即可。没有复杂的配置，用起来还是非常简单的。

还有一个比较出名的补全插件是[YouCompleteMe](https://github.com/Valloric/YouCompleteMe).大家也可以去看下，应该是现在用的最广泛的补全插件了。我很早就试用过，还是neocomplete要更顺手更快些，总感觉neocomplete要更智能些。


#superTab

说到补全还有个不得不说，那就是tab键，必须用tab来进行补全那才是补全啊。其实neocomplete的推荐配置已经配置成了自动提示补全文字，并且支持tab选择，但是还是无法`shift-tab`回退选择。superTab就是为增强tab而生，当然可以做到这点。

superTab 和neocomplete一样，几乎不用自己折腾什么配置，也不用过多的介绍，一句话就可以说完它的功能。

	bar
	baz
	b*<Tab> (*为光标所在位置) 

按示例在光标处按下tab，就会展开推荐补全bar和baz，按tab即可进行循环选择。

可以配置supertab的默认补全类型（对vim补全不了解的同学自己补下omni补全相关知识，需要我介绍可以回复下）：


	let g:SuperTabDefaultCompletionType = "<c-x><c-u>"
	
示例把supertab修改为了用户补全，默认是`<c-p>`.

今天就介绍到这里。



