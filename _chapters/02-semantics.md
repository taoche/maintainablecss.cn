---
layout: chapter
title: Semantics / 语义
section: Background
permalink: /chapters/semantics/
description: Why naming something based on what it is, instead of how it looks or behaves is a cornerstone of writing well architected and maintainable CSS code.
---

**概要:** 根据元素 *是什么* 来命名， 而不是它的 *外观* 或者 *行为*.

## 通俗的解释

HTML的语义化不仅仅是指HTML标签的语义化&mdash;链接用`<a>`标签，表格式数据用`<table>`标签来呈现，段落用`<p>`标签等等这些都是显而易见的。

除此之外，我们给HTML元素添加的类名（及ID选择器）也给CSS（及Javascript）提供了*额外的*钩子，从而帮助增强HTML的语义化。

给元素添加类名看上去是一件相当容易，不需要太动脑的事情， 而事实上命名非常的重要。

> &ldquo;计算机科学当中真正难的只有两件事： 缓存失效和变量命名&rdquo;
<br>&mdash; <cite>Phil Karlton</cite>

这是因为人类非常易于理解通俗的语言交流，而并不擅长于去理解缩略化、非语义化的抽象概念。

## 好/坏类名的例子

请试着找出以下例子语义化类名和非语义化类名的区别...

	<!-- bad -->
	<div class="red pull-left">
	<div class="grid row">
	<div class="col-xs-4">

根据以上`<div>`标签的几个类名的命名方法，我们顶多可以猜测出这几个元素的外观（以及适用于小屏幕或大屏幕），却完全看不出来它们表示什么样的HTML结构。

	<!-- good -->
	<div class="header">
	<div class="basket">
	<div class="product">
	<div class="searchResults">

而以上这几个的命名则非常直观地显示出它们是什么，据此，我们可以很确切地知道它们在HTML中代表了什么。这样的命名让我们对它们的外观一无所知&mdash;而这正是CSS真正应该做的事情。因此，当我们讲语义化的类名命名时，其实是包括了HTML和CSS（以及JS）的。

So **why** else should we use semantic class names?

## Because it's easier to understand.

Whether you're looking at the HTML or the CSS, you know what you're affecting. With visual class names you end up having to sprinkle several class names on to each element, ending up with a vague understanding of the intention of these visual class names. This is hard to maintain.

## Because we are building responsive websites.

Styles often need changing based on viewport size. For example, you might float elements on big screens and not on small screens. So if you have a class called `clearfix` but you don't clear on small screens, then you now have misleading code.

If you use semantic class names, then you can style them differently based on media queries making it easier to maintain.

## Because semantic class names are easier to find.

If an element has classes based on how it looks such as `.red`, `clearfix` and `.pull-left`, then these classes will be scattered all over the codebase&mdash;so if you’re trying to hunt for a particular piece of HTML, the class name is not going to help you.

On the other hand, if your class names are semantic, a search will find the HTML in question. Or more typically, if you're beginning your search via the HTML (think: inspecting an element) then finding unique CSS selectors based on the class name will be far easier.

## Because you don't want unexpected regression.

If you have utility non-semantic classes that describe the look then when you edit one of these classes, they will propagate to every single element with that class. Knowing CSS as you do, do you feel comfortable that the propagation didn't cause unexpected problems elsewhere?

Semantic class names are unique, so when editing one, you *can* feel comfortable that your change only applies to the module in question as you intended, making maintainance easier.

## Because you don't want to be afraid to update code.

Directly linked with the previous point about regression, when you don't feel comfortable touching code, you end up causing problems or being afraid to touch it at all. Therefore you end up with redundant code, increasing maintainance issues.

## Because it helps automated functional testing.

Automated functional tests work by searching for particular elements, interacting with them (typing in text, clicking buttons and links etc) and verifying based on that.

If you have visual class names all over the HTML, then there is no reliable way to target particular elements and act upon them.

## Because it provides meaningful hooks for Javascript to enhance.

Just like these hooks help automated testing, they are also useful to enhance behaviour with Javascript. Visual class names can't be used as a reliable way to target modules or components.

## Because of general maintainance concerns.

If you name something based on what it is, you won't have to touch the HTML again i.e. a heading is always a heading, no matter what it *looks* like.

The styling might change but you only have to update the CSS. This is otherwise known as Loose Coupling and this improves maintainability.

## Because utility classes increase noise when debugging.

When debugging an element, there will be several applicable CSS selectors making it noisey to debug.

## Because the standards recommend it.

On using the class attribute, HTML5 specs say in 3.2.5.7:

> "[...] authors are encouraged to use values that describe the nature of the content, rather than values that describe the desired presentation of the content."

## Because you get a performance bump.

This is a *very* small benefit but when you have one class name per element, you end up with smaller HTML. With visual classes you end up with multiple class names per element and that can add up.

## Because it adheres to the reuse rule.

If you don't use semantic class names, then you will likely be breaking the rules of reuse. Read the next chapter to find out more.

<!--## Why? Because visual class names might declare the same property!

It's likely that several different utility classes could refer to the same property meaning order matters and performance degrades.

Think of an example of this.
-->

## Final thoughts about semantics

Semantic class names are a corner stone of *MaintainableCSS*. Without this, everything else makes little sense. So name something based on what it is and everything else follows.
