---
title: ANALYSIS OF INLINE ELEMENTS' FORMATTING
date: 2017-12-09 12:50:23
tags: css
---

# Abstract

*
Each HTML element (acts as container) is actually a stack of line-boxes, the contained elements is stuffed into each line-box. In this article, we try to find out what makes the height of each line-box, and how each element is aligned in line-box
*

*keywords*: strut, line box, content area, vertical align 


# Content Area

Let's start from the start point, there's only one element in line-box:

{% raw %}
<p data-height="265" data-theme-id="0" data-slug-hash="xPvMdJ" data-default-tab="css,result" data-user="xiechao06" data-embed-version="2" data-pen-title="xPvMdJ" class="codepen">See the Pen <a href="https://codepen.io/xiechao06/pen/xPvMdJ/">xPvMdJ</a> by 谢超 (<a href="https://codepen.io/xiechao06">@xiechao06</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
{% endraw %}

Let checkout it out carefully, here we set `font-size: 100px`, but is this `span` element has a height of 100px? No!!!, it has a height of 164px. The reason
is elaborated in this great article [https://iamvdo.me/en/blog/css-font-metrics-line-height-and-vertical-align](https://iamvdo.me/en/blog/css-font-metrics-line-height-and-vertical-align).
Here we only remember the facts:

1. `font-size` is relative to height of content area, but **IS NOT** the height of content area.
2. *ascender* plus *descender* is the height of the **content area**.

***note!!! content area is not the line box's height, but as we have only element, we could consider these 2 value has the same value for now***

Let's go a little further, what is line-box's height? 164px, and what is *span*'s line height property? a special value: `normal`, which is also the **default value**. We could guess what does *normal* means: a value computed by browser, that could just accomodate (sometimes, a little bigger than) the content area.

Now we comes to a problem: ***what is a proper value of line height?***

*line height* determines the height of line-box (only for inline elements). and usually, it is set to the times of *font-size*,  not the *height of content area*. so if the line-height is too small, two line-box may collapse. Here we got:
{% raw %}
<p data-height="265" data-theme-id="0" data-slug-hash="XzvOyB" data-default-tab="css,result" data-user="xiechao06" data-embed-version="2" data-pen-title="XzvOyB" class="codepen">See the Pen <a href="https://codepen.io/xiechao06/pen/XzvOyB/">XzvOyB</a> by 谢超 (<a href="https://codepen.io/xiechao06">@xiechao06</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
{% endraw %}
As we said, padding won't affect the height of line-box either.
{% raw %}
<p data-height="265" data-theme-id="0" data-slug-hash="xPvMeB" data-default-tab="css,result" data-user="xiechao06" data-embed-version="2" data-pen-title="xPvMeB" class="codepen">See the Pen <a href="https://codepen.io/xiechao06/pen/xPvMeB/">xPvMeB</a> by 谢超 (<a href="https://codepen.io/xiechao06">@xiechao06</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
{% endraw %}


What about *inline-box* elements? Height of line-box is determined by [height](https://developer.mozilla.org/en-US/docs/Web/CSS/height), that is the only difference. but you may not notice the difference, since property `height` has a clever default value **auto**, which is equal to the height of content area. but to understand this, we must understand `vertical-align` at first, we will give the example in following parts.

# the real computation of height of line box and vertical-align

Start from this example:
{% raw %}
<p data-height="265" data-theme-id="0" data-slug-hash="rYXRaQ" data-default-tab="css,result" data-user="xiechao06" data-embed-version="2" data-pen-title="rYXRaQ" class="codepen">See the Pen <a href="https://codepen.io/xiechao06/pen/rYXRaQ/">rYXRaQ</a> by 谢超 (<a href="https://codepen.io/xiechao06">@xiechao06</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
{% endraw %}

You may wonder why there are 2 spacings below and up the *span* element, that is due to ***strut*** - an imagined, invisible and zero-width character inserted into container automatically. if there's any elements in this container. here we give a more extrem example:

{% raw %}
<p data-height="265" data-theme-id="0" data-slug-hash="xPvBwN" data-default-tab="css,result" data-user="xiechao06" data-embed-version="2" data-pen-title="xPvBwN" class="codepen">See the Pen <a href="https://codepen.io/xiechao06/pen/xPvBwN/">xPvBwN</a> by 谢超 (<a href="https://codepen.io/xiechao06">@xiechao06</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
{% endraw %}

Even the contained element has no size, *strut* still works. With the help of *strut*, we could enter the dangerous zone of ***vertical-align***.

{% raw %}
<p data-height="265" data-theme-id="0" data-slug-hash="WXVmGd" data-default-tab="css,result" data-user="xiechao06" data-embed-version="2" data-pen-title="WXVmGd" class="codepen">See the Pen <a href="https://codepen.io/xiechao06/pen/WXVmGd/">WXVmGd</a> by 谢超 (<a href="https://codepen.io/xiechao06">@xiechao06</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
{% endraw %}

Let's analysis this snippet: 

The first element, `spacing maker`, align its baseline to strut's baseline, then browser found its line height is 4, so the line box's height is expanded to 4 * 18px = 72px.

![spacing maker](/assets/inline-elements-formatting/spacing-maker.svg "opt title")

here, we could see where **line-box's baseline** is clearly.

Then comes *middle*, align the middle point of content area to the line-box's baseline plus half of x-height.
Then come *text-top*, align the top of content area to the **strut**'s content area.
Then come *text-bottom*, align the bottom of content area to the **strut**'s content area.
Then come *top*, align the top of content area to the top of line-box.
Then come *bottom*, align the top of content area to the bottom of line-box.

As we could see, ***find out where is strut's content area (top, bottom, baseline) and line-box is key to understand vertical-align***. Now let's analyse some more examples:

{% raw %}
<p data-height="265" data-theme-id="0" data-slug-hash="WXVBqE" data-default-tab="css,result" data-user="xiechao06" data-embed-version="2" data-pen-title="WXVBqE" class="codepen">See the Pen <a href="https://codepen.io/xiechao06/pen/WXVBqE/">WXVBqE</a> by 谢超 (<a href="https://codepen.io/xiechao06">@xiechao06</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
{% endraw %}

Without the leading x, you may be very confused why the last `span` goes up/down. But with it, you could see the relation between strut and line-box.

Now we comes back to the inline-box elements. As we said earlier, property *height* decides height of the element's content area, let's check an example.

{% raw %}
<p data-height="265" data-theme-id="0" data-slug-hash="VroJeL" data-default-tab="css,result" data-user="xiechao06" data-embed-version="2" data-pen-title="VroJeL" class="codepen">See the Pen <a href="https://codepen.io/xiechao06/pen/VroJeL/">VroJeL</a> by 谢超 (<a href="https://codepen.io/xiechao06">@xiechao06</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
{% endraw %}

The confusing part is the second section, you could see, 




baseline of elements with no flow-in content.

height of line-box

height vs line-height


# References

[https://iamvdo.me/en/blog/css-font-metrics-line-height-and-vertical-align](https://iamvdo.me/en/blog/css-font-metrics-line-height-and-vertical-align)



