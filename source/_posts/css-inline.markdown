---
layout: post
title: "css inline"
date: 2014-03-20 11:18:30 +0800
comments: true
categories: CSSinline
tag: CSS
---
今天使用要为元素设置水平居中的时候发现 `margin:0 auto` 不起作用，查资料发现原来我给div设置了的是`disply:inline-block`属性,这种情况下 `margin:0 auto` 是无效的，于是通过给父元素div 设置了 `text-aglin:center`得以解决.然后搜集了一些资料，加深一下对inline 属性的了解。
###测试结果 
1. inline元素, 有效的: `background`, `margin-left`, `margin-right`, `padding`, `border`
2. inline元素, 无效的: `width`, `height`, `margin-top`, `margin-bottom` (个别除外)
3. 因width, height无效, 因此下列属性也无效: `text-align`, `text-indent`, `min|max-width|height`, `overflow` 
4. 原文字相关的依然有效, 如`word-spacing`,  `word-wrap`,  `white-space`, `letter-spacing` 
5. 下面两个有效, `line-height`, `vertical-align`, `box-shadow` 
6. `float`,`position`及与之相关`top`,`left`, 不考虑, 已经不是inline了 
7. `img`, `button`, `input`, `label`, `select`, `textarea` 设置inline与inline-block, 效果是一样的, 没有变化, 都等同于设置了inline-block 

####思考: 
1. inline元素的宽高是不能定义, 即宽高度是由inline元素inline内容的元素决定的,(ie6,ie7下高度会受inline-block元素的影响) 
2. 即宽高完全是自适应而得出的, 因此也不存在overflow的问题, `min|max-width|height`也没有影响 
3. margin四个属性, `left|right` 与 `top|bottom`有不同的待遇 
4. `margin-top`, `margin-bottom`, 可以用line-height模拟, 但有很大不同, 与`inline-block`也不兼容,  另 `margin:0 auto` 的居中效果, 也没有作用 

####最后: 
mdn中说: 默认 inline-level 的元素 inline + inline-block 

- `b`, `big`, `i`, `small`, `tt`
- `abbr`, `acronym`, `cite`, `code`, `dfn`, `em`, `kbd`, `strong`, `samp`, `var`
- `a`, `bdo`, `br`, `img`, `map`, `object`, `q`, `script`, `span`, `sub`, `sup`
- 表单元素:`button`, `input`, `label`, `select`, `textarea` 与inline-block等同