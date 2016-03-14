---
title: "A Neovim Approach"
date: 2016-03-14 20:25
categories: [tools]
tags: [neovim, tmux]
---

Still on the topic of Neovim taking over tmux's place, and as a response to several interesting comments I've read on hackernews and reddit regarding [previous post][ditching-tmux], I'd like to show a new approach to what has been discussed.

Why not Emacs?
--------------

With all due respect to Emacs, it is simply a matter of personal taste. I won't start a flamewar here; on the contrary. I do enforce people to try Emacs, as it is a very personal aspect of how you feel confortable to interact with code, scripts.

I value, however, the knowledge I've already gathered. Muscle memory strikes me whenever I'm writing anything outside of vim/neovim. Several plugins I've been using for so long that feel just as vim for me. There is no need for me to learn `Magit` now if I'm still very productive using 'vim-fugitive'.

Why ditch tmux?
---------------

Several people stated that tmux does attach/detach and remote/ssh session. Well, I don't normally ssh into servers, but still, should be no problem to open a tmux or even a screen session from within neovim's terminal. As for attach/detach, `<C-z>` and `fg` suffice for me.

I started to think about taking tmux out of my stack recently because:

  * It made more complex to manage clipboard (specially on mac, which I use at work);
  * `<C-b>n`, `<C-b>p`, `gt`, `gT` and variations started to come on the way if I had several tabs and several panes open;
    * Specially confusing if multitasking;
    * Had to swtich over several times to check compilation/tests;
  * Another dotfile to manage, another config to tweak, another app to provision in order to start working.

A Neovim Approach
-----------------

Thanks to asynchronous support on neovim, I started having jobs running on background ([neomake][neomake], for exemple). This means I can have _less panes open doing the same amount of work._

I can have [terminal buffers][terminal-buffers] within neovim, so I don't need to keep a tmux pane open to bring me a shell.
I can have [remote plugins][remote-plugins] interacting asynchronously with neovim, so the editor does not freeze.

I don't need to have several tmux panes opened at the same time _to have the same (or even better) productivity now with neovim._ And that is what this is all about: Productivity.
Taking everything that reduces your productivity is a recycling process that you should do often. If you are getting off the track, the sooner you realise it and the soyour you fixe it, the better.

I probably have read it somewhere, but it is very clear to me that we should be very familiar with the tools we use daily. Neither the editor nor any other tool (shell, terminal multiplexer, terminal application/gui) should be unfamiliar to the point it hurts your ability to code (or whatever else you must do).

The thing is, _neovim has maked unnecessary the use of a terminal multiplexer for my development workflow_ and it surely might do that to others.

A Final Note
------------

Overall, I'm very happy with the repercussion of my previous post. I'll not be answering on posts like this one, but as I understand that talking about editors normally starts flamewars, I just wanted to make things clearer and potentially help people to understand more and make better use of currently available tools, which is my ultimate goal with this blog.

If you happen to enjoy the idea of my [previous post][ditching-tmux], consider using and contributing with [nvimux][nvimux].

:x

[ditching-tmux]:       http://hkupty.github.io/2016/Ditching-TMUX/
[neomake]:             https://github.com/benekastah/neomake
[terminal-buffers]:    https://neovim.io/doc/user/nvim_terminal_emulator.html
[remote-plugins]:      https://neovim.io/doc/user/remote_plugin.html
[nvimux]:              https://github.com/hkupty/nvimux
