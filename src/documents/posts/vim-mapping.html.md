```
title: Vim mapping for git grep
layout: post
tags: ['vim', 'post']
```

Today I decided to finally clean up a vim leader mapping that has been working 
but causing some redraw issues for me for quite a while.

The mapping is for the very useful Ggrep command from Time Pope's awesome 
[fugitive](https://github.com/tpope/vim-fugitive) plugin.
```
nnoremap <Leader>gg :silent Ggrep  <bar> cw<Left><Left><Left><Left><Left>
```
The idea here is that I can hit &lt;leader&gt;gg and I will land in the commandline
ready to type whatever word I want to git grep the current repo for.

This mapping works fairly well but there's a redraw issue that means I have to
hit Ctrl-l to redraw the screen.

<a href="/images/redrawissue.gif">
  <img src="/images/redrawissue.gif" class="img-responsive">
</a>


I put up with that for a while and then finally asked on the #vim channel on
freenode how to fix it.
```
nnoremap <Leader>gg :silent Ggrep  <bar> cw <bar> redraw!<Left><Left><Left><Left><Left><Left><Left><Left><Left><Left><Left><Left><Left><Left><Left>
```

Now I have a redraw call on the end of the mapping which fixes the screen 
straight after running the command.
This works fine but this is becoming a damned ugly mapping.

With some further help from the #vim channel we (they) came up with this:
```
command! -nargs=1 G silent Ggrep <args> | cwindow | redraw!
nnoremap <leader>gg :G 
```
This one sets up a command and passes the arguments through without needing the
hundreds of &lt;Left&gt; tags.

