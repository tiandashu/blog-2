date: 2020-06-21 19:32

---

# 在线教育中技术与业务疑难问题之考试系统

我在一家在线教育公司有一两年的时间，精力都投入到了考试系统的设计上。

对于考试系统这个产品，无论从各方面来讲，都有一些小小的收获。于是抽时间回顾并写了下来，希望对后来人有所裨益

> 东坡云："事如春梦了无痕"，苟不记之笔墨，未免有辜彼苍之厚。

## 考试

考试，几乎所有人都经过中考高考的洗礼，所有人对它都非常熟悉，甚至痛恨与恐惧。

这一点很好，这是所有人都熟悉的业务，每个人都曾是该业务的用户，每个人都能对业务此添上一嘴，并很有可能提出****的建设性意见。

先来说一说考试的特点

1. 次数少。每年两次考试，因此我们作为开发来讲，一般大忙都在期末
1. 科目多。中学有语数外理化生政史地九门，大学有高数英语及各种专业课考试，题型及科目的多样性注定了它的题库复杂。科目多则体型杂，体型杂则数据结构化复杂，填空、单选、多选等客观题需要给出，数学推断，英语作文，英语口语及翻译。雅思多选与托佛多选的判分不一致
1. 标准化考试。标准化考试，就有标准题型，一致的判分准则及
1. 高并发。高并发没有绝对之分，只有相对而言。**当期末多个学校、院系统一考试几千人甚至几万人同时答题**，并且学生试卷答案实时提交，主观题几乎是有所改动就要提交一次，会对服务器造成过大的压力
1. 固定场地。线上会在机房进行统一考试，并且有监考老师巡场
1. 机房考试。可以使用最新的 Chrome，减少兼容性问题

关于这点，将在以下对他们的技术及业务层面展开讨论

## 科目分裂、题库及通用考试系统

正因为有多个科目，基于结构化的题库系统，**做一套通用的考试系统的本质是做一套通用的题库系统**

如对于数学而言

当然，更复杂的情形可参考我关于题库系统的文章

> 关于考试背后的题库系统，对于教育公司是相当重要的基础支撑服务，可参考我以前的文章分析

## 业务价值及中学，高校及信息化建设

## toB (or toC)

考试系统这一业务形态便注定它的场景大部分在 toB 端

## User: Student or Teacher

## 关于考试系统的表与关系

+ `Paper`: 试卷
+ `Exam`: 考试，一份试卷可以生成多次考试
+ `Classes`: 班级，一份考试可布置在多个班级当中，一个班级可以有多份考试
+ `Student`: 学生，一个班级有多个学生，一个学生也可在多个班级，一个学生对一份考试会生成一份答卷
+ `Sheet`: 答卷，某学生关于某考试的答题卡
+ `Question`: 问题，某一个试卷由多道题目组成，可能有多层嵌套。
+ `Answer`: 答题，答题卡上关于某问题的答案

以上就是关于考试系统的所有表及关系，这有助于梳理业务及对以下在真实场景中遇到的问题进行讲解

## 防作弊，技术、业务与用户(学生)的折中

从中学到大学，每次考试总会听说有几个考试作弊的，对于考试作弊的手段也屡见不鲜。还有一部把考试作弊拍得紧张刺激的电影。

但是线上的作弊手段比线下更繁杂，更新颖以及更容易。以下是我在做考试系统时经常能够看到的场景

1. 线上考试如何避免学生百度答案
1. 线上考试如何避免场外学生替考
1. 线上考试如何避免

**但是增加技术或业务的复杂性的同时，也会带来更多的问题** ，考试系统的防作弊带来的问题更多。

### 避免百度：离屏检测

对于离屏检测的原理可以总结为两个方面，次数及时间

+ 离屏五次
+ 并且累计丽萍时间超过三分钟

满足以上条件则判为作弊

#### 离屏检测前端技术实现

在伟大的开放的浏览器环境下，可以通过浏览器事件 `visibilitychange` 及 `hide/show` 来监听离屏事件

但是它有一个致命的问题，悬浮框！当你开两个浏览器，一边做题，一边百度，这是无法被检测到的，那怎么办？

**全屏**

关于全屏也有一个浏览器的 API 及事件: `FullScreen API`，虽然 API 兼容性不太好，但是对于机房环境也是足够了的。

如此之来便没有问题了吗？如果你使用浏览器插件或者特殊浏览有小悬浮框的话，依然会被误判为离屏，并在线上前线老师反馈过几次问题。因此需要

1. 统一的考试环境
1. 卸掉不必要的插件

总结下离屏检测的技术实现

1. `FullScreen API` 全屏答卷
1. `visibilitychange` 离屏检测
1. 强统一的考试环境

### 避免替考：单设备登录及 IP 限制

为了避免学生考试替考，当考生在场外有人替答时，在考场本人的电脑上显示整屏并且醒目的提示语，使得老师巡考时就能够发现该学生作弊。

### 解除作弊嫌疑


## 补考: 考试故障的兜底方案



### Web or Native

## 题库系统及其结构化

## 拍照答题

## 试卷答题补救

## 自动改卷及人工改卷

## 自动改卷：AI，自研或第三方服务?

## 考试分析: 班级分析与试卷分析

## 试卷及班级报告 EXCEL/PDF 批量生成

关于某一试卷及某一班级成绩的 EXCEL 信息生成及 PDF 批量下载

### EXCEL 生成

这是一个简单但是繁琐的需求，如果非要在技术上说一些可说的，那就是**在前端生成 excel 还是后端生成**

我是偏向于前端生成的，前后端分离，后端负责数据，前端负责 UI，而 excel 明显是属于 UI 一类的。而且在生产环境不止一次出现过 Excel 数据与页面列表数据不一致的问题，这就是 Excel 在后端生成的果

> 关于在生产环境的更多问题，可以参考山月总结的 [虫子集]()

### PDF 批量生成并打包下载

在教师端的考试分析页面，一个班级，所有学生成绩，一张列表，一目了然，每个学生成绩都可以查看他们的本次考试报告

院方的教师看在眼里，想在心里：**如果可以把所有学生的报告都打包下载出来就好了**

### PDF 批量打包下载方案

目前可以确定几个事情

## 与微信小程序的互补

## 错题本及题库系统的进一步结合

## 钉钉及文档类工具的冲击

## 与硬件进一步结合的思考

## 功能必要性，技术可行性与规格问题

## 总结


