<!--
.. title: Gitignore windows cmd util
.. slug: gitignore-windows-cmd-util
.. date: 2025-06-22 13:47:35 UTC+02:00
.. tags: git,gitignore,windows,linux,command-line,cmd,zsh,bash
.. category: git
.. link: 
.. description: 
.. type: text
-->

Simple utility that lets you generate .gitignore using cmd/shell.
You must have curl in your path.

- windows/cmd: `gitignore.cmd`

```cmd
@echo off
curl -sL https://www.toptal.com/developers/gitignore/api/%*
```

- linux - add function into your aliases file or .*rc
  * function for alias
```shell
function gitignore() { curl -sL https://www.toptal.com/developers/gitignore/api/\$@ ;}
```

  * zsh - code to run to add function to `.zshrc`
```shell
echo "function gitignore() { curl -sL https://www.toptal.com/developers/gitignore/api/\$@ ;}" >> ~/.zshrc
```

  * bash - code to run to add function to  `.bash`
```shell
echo "function gitignore() { curl -sL https://www.toptal.com/developers/gitignore/api/\$@ ;}" >> ~/.bashrc
```

and than you can type:

```shell
gitignore python,pycharm,django
```

