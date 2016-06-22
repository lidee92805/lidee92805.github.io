---
layout: post
title: "负边距和布局"
date: 2016-06-22
tags: [Web]
comments: true
---

# 负边距在让元素产生偏移时和position: relative有什么区别

负边距偏移会影响整个文档流，position: relative不影响文档流

![](/images/margin-relative.png)

# 使用负边距形成三栏布局有什么条件？

1. 三栏左浮动，父容器清除浮动

2. 左右栏定宽，中栏适应宽度

3. 左栏margin-left设为-100%,右栏margin-left设为-栏宽

# 圣杯布局的原理是怎样的？简述实现圣杯布局的步骤

原理: 三栏浮动，左右栏负边距，父容器设置padding挤压中栏，左右栏相对定位

1. 三栏浮动，父容器清除浮动

2. 左栏margin-left设为-100%,右栏margin-left设为-栏宽

3. 设置父容器padding挤压中栏

4. 左右栏相对定位确定位置

# 双飞翼布局的原理？实现步骤？

原理: 三栏浮动，左右栏负边距，中栏设置margin与左右栏形成间距

1. 三栏浮动，父容器清除浮动

2. 左栏margin-left设为-100%,右栏margin-left设为-栏宽

3. 中栏设置margin与左右栏形成间距


本博客版权归lidee92805和饥人谷所有，转载请注明出处





