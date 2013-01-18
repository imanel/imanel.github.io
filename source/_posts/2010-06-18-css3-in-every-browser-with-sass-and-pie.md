---
layout: post
title: CSS3 in every browser with SASS and PIE
---

Recently, more and more popular is the use of the HTML5 and CSS3. Everyone rushed to them and this is perfectly understandable - after all, developers will be able to bet on semantic instead of doing work arounds for some browsers. But the problem is that at the moment even the "modern" browsers have problems handling some common attributes. The most common example is the border-radius, which has up to three versions - so if we want all browsers display the same rounding, we need to write this as:

``` css
#round_me {
  -moz-border-radius: 1em;
  -webkit-border-radius: 1em;
  border-radius: 1em;
}
```

And this isn't "uniformity", but hopefully that for some time only one of them will be needed. At the moment we need to put the whole set every time. And we often forget - as the example [GitHub](http://github.com) who sometimes remember and sometimes not to use "border-radius."

Fortunately, if you use the help such as [SASS](http://sass-lang.com) (available for many programming languages) then the problem disappears. This framework allows us to define the "mixins" which then can be attached anywhere without the need to rewrite the same code several times (according to the [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself) mantra) The easiest way would be to show how:

``` scss
@mixin rounded-corners($size) {
  -moz-border-radius: $size;
  -webkit-border-radius: $size;
  border-radius: $size;
}

#round_me {
  @include rounded-corners(1em);
}
```

Mixins themselves are not rendered, but only attached to the other classes so the end user code will be as in the first example.

Some ask why not use the class instead of id - the answer is simple: the separation of HTML and CSS. Because if one day you'll want to get rid of the rounded corners then with one is easier: to remove the entry from the appropriate id or search through all the HTML pages and remove the class?

There remains the problem of one browser - namely Internet Explorer. It does not support the CSS3 specification and at the moment is still very popular. Thats why there are libraries like the [PIE](http://css3pie.com) - to facilitate this. For the above border-radius example(and many others) it require only to attach a .htc file and attribute will work well in IE6, IE7 and IE8. The specification requires, however, to attach it to every class in which CSS is used - so our definition this time will look like this:

``` css
#round_me {
  -moz-border-radius: 1em;
  -webkit-border-radius: 1em;
  border-radius: 1em;
  behavior: url(PIE.htc);
}
```

This is another line of which you must remember. And this is the best place where you can see the beauty of SASS - just add this line in one place and will work everywhere. Thus, the final version of the code will look like this:

``` scss
@mixin rounded-corners($size) {
  -moz-border-radius: $size;
  -webkit-border-radius: $size;
  border-radius: $size;
  behavior: url(PIE.htc);
}

#round_me {
  @include rounded-corners(1em);
}
```

I hope that this solution will help you to create beautiful and semantic pages a lot easier.
