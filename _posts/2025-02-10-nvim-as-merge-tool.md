---
title: "Resolve merge conflicts like a pro with nvim"
date: 10/02/2025
categories:
  - blog
tags:
  - merge
  - git
  - nvim
---

![diffview screenshot](/assets/img/diffview.png)

Over the years, I've gone through so many merge tools - kdiff3, meld, beyond compare, Kaleidoscope. Nvim being my editor of choice, fortunately, has a rich ecosystem of plugins where I can set up an uber merge workflow using `diffview.nvim` and avoid using external tools altogether.

##### Getting started

Make sure you have the latest nvim installed (I have v0.10.4). Although I use [lazyvim](https://lazyvim.org) as the base to provide the basic (and some advanced) editor features, I have to set recourse to _diffview.nvim_ to ease my merging process.

The first and only step is to edit the .gitconfig file to add these options

```
[merge]
  tool =nvim-diffview
[mergetool "nvim-diffview"]
  cmd = "nvim -c 'DiffviewOpen'"
  prompt = false
```

Et voila. Now each merge conflict that opens automatically in a diff view with simple keybindings, a brief overview of which is provided below:

- `[x , ]x` jump to different previous / next conflict marker
- `<leader>co`: Choose the OURS version of the conflict.
- `<leader>ct`: Choose the THEIRS version of the conflict.
- `<leader>cb`: Choose the BASE version of the conflict.
- `<leader>ca`: Choose all versions of the conflict (effectively
  just deletes the markers, leaving all the content).
- `dx`: Choose none of the versions of the conflict (delete the
  conflict region).

As usual, you can just `:h diffview` anytime you need to refer to some config / keybinding.

You can also check out my full `.gitconfig` [here](https://gist.github.com/navxio/784c58ef351551b139b78fb1e775bf55) and my [full neovim config](https://github.com/navxio/neode)
