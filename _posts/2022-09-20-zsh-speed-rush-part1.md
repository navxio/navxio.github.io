---
title: "Speeding up my zsh prompt: Part 1"
categories:
  - Blog
tags:
  - computer-science
  - cs
  - programming
  - command-line
  - zsh
---

I'm a big fan of the `z shell`. I use it with the [spaceship prompt](https://github.com/spaceship-prompt/spaceship-prompt).
I use [zplug](https://github.com/zplug/zplug) for plugin management.

Right now, it takes an uncomfortable amount of time to load up the prompt on terminal emulator launch, which is annoying for me since I like everything to be snappy, at least on the command line.

#### Overview of my zshrc

It's pretty basic, boasts of a `zplug` installation for plugin management
and some attendant plugins.

{% gist 8c44876f561517edbd5d2c722ddc47fc %}

The culprit could be any one of those.

#### Diagnosing

The first step in any speed based diagnoses is profiling. I need to convert the
subjective speed rush into tangible numbers and then try to reduce the load time
by an order of magnitude

Using `bash` inbuilt utility `time`

`time zsh -i -c exit`

```
zsh -i -c exit  0.17s user 0.17s system 86% cpu 0.402 total
```

It returned 0.17 seconds, and a long 0.17 seconds it is.

This happens when the said command is launched from zsh itself. Trying to launch it from `bash`

```
real	0m0.892s
user	0m0.579s
sys	0m0.278s
```

user time is 0.578s which is a lot again. Apparently it's even higher because zsh takes time to load the first time.
Debugging it is gonna be interesting.

#### Profiling

zsh contains a profiling tool called `zprof` bundled. Let's figure
out how to inject it.

You just use it this way it appears in my zshrc:

`zmodload zsh/zprof`

\<rest of my zshrc\>

`zprof`

Launched zshell and here's an extract from the output:

```
num  calls                time                       self            name
-----------------------------------------------------------------------------------
 1)    2         346.23   173.12   52.57%    346.23   173.12   52.57%  compdump
 2)    4         574.15   143.54   87.17%    116.10    29.02   17.63%  compinit
 3) 1825          75.05     0.04   11.39%     75.05     0.04   11.39%  compdef
 4)    6          36.77     6.13    5.58%     36.77     6.13    5.58%  compaudit
 5)    1          12.63    12.63    1.92%     12.58    12.58    1.91%  __zplug::base::base::git_version
 6)    1           9.53     9.53    1.45%      9.53     9.53    1.45%  __zplug::utils::awk::available
 7)    4          10.50     2.62    1.59%      7.63     1.91    1.16%  __zplug::core::load::as_plugin
 8)    1          12.50    12.50    1.90%      6.27     6.27    0.95%  __check__
 9)    4           5.87     1.47    0.89%      5.87     1.47    0.89%  __zplug::sources::github::check
10)    1          43.06    43.06    6.54%      5.24     5.24    0.80%  __zplug::core::core::prepare
11)    4           6.49     1.62    0.98%      5.15     1.29    0.78%  __zplug::core::sources::call
12)    2           4.65     2.32    0.71%      4.65     2.32    0.71%  __zplug::io::file::rm_touch
13)    4           9.82     2.46    1.49%      3.34     0.83    0.51%  __zplug::core::sources::use_default
14)    3           4.29     1.43    0.65%      3.16     1.05    0.48%  __zplug::core::core::get_interfaces
15)    4          12.40     3.10    1.88%      2.55     0.64    0.39%  __zplug::core::add::to_zplugs
16)    4           2.39     0.60    0.36%      2.39     0.60    0.36%  __zplug::job::handle::flock
17)    1           1.90     1.90    0.29%      1.90     1.90    0.29%  __zplug::core::cache::diff
18)    5           1.69     0.34    0.26%      1.69     0.34    0.26%  github.zsh
19)    7           7.45     1.06    1.13%      1.57     0.22    0.24%  __zplug::base
20)    1           5.82     5.82    0.88%      1.51     1.51    0.23%  __zplug::core::core::variable
21)   29           1.13     0.04    0.17%      1.13     0.04    0.17%  regexp-replace
22)    1           0.76     0.76    0.12%      0.76     0.76    0.12%  colors
23)    1           0.44     0.44    0.07%      0.44     0.44    0.07%  git.zsh
24)    1           0.44     0.44    0.07%      0.44     0.44    0.07%  handle.zsh
25)    1           0.39     0.39    0.06%      0.39     0.39    0.06%  core.zsh
26)    1          31.61    31.61    4.80%      0.36     0.36    0.05%  __zplug::core::load::from_cache
27)    1           0.34     0.34    0.05%      0.34     0.34    0.05%  theme.zsh
28)    6           0.28     0.05    0.04%      0.28     0.05    0.04%  add-zsh-hook
29)    1           0.28     0.28    0.04%      0.28     0.28    0.04%  cache.zsh
30)    1           0.27     0.27    0.04%      0.27     0.27    0.04%  load.zsh
31)    1           0.26     0.26    0.04%      0.26     0.26    0.04%  oh-my-zsh.zsh
32)    2           0.25     0.13    0.04%      0.25     0.13    0.04%  prezto.zsh
33)    1           0.23     0.23    0.04%      0.23     0.23    0.04%  shell.zsh
```

Looks like `compdump` and `compinit` are taking up most of the initialization time.
Don't exactly remember what they are.. gonna have to reinvestigate zshrc

This also gives me an opportunity to clean up my `zshrc`.

After a brief research I come across this gist that recommends checking a `.zcompdump` file
only once a day for improved performance.

{% gist ca1035271ad134841284 %}

After incorporating this to my `zshrc`, I try my tests again. And the results are in:

With `bash`:

```
real    0m0.449s
user    0m0.190s
sys     0m0.217s
```

Looks like the init time has almost halved for `user`. Good enough for me I guess.

However the profiling tells another story.

```
num  calls                time                       self            name
-----------------------------------------------------------------------------------
 1)    3         426.16   142.05   54.35%    426.16   142.05   54.35%  compdump
 2)    4         697.40   174.35   88.95%    137.28    34.32   17.51%  compinit
 3) 2312          99.99     0.04   12.75%     99.99     0.04   12.75%  compdef
 4)    6          34.01     5.67    4.34%     34.01     5.67    4.34%  compaudit
 5)    1          12.43    12.43    1.59%     12.39    12.39    1.58%  __zplug::base::base::git_version
 6)    1           8.87     8.87    1.13%      8.87     8.87    1.13%  __zplug::utils::awk::available
 7)    6          11.56     1.93    1.47%      7.94     1.32    1.01%  __zplug::core::load::as_plugin
 8)    1          15.24    15.24    1.94%      7.73     7.73    0.99%  __check__
 9)    5           7.10     1.42    0.91%      7.10     1.42    0.91%  __zplug::sources::github::check
10)    5           8.41     1.68    1.07%      6.70     1.34    0.85%  __zplug::core::sources::call
11)    1          42.80    42.80    5.46%      6.47     6.47    0.82%  __zplug::core::core::prepare
12)    5          12.70     2.54    1.62%      4.29     0.86    0.55%  __zplug::core::sources::use_default
13)    5          16.08     3.22    2.05%      3.34     0.67    0.43%  __zplug::core::add::to_zplugs
14)    3           4.41     1.47    0.56%      3.19     1.06    0.41%  __zplug::core::core::get_interfaces
15)    6           1.99     0.33    0.25%      1.99     0.33    0.25%  github.zsh
16)    1           1.97     1.97    0.25%      1.97     1.97    0.25%  __zplug::core::cache::diff
```

The time taken by `compinit` has increased from 574.15 ms to 697 ms and it doesn't feel snappy either.

The plot thickens. To be continued..
