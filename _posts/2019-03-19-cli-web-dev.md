---
title: "Command line tools for the productive web developer"
categories:
  - Blog
tags:
  - cli
  - command-line
  - webdev
  - programming
---

In my [previous post](https://nav.wiki/blog/cli-tools), we discussed some general purpose command line utilities that help ease your way around the command line. In this post we'll discuss some command line utilities that are indispensable to my web development workflow, and might come in handy for you too :)

#### pgcli/mycli

All web apps depend on a database of some kind. Thus, communicating with a database is one of the most common tasks a web developer has to perform. Although MySQL and PostgreSQL come bundled with their own CLI tools, there are options like pgcli/mycli which provide more powerful interfaces to the same databases, with features like auto completion, command history, syntax highlighting and many more.

[pgcli](https://pgcli.com)
[mycli](https://mycli.net)

#### live-server

Every once in a while, one has to deploy a static site locally to test out ideas(say fonts etc). Live-Server is my tool of choice in such cases. It supports live reload, and works without installing any browser plugins. So simple and minimalist.

[live-server](https://www.npmjs.com/package/live-server)

#### json-server

If you do frontend development with React/Angular, you probably need to intnegrate a REST API backend; which might not be ready from the get go. Enter json-server, a handy tool which allows you to spin up an API server from a JSON file. For eg. to define a endpoint, you just need a key in the json file by the same name. Quite simple, eh?

[json-server](https://www.npmjs.com/package/json-server)

#### httpie

If you develop REST APIs, sometimes you need a tool to manually test out the responses from the same. Although there are some GUI applications available like Postman/Insomnia, I rely on this beautiful and popular command line application. It has a simple syntax and sane defaults which make it quite easy to get started. For ex. making a `GET` request is as easy as

`http example.com`

It supports a tonne of options which are quite straightforward and easy to remember.

[httpie](https://httpie.org)

#### bpython/pry/quokka.js

If you program in an interpreted language, it's worthwhile to find a 'super' shell for it, that has more/better features than the native interpreter shell. For ex. there's [bpython](https://github.com/bpython/bpython) for Python, [pry](https://github.com/pry/pry) for Ruby etc. Some common features that they include are syntax completion, interactive history, syntax highlighting etc.

[Quokka.js](https://quokkajs.com) is a tool to test out an npm package without creating a new project. It's quite handy and there's also a [VS Code extension](https://medium.com/r/?url=https%3A%2F%2Fmarketplace.visualstudio.com%2Fitems%3FitemName%3DWallabyJs.quokka-vscode) available.

#### Snyk

Snyk allows you to do check for/fix security vulnerabilities in your app's dependencies. It's open source and available for a variety of platforms, such as Node.js/ Python/PHP etc. I use it regularly with my projects and highly recommend it.

[snyk](https://snyk.io)
