---
title: "An adequate neovim config"
date: 13/02/2025
categories:
  - blog
tags:
  - neovim
  - config
  - lua
  - vim
---

![neovim perennial](/assets/images/nvim_perennial.jpeg)

**tl;dr**: Neovim has come a long way from an experimental vim fork that it was at inception and is veritably, in my opinion, the hotbed for open source editor innovation currently. However customising it can feel daunting especially since things are changing rapidly.

Having lived through [vi vs emacs](https://en.wikipedia.org/wiki/Editor_war) I was a full on emacs person jumping through my ocaml code with the venerable [tuareg mode](https://github.com/ocaml/tuareg) which is loved and maintained to this day. At that time, vim support for ocaml didn't exist, thus emacs came out on top for me.

After, I tried to setup emacs for javascript / css / web development and everything was as smooth as ever. However I got tired of the chord keybindings and I started using (evil/viper) mode and eventually switched to vim. Meanwhile vimscript was off putting for me on top of the immense configuration options that vim comes with. All of this was solved when lua was introduced as first class config language in neovim, and since then I've never looked back. At this point my editor config is quite stable and integral part of my workflow.

> Perfect is the enemy of good - Voltaire

For me, my text editor needs just the basic set up so that I can get productive with it from day one. For me they are

##### Load time

First and foremost, the most off-putting thing about IDEs of yore(Eclipse) and IDEs of today(Jetbrains etc) is the load time. Although it's very much subjective what is high, it can never match up to neovim's <100ms that I get with minimum optimisation.

![load time](/assets/images/nvim_load_time.png)

The max load time for me is 500ms which is still 1/3 of pycharm community edition(~1.5s) on my system.

##### Big files

Big files aka unwanted surprises. Imagine a text editor that can't open text files. A lot of editors have failed this benchmark not too far back in the past. I recall Github's now abandoned Atom that would [crash for files bigger than 20MB](https://github.com/atom/atom/issues/12049).

Not neovim though. I've tested and opened files as big as 1GB and everything in the editor stays stable although the load time could be significant (~10s).

##### Stability

`vim` has traditionally been a stable software, and carrying this into neovim must not have been an easy task for neovim contributors and maintainers, especially when they were trying to improve upon the codebase to ease contributions. But they've done a great job by being laser focused on the core mission which was to ease of contribution. (Personally I've been waiting for [this](https://github.com/neovim/neovim/issues/3681#event-15685900265) to be merged for a decade seems like but it only recently found its way into v0.11 after it was put on low priority since inception. Much discipline.) First class lua support can be a boon or a curse the way you look at it. For me it works really well as lua is an approachable little language that's understood by tools I use all over (say awesomewm, hammerspoon,). One can even use lua as a scripting language with nginx to create dynamic responses.

Checking all these boxes at once is rare but the team as quite deftly handled the transition. Because of this uber reliability and stability and ease of configuration, the community members have created a wide variety of distributions that decisively turn neovim to a full fledged ide.

##### lazyvim

Although there are many awesome and older config distributions like [astrovim](https://github.com/astro/astrovim) / [nvchad](https://github.com/nvchad/nvchad), I prefer [lazyvim](https://lazyvim.org). It's a comprehensive plugin management system that gives you exactly what you need and can save hours of configuration hell. It's highly approachable and my configuration builds on top of it. Props to [folke](https://github.com/folke) for creating some awesome plugins and now this distro that he maintains with a passion.

##### Configuration

One of the more controversial aspects of nvim, the achilles' heel of productivity, the bottomless pit that some people find themselves in. Particularly I didn't think much about my time spent on configuration until it was already too late. Yet I'm glad to have found this balanced config such that the editor just gets out of the way. On top of the afore mentioned minimum features, my editor also does the following with Python/Web:

- Tight integration with git with inbuilt mergetool
- Task runner (with overseer.nvim)
- Tightly integrated testing (with neotest)
- Debugger support
- Automatic annotations
- Opt in AI code completion support
- Inline markdown preview(with markview.nvim)
- And many other

Check out my full config [here](https://github.com/navxio/neode)

##### Conclusion

The neovim community is a welcoming, dynamic and helpful one, which has led to a lot of innovation in the editor / ide space. Plugins like [markview.nvim](https://github.com/oxy2dev/markview.nvim) (inline markdown preview), [hardtime.nvim](https://github.com/m4xshen/hardtime.nvim) (helps you get used to healthy vim motions), [minty](https://github.com/nvchad/minty) (modern
color tools) further improve one's experience with neovim as the editor and are constantly pushing the boundaries of what neovim can do. Which is why I've taken to calling my particular configuration `neo development environment` in place of ide. Although plugins / neovim core are quite stable at this point, I believe the best of neovim is yet to come.
