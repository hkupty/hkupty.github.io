---
title: "Ditching tmux"
date: 2016-03-13 10:55
categories: [tools]
tags: [neovim, tmux]
layout: single
---
This week I started an experiment. I wanted to know whether I would be able to completely ditch tmux.

Don't get me wrong here, I absolutely love how tmux makes terminal multiplexing transparent for me. I'm also very used to `<C-b>c`, `<C-b>1-0` and so on. I also have a custom set of [keymaps and plugins][keymaps] I use frequently, such as [tmux-copycat][tmux-copycat], which helps me find paths or urls on the terminal.

With that said, I found that my workflow was not optimal, a certain day, where I was constantly switching tabs on neovim and panes on tmux. This was unlikely the best scenario and I challenged me to overcome this by runnig neovim _as my terminal multiplexer_.

In case you don't know [neovim][neovim] yet, it is an awesome fork of vim with several enhancements, such as built-in terminal emulator and asynchronous jobs.

Defeating Muscle Memory
-----------------------
Well, some times you get way too much used to some keymaps. This is the case. I use `<C-b>` for everything. Even on plain terminal, I use `<C-b><C-l>` (custom bind) for clearing the screen.

So my first step to remove tmux out of my way was to bind several of the keybinds I'm used to in neovim. And I came up with [nvimux][nvimux]. This is just a fancy name for 'tmux bindings on neovim'. It is very simple and has almost no fancy stuff, such as configurable keybinds, conditional mappings and so on. It just enables tmux bindings if tmux is off. Feel free to use if you get interested.

Overcoming the limitations
--------------------------
Now that I can `<C-b>c` instead of `:tabe` and `<C-b>1` instead of `1gt` (which I normally use, but I used to navigate tmux's panes way more often), its time to handle the limitations.

The first thing is editing files from the shell. While on tmux, I normally typed `vim path/to/some/file` to edit it. This was fine inside tmux but from within neovim's terminal, this would cause a nested vim session, which is not what I wanted.

So I found an excellent tool called [neovim-remote][nvr]. It is fantastic. I have written a small wrapper around it on my `.zshrc`, but just for faster typing:

```bash
if [ -n "${NVIM_LISTEN_ADDRESS+x}" ]; then
  alias h='nvr -o'
  alias v='nvr -O'
  alias t='nvr --remote-tab'
fi
```

Now, from inside neovim I can type `t path/to/some/file` to open it on another tab or replace `t` with `h` to open horizontally (or `v` for vertically) on the same tab I'm now.

Another plugin that helped me deeply was [neoterm][neoterm]. I mapped `<C-b>q` to be my quick terminal and I can open the same instance on every screen. It just wraps neovim terminal in an easy and reusable way.

//TODO
------
I'm still adjusting all the stuff, so there are several things I miss or need to improve my usage.

  * Tab-local pwd. As of writing this arcticle, the feature is still [a pending PR on neovim][neovim-pr].
  * Toggling a specific terminal instance on neoterm. Neoterm only toggles the last instance, which makes it weird if I have two term instances open.
  * Better buffer management for nvimux. I wish I could open a terminal and jump right into it, but instead I have to `<C-w>l<CR>` every time to navigate and enter on terminal mode.

Other than that, I has been a quite interesting week and I feel I'm making a better usage of other tools such as `vim-fugitive` instead of `git` directly on command line. As for productivity I think it is still too early to say.

__Edit:__ As this post has made some discussion happen on [hackernews][hn], I've added some more information about it on [this post][next-post]

:x

[keymaps]:      https://github.com/hkupty/dotfiles/blob/master/tmux/.tmux.conf
[tmux-copycat]: https://github.com/tmux-plugins/tmux-copycat
[neovim]:       https://github.com/neovim/neovim
[nvimux]:       https://github.com/hkupty/nvimux
[nvr]:          https://github.com/mhinz/neovim-remote
[neoterm]:      https://github.com/kassio/neoterm
[neovim-pr]:    https://github.com/neovim/neovim/pull/3229
[hn]:           https://news.ycombinator.com/item?id=11283955
[next-post]:    http://hkupty.github.io/2016/A-Neovim-Approach/
