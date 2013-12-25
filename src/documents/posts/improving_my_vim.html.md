```
title: Improving my Vim skills
layout: post
tags: ['vim', 'post']
```

I've always considered myself a fairly good vim user and recently I went looking for ways to improve my skills.  I found out that I've barely scratched the surface of vim and should totally be ashamed of myself.
If you're a vim, user these two talks are definitely worth watching:

   * [Ben Orenstein - Write code faster: expert-level vim](http://www.youtube.com/watch?v=SkdrYWhh-8s)
   * [Damian Conway - More Instantly Better Vim](http://www.youtube.com/watch?v=aHm36-na4-4)

I've also started going through Steve Losh's book: [Learn Vimscript the Hard Way](http://learnvimscriptthehardway.stevelosh.com/)

Something I really liked from Ben Orenstein's talk was where he mentioned having a sticky note on his desk with the couple of things that he's currently trying to turn into muscle memory and the couple of things that annoy him that need to be fixed later on.  I love that idea but I think we can do better by having them directly in vim!

## Quick edit files
I've now put these mappings in my vimrc:
```VimScript

"Quick edit file list
:nnoremap <Leader>ev :e ~/dotrc/vimrc<CR>
:nnoremap <Leader>sv :source ~/dotrc/vimrc<CR>
:nnoremap <Leader>es :e ~/dotrc/vimstickynotes.md<CR>
:nnoremap <Leader>ea :e ~/dotrc/vimannoyances.md<CR>

```
So now I can very quickly edit my vimrc file, my vim stickynotes file or my vim annoyances file by just pressing &lt;Space&gt;ev, &lt;Space&gt;es or &lt;Space&gt;ea. 

Space is my leader key and the mnemonics stand for: [e]dit [v]imrc, [e]dit [s]tickynotes and [e]dit [a]nnoyances.

(As a sidenote, the ~/dotrc directory is my git repo for my dotfiles.  ~/.vimrc is a symlink to ~/dotrc/vimrc.  My dotrc repo is up on github here: https://github.com/AshyIsMe/dotrc )

The next thing I'd like to do is turn the statusline to always be on and have it display a random line from vimstickynotes.md in the middle so I get reminded of them all the time. 

## Speeding up hard to reach keys
Another thing I've been experimenting with is the [Arpeggio](http://www.vim.org/scripts/script.php?script_id=2425) plugin for vim.  This allows you to define keyboard chords so that pressing 2 or more keys at the same time will register as whatever mapping you decide.  So far I've mapped the left and right index finger to be &lt;Esc&gt; and then some mappings for all the brackets: () [] {}.  

The &lt;Esc&gt; mapping seems to be working fairly well but unfortunately the mappings for ( and ) correspond to eu and th on the dvorak keboard layout.  "th" is used a surprising amount in english so I'm going to have to rethink what I map these two characters to.  

The mappings for the other brackets are working out fairly well though.

This is what the mappings look like with my layout:
<img src="/images/keychords-dvorak.png" class="img-responsive">

Vim is a tool of neverending improvement.  Push yourself to learn something about it every day you use it!
