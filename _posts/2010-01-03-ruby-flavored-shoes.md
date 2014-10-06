---
title: Ruby-flavored shoes
redirect_to:
  - http://blog.imanel.org/ruby-flavored-shoes
---

Recently, working on my BSc thesis, I had to choose programming language and some framework to create graphical interface. The first thing that came to my mind was C++ and QT library. But that would be too easy :). It's time to learn something new. Something connected with Ruby (cause that's the language I currently work with). There's a couple of nice bindings for QT in Ruby, but all of them seemed not enough "ruby-way" to me. Most of them didn't comfort to some of my needs (multi-platform, should work OOTB, with acceptable performance) too. Finally I found really interesting framework, namely, Ruby Shoes.

After quick installation on OS X, virtualised Windows and Ubuntu it is time for our beloved "hello world" test. Next, few more advanced test. Everything works great. Applications work on all three platforms,  contain Ruby Shoes (statically linked), so there's no need for any dependencies installation. The only problem is weak support for gems containing native extensions. This especially shows with RMagick. It compiles and starts but as soon as you try to call any function it fails with message that some (existing) bundle doesn't exist. You should also be careful when searching for documentation - not every function is described in RDoc. If you can't find one then try to look into manual or google it.
