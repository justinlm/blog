---
title: win下删除Git中缓存的用户名和密码
date: 2018/8/30
tag: Git
---
虽然这个功能不是那么常用，但是如果突然需要我们要用另一个账户登录在当前电脑登录git的时候，那就有用了，我们需要清掉原来的账户才行。

## 那么就开始我们的表演

### [Git credential](https://git-scm.com/docs/gitcredentials)
(⊙o⊙)…，我们只要了解：gitcredentials为Git提供用户名和密码功能。
以windows为例：
##### 1.告诉Git当前使用的是windows系统
```
git config --global credential.helper wincred
```
##### 2.卸载credential-manager
```
git credential-manager uninstall
```
大功告成，Git中缓存的用户名和密码已经被清掉了。