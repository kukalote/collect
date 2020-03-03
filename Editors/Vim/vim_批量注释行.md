### 多行注释：

1. 进入命令行模式，按 `ctrl + v` 进入 visual block模式（可视快模式），然后按 `j` , 或者 `k` 选中多行，把需要注释的行标记起来

2. 按大写字母 `i`，再插入注释符，例如 `//`

3. 按 `esc` 键就会全部注释了（我的是按两下）




### 取消多行注释：

1. 进入命令行模式，按 `ctrl + v` 进入 visual block模式（可视快模式），按小写字母L横向选中列的个数，例如 `//` 需要选中2列

2. 按字母 `j`，或者 `k` 选中注释符号
3. 按 `d` 键就可全部取消注释


### map 注释命令

1. 初级操作
```
   " lhs comments
    map ,# :s/^/#/<CR>:nohlsearch<CR>
    map ,/ :s/^/\/\//<CR>:nohlsearch<CR>
    map ,> :s/^/> /<CR>:nohlsearch<CR>
    map ," :s/^/\"/<CR>:nohlsearch<CR>
    map ,% :s/^/%/<CR>:nohlsearch<CR>
    map ,! :s/^/!/<CR>:nohlsearch<CR>
    map ,; :s/^/;/<CR>:nohlsearch<CR>
    map ,- :s/^/--/<CR>:nohlsearch<CR>
    map ,c :s/^\/\/\\|^--\\|^> \\|^[#"%!;]//<CR>:nohlsearch<CR>

    " wrapping comments
    map ,* :s/^\(.*\)$/\/\* \1 \*\//<CR>:nohlsearch<CR>
    map ,( :s/^\(.*\)$/\(\* \1 \*\)/<CR>:nohlsearch<CR>
    map ,< :s/^\(.*\)$/<!-- \1 -->/<CR>:nohlsearch<CR>
    map ,d :s/^\([/(]\*\\|<!--\) \(.*\) \(\*[/)]\\|-->\)$/\2/<CR>:nohlsearch<CR>
```

2. 使用方法
	```
	fun CppstyleQuote()
	  let hls=@/
	  s/^/\/\//
	  let @/=hls
	endfun
	map ,/ :call CppstyleQuote()<CR>
	```

	```
	" I use a single mapping, which can comment/uncomment line:
	function! C_CommentLine()
	  if getline(".") =~ '/\*.*\*/'
		normal ^2x$xx
	  else
		normal I/*A*/
	  endif
	endfunction
	nmap <buffer> <Esc>d :call C_CommentLine()<LF>

	" <Esc>d on my term means <M-D> or Alt-D

	" This mapping comments the visual selection, You have to perform a selection from left to
	" right or from top to bottom. If you do a linewise selection, the cursor
	" position, not the end of selection matters.
	vmap <buffer> <Esc>d a*/gvoi/*
	```

	```
	:vmenu PopUp.Comments.Add :s/^/\/\//<CR>
	:vmenu PopUp.Comments.Remove :s/^..//<CR>
	```
	```
	function! MyComm()
	  let linenum= line('.')
	  let line = getline('.')
	  let commpos= match(line, "//")
	  let n = 0
	  while n< commpos
		if line[n]!= " " && line[n]!= "\t"
		  break
		endif
		let n= n+1
	  endwhile
	  if n== commpos && commpos!= -1
		let line= strpart(line, 0, commpos).strpart(line, commpos+2)
	  else
		let line= "//".line
	  endif
	  let err= setline(linenum, line)
	endfunction
	map <M-c> :call MyComm()<CR>
	imap <M-c> <Esc>:call MyComm()<CR>i

	" for the /* */ pair, I use visual mode, an then alt-v:
	vmap <M-v> v`<I<CR><Esc>k0i/*<Esc>`>I<CR><Esc>k0i*/<Esc>
	```

	```
	" Define functions
	function! PoundComment()
	  map - :s/^/# /<CR>
	  map _ :s/^\s*# \=//<CR>
	  set comments=:#
	endfunction

	function! SlashComment()
	  map - :s/^/\/\/ /<CR>
	  map _ :s/^\s*\/\/ \=//<CR>
	endfunction

	" And then later...
	autocmd FileType perl call PoundComment()
	autocmd FileType cgi call PoundComment()
	autocmd FileType csh call PoundComment()
	autocmd FileType sh call PoundComment()
	autocmd FileType java call SlashComment()
	```

	```
	map \# :call Comment()<CR>
	map \-# :call Uncomment()<CR>
	map \--# :call UncommentBlock()<CR>

	" Comments range (handles multiple file types)
	function! Comment() range
	  if &filetype == "c" || &filetype == "php" || &filetype == "css"
		execute ":" . a:firstline . "," . a:lastline . 's/^\(.*\)$/\/\* \1 \*\//'
	  elseif &filetype == "html" || &filetype == "xml" || &filetype == "xslt" || &filetype == "xsd"
		execute ":" . a:firstline . "," . a:lastline . 's/^\(.*\)$/<!-- \1 -->/'
	  else
		if &filetype == "java" || &filetype == "cpp" || &filetype == "cs"
		  let commentString = "//"
		elseif &filetype == "vim"
		  let commentString = '"'
		else
		  let commentString = "#"
		endif
		execute ":" . a:firstline . "," . a:lastline . 's,^,' . commentString . ','
	  endif
	endfunction

	" Uncomments range (handles multiple file types)
	function! Uncomment() range
	  if &filetype == "c" || &filetype == "php" || &filetype == "css" || &filetype == "html" || &filetype == "xml" || &filetype == "xslt" || &filetype == "xsd"
		" [[VimTip271]]
		execute ":" . a:firstline . "," . a:lastline . 's/^\([/(]\*\|<!--\) \(.*\) \(\*[/)]\|-->\)$/\2/'
	  else
		if &filetype == "java" || &filetype == "cpp" || &filetype == "cs"
		  let commentString = "//"
		elseif &filetype == "vim"
		  let commentString = '"'
		else
		  let commentString = "#"
		endif
		execute ":" . a:firstline . "," . a:lastline . 's,^' . commentString . ',,'
	  endif
	endfunction

	" Uncomments from current line up to last line that's commented
	function! UncommentBlock()
	  if &filetype == "c" || &filetype == "php" || &filetype == "css" || &filetype == "html" || &filetype == "xml" || &filetype == "xslt" || &filetype == "xsd"
		echoerr "TODO: haven't implemented UncommentBlock; use Uncomment instead"
	  else
		if &filetype == "java" || &filetype == "cpp" || &filetype == "cs"
		  let commentString = '\/\/'
		  let firstChar = '/'
		elseif &filetype == "vim"
		  let commentString = '"'
		  let firstChar = '"'
		else
		  let commentString = '#'
		  let firstChar = '#'
		endif
		if version < 600 && strlen( commentString ) > 1
		  echoerr "TODO: haven't implemented multi-character comment block"
		else
		  " TODO: doesn't handle case where the block ends at end of file
		  execute ':.,/^\(\(' . commentString . '\)\@!\|[^' . firstChar . ']\|$\)/-1s/^' . commentString . "//"
		endif
	  endif
	endfunction
	```

	```
	function! Komment()
	  if getline(".") =~ '\/\*'
		let hls=@/
		s/^\/\*//
		s/*\/$//
		let @/=hls
	  else
		let hls=@/
		s/^/\/*/
		s/$/*\//
		let @/=hls
	  endif
	endfunction
	map k :call Komment()<CR>
	```





