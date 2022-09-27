---
title: "Speeding up my zsh prompt: Part 2"
categories:
  - Blog
tags:
  - computer-science
  - cs
  - programming
  - command-line
  - zsh
---

In the previous [post](https://nav.wiki/blog/zsh-speed-rush-part1) I profiled my zsh things to debug the monumental shell load time which is really annoying for me, since I spend all my time in the shell.

I think I'm gonna get rid of `compinit` totally and look at the profiling data. Let's dive in

```
#  autoload -Uz compinit
#  if [[ -n ${HOME}/.zcompdump(#qN.mh+24) ]]; then
	#  compinit;
#  else
	#  compinit -C;
#  fi;

```

Launching the profiler now, the results are-

```
num  calls                time                       self            name
-----------------------------------------------------------------------------------
 1)    3          48.43    16.14   33.28%     26.07     8.69   17.92%  compinit
 2)    4          22.36     5.59   15.36%     22.36     5.59   15.36%  compaudit
 3)    1          12.94    12.94    8.89%     12.94    12.94    8.89%  __zplug::utils::awk::available
 4)    1          12.32    12.32    8.46%     12.27    12.27    8.43%  __zplug::base::base::git_version
 5)    6          13.51     2.25    9.29%      9.35     1.56    6.42%  __zplug::core::load::as_plugin
 6)    1          15.70    15.70   10.79%      7.92     7.92    5.45%  __check__
 7)    5           7.34     1.47    5.04%      7.34     1.47    5.04%  __zplug::sources::github::check
 8)    5           8.29     1.66    5.70%      6.61     1.32    4.54%  __zplug::core::sources::call
 9)    1          48.17    48.17   33.11%      6.59     6.59    4.53%  __zplug::core::core::prepare
10)    5          12.58     2.52    8.65%      4.29     0.86    2.95%  __zplug::core::sources::use_default
11)    3           5.50     1.83    3.78%      4.26     1.42    2.93%  __zplug::core::core::get_interfaces
12)    5          16.01     3.20   11.00%      3.39     0.68    2.33%  __zplug::core::add::to_zplugs
13)    1           2.18     2.18    1.50%      2.18     2.18    1.50%  __zplug::core::cache::diff
14)    6           2.01     0.33    1.38%      2.01     0.33    1.38%  github.zsh
15)    1           7.26     7.26    4.99%      1.74     1.74    1.20%  __zplug::core::core::variable
16)    7           8.54     1.22    5.87%      1.55     0.22    1.06%  __zplug::base
17)    1           1.41     1.41    0.97%      1.41     1.41    0.97%  async_init
```

Looks like the calls to compinit have reduced by 1. Can't tell if the shell feels snappy or not.
Launching bash based time evaluator again:
`time zsh -i -c exit`

```
real	0m0.831s
user	0m0.521s
sys	0m0.267s
```

Looks like it increased? I think i'm gonna ditch this bash based benchmarks for the time being.

The shell does feel snappier and looks like the completions work fine too. So I guess that's enough for
me for the time being. One of these days I'm gonna get dissatisfied with the current load time and
continue again on this path. Until then, I'm sorted.

BTW you can look at my dotfiles [here](https://github.com/navxio/dots)
