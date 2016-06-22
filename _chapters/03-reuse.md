---
layout: chapter
title: Reuse / 复用
section: Background
permalink: /chapters/reuse/
description: Learn why avoding reuse and embracing repetition makes CSS maintenance easier.
---

**摘要:** 不要尝试复用样式. 优先采用删除重复内容.

> &ldquo;DRY(Don't repeat yourself / 不要重复自己)常被误解为永远不要重复做同样的事情[…]. 这是不切实际的，通常适得其反， 并可能导致强制抽象、过度思考和过度设计的代码.&ldquo;
<br>&mdash; <cite>Harry Roberts, CSS Wizardy</cite>

不要采取这种错误的方式&mdash;MaintainableCSS 具备可复用性，我将在以后讨论它. 问题是尝试重用花括号之间的代码是有问题的. 原因如下:

## 因为样式随着视图改变

建立响应式的网站时，我们添加的样式是基于不同的窗口尺寸的. 假设我们要构建一个两列网格:

- 大屏幕和小屏幕下padding属性值分别为50px和20px.
- 大屏幕和小屏幕下font-size属性值分别为3em and 2em.
- 小屏幕下， 每一列都堆放在上一列的下面。注：「列」现在是误导.

考虑到这一点， 你怎么使用这些原子类： `.grid`, `.col`, `.pd50`, `.pd20`, `.fs2` and `fs3` 实现以上需求?

  <div class="grid">
    <div class="col pd50 pd20 fs3 fs2">Column 1</div>
    <div class="col pd50 pd20 fs3 fs2">Column 2</div>
  </div>

你可以看到这恰恰是行不通的. 为了实现需求，你需要一些疯狂的类名，比如 `fs3large`. 这只涉及到维护问题的冰山一角.

或者，试试下面没有复用样式的语义化标记:

  <div class="someModule">
    <div class="someModule-someComponent"></div>
    <div class="someModule-someOtherComponent"></div>
  </div>

如果按照上面的方式添加样式, 现在这个简单的任务总共声明了6个CSS类, 其中3个应用在Media Query.

## 因为样式随着状态改变.

如何让 `<a class="padding-left-20 red" href="#"></a>` 在触摸或者获取焦点时样式发生以下的改变：padding值变为18px，添加一个浅色的边框，背景变为灰色，红色文字稍加阴影 （动作如 `:hover`,`:focus`, `:active` 等）?

简短的答案是不行。尽量避免去修复自找的问题.

## 因为复用让调试更困难.

当调试一个元素时，会有一些起作用的CSS选择器使调试变得复杂.

## Because granular styles aren't worth bothering with.

If you're going to do `<div class="red">` you may as well do `<div style="color: red">` which is more explicit anyway. But we don't want to do this because we don't want to mix concerns.

## Because visual class names don't hold much meaning.

Take `red`. Does this mean a red background? Does this mean red text? Does this mean a red gradient? What tint of red does this mean?

## Because updating a "utility" class applies to all instances.

This sounds good but it isn't. You end up applying changes where you didn't mean to. Think regression. Alternatively, you end up scared to touch this utility class so you end up with `.red2`. Then you end up with redundant code. Obviously this is not fun to maintain.

## Because non-semantic class names are hard to find.

If an element has classes based on how it looks such as `.red`, `.col-lg-4` and `.large`, then these classes will be scattered all over the codebase so searching for "red" will yield many results across the HTML templates, making it hard to find the element in question.

If you use semantic class names, a search should yield just one result. And if it yields more than one result, then this should indicate a problem that needs dealing with.

Note: if you have a repeated *component* within a module, then searching might yield several results within 1 file. That is, a module would typically live in a single template.

## Because reuse causes bloat.

If you attempt to reuse every single *rule* you'll end up with classes such as: `red`, `clearfix`, `pull-left`, `grid` which leads to HTML bloat:

  <div class="clearfix pull-left red etc">

Bloat makes it harder to maintain and degrades performance (albeit in a minor way).

## Because reuse breaks semantics.

If you strive to reuse the bits inbetween the curly braces to create "atomic" class names, then you encounter all the problems stated in the chapter about [Semantics](/chapters/semantics/). Read that chapter now, if you haven't already.

## What if I really want to reuse a style?

It is extremely rare, but there are times when it really does make sense to reuse a style. In this case use the comma in your selectors and place it in a well named file.

For example let's say you wanted a bunch of different modules or components to have red text you might do this:

  /* colours.css */

  /* red text */
  .someSelector,
  .someOtherSelector {
    color: red;
  }

Remember though that if any selector deviates, even a little bit, then remove it from the common list and duplicate. You must be very careful with something like this. Do it for convenience, not for performance. Your mileage may vary.

## What about mixins?

CSS preprocessors allow you to create mixins which can be really helpful because they provide the best of both worlds but they should be designed with caution.

You have to be very careful to update a mixin because it propagates in all instances just like utility classes. It can be problematic to edit and instead you create new mixins that are slightly different causing redundancy and maintenance problems.

Also, mixins can become very complicated with lots of params, conditionality and large declarations of styles. All of this makes it complicated and complicated is hard to maintain.

To mitigate, you can make mixins really granular. For example you could have a "red text" mixin which is certainly better. But then again, the declaration of that mixin is basically the same as a declaration of red text&mdash;might as well just declare that instead.

If you need to update it in multiple places, then a search and replace might just do it, depending on your context. And even if you did use a mixin, when red changes to orange, you will have to do a search and replace anyway, because the mixin name will otherwise be misleading.

This does not mean mixins are "bad"&mdash;in fact they can be very helpful. For example, being able to apply *clearfix* rules across different elements at different breakpoints is probably a very helpful thing to do for maintainability. Just make sure to use them with care.

## What about performance?

I don't have stats to hand, but I can very confidently say that it's not wise to practice premature optimisation. Let's say you have a very large CSS codebase (100KB or more).

Firstly, I *guess* you *might* save yourself a few KB. Secondly, there are alternative paths to improving performance and thirdly, you probably have redundant code due to a lack of maintainability.

Also consider that one or two images are likely to be far larger than the entire CSS so exerting energy here is probably of little value.

## Is this violating DRY principles?

Attempting to reuse `float: left` as an example, is akin to trying to reuse variable names in different Javascript objects. It's simply not in violation of DRY.

## Final thoughts on reuse

Reuse and DRY are such important principles in software engineering but when it comes to CSS, striving to reuse too much ironically makes maintainance *harder*. However, there are times when reuse and abstraction makes sense which is discussed in later chapters.
