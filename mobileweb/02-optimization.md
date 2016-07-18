---
title: H5前端性能优化指南
category: mobileweb
layout: page
date: 2016-07-10
modifiedOn: 2016-07-10
---

## 优化指南

- PC优化手段在Mobile端照样适用
- 在Mobile端我们提出三秒钟渲染完成首屏指标
- 基于第二点,首屏加载3秒完成或使用Loading
- 基于联通3G网络平均338KB/s(2.71Mb/s), 所以首屏资源不应超过1014KB, 
- Mobile端因手机配置原因, 除加载外渲染速度也是优化重点
- 基于第五点, 要合理处理代码减少渲染损耗
- 基于第二、第五点, 所有影响首屏加载和渲染的代码应该在处理逻辑中后置
- 加载完全之后用户交互使用时也需注意性能优化




