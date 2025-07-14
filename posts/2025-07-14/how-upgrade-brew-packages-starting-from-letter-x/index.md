<!--
.. title: how upgrade brew packages starting from letter X
.. slug: how-upgrade-brew-packages-starting-from-letter-x
.. date: 2025-07-14 16:42:01 UTC+02:00
.. tags: brew,linux,shell,bash,zsh
.. category: brew
.. link: 
.. description: 
.. type: text
-->

How upgrade brew packages starting from letter x.
Run this command:
```shell
brew upgrade $(brew list | grep '^x')
```
