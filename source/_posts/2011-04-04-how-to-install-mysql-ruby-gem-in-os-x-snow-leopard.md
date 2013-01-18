---
layout: post
title: How to install MySQL Ruby gem in OS X Snow Leopard
comments: true
---

I know this was posted multiple times by other people, but this information is scattered and hard to find so I'm posting quick summary.

First of all you must download [MySQL binary](http://dev.mysql.com/downloads/mysql/5.1.html). Be aware that ruby gem will not work with version newer than 5.1, and in Snow Leopard you should use 64-bit version.

After installing it(it should land in /usr/local) you can start compiling mysql gem. In order to do so you need to specify arch flags and path to your mysql dir:

``` js
env ARCHFLAGS="-arch i386" gem install mysql -- \
  --with-mysql-dir=/usr/local/mysql \
  --with-mysql-lib=/usr/local/mysql/lib \
  --with-mysql-include=/usr/local/mysql/include
```

Note that if you are using rvm then you shouldn't install it in @global gemset - bundler will not see that and will try to recompile it again. So you will need to run this command for every gemset you will be using.
