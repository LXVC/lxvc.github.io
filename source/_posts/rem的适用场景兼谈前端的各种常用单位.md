---
title: rem 的适用场景兼谈前端的各种常用单位
date: 2017-04-09 15:55:13
tags: [CSS, 前端]
---


这个时间点，没用过、没听说过 `rem` 这个单位的前端程序员应该很少了。在我以前做过的项目里，就大量使用了 `rem` 作为单位，然而正是因为大量使用，所以习以为常，没有去深入研究过 `rem` 的哪些优势使得它这么受欢迎。

这次我有机会去主导一个项目前端的方方面面，包括不限于框架，规范，组件、CSS 设计，测试，部署，监控各个方面，所以对我提出了更高的要求，我不得不去了解那些以前被我忽略的点，以保证整个系统的稳定，可控。这是研究了相关资料后得出的第一篇总结，以后会尽量把踩下的坑记录在这个博客里。

<!-- more -->

## 背景

说到 `rem` (font size of the root element) 就不得不说到另一个单位：`em` (font size of the element)。
`em` 可以简单的理解为：

> em = 希望得到的像素大小 / 父元素字体像素大小

例如有如下代码：
```html
  <style>
    p {
      font-size: 30px;
    }

    p span {
      font-size: 2em;
    }

    .small-span {
      font-size: 0.5em;
    }
  </style>

  <p>
    test-p
    <span>test-span</span>
    <span class="small-span">test-small-span</span>
  </p>
```

![](http://vc-pics.oss-cn-shanghai.aliyuncs.com/test-em.png)

所以 `rem` 可以理解为：

> rem = 希望得到的像素大小 / root元素字体像素大小

然而，哪个元素是 `root`，是 `body` 还是 `html`?
