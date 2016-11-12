---
layout: post
title:  "基于Android平台的糖尿病人自我管理的手机应用"
date:   2013-10-15
categories: [project, android]
comments: true
---

## 背景

- 在全球几乎每一个国家，糖尿病发病率都在上升
  - 世卫组织估计全世界2.2亿多人患有糖尿病
- 糖尿病是导致患者死亡的最重要原因之一
  - 每年因它而丧命的患者人数与因艾滋病病毒/艾滋病 (HIV/AIDS) 而导致的死亡人数不相上下
- 糖尿病难以治愈的原因
  - 病人难以坚持治疗
  - 病人的病情也无法及时地反馈给医师

## 目标

开发一款手机软件，提醒患者即时测量血糖等数据，并且自动通过无线网络上传到医疗中心，从而促进病情的控制。

软件的基本功能

- 记录血糖等数据
- 通过图表以及表格显示这些数据
- 将这些数据上传到网上存储，共享
- 提醒按时服药
- 每日通过打卡帮助用户养成良好的生活习惯

## 设计

用户行为特点

![project-android-diabetes-users](/source/img/project-android-diabetes-users.jpg)

打卡模块

![project-android-diabetes-module-1](/source/img/project-android-diabetes-module-1.jpg)

记录模块

![project-android-diabetes-module-2](/source/img/project-android-diabetes-module-2.jpg)

显示模块

![project-android-diabetes-module-3](/source/img/project-android-diabetes-module-3.jpg)

## 布局

程序的主界面采用了线性布局LinearLayout

- 首先定义一个总体布局
  - 给这个布局起一个ID
  - 设置布局的宽度和高度为整个容器（屏幕）的高度
  - 并设置组件排列方式为垂直排列
- 接着，在这个总体的布局之中，加上若干的模块组件
  - 每个组件同样放在一个线性布局中，并采用嵌套的方式组合，以达到预期的效果

![project-android-diabetes-layout-1](/source/img/project-android-diabetes-layout-1.png)

![project-android-diabetes-layout-2](/source/img/project-android-diabetes-layout-2.png)

![project-android-diabetes-layout-3](/source/img/project-android-diabetes-layout-3.png)

## 功能实现

作图

- 使用了Google的一个开源图表库AChartEngine，它功能强大，支持散点图、折线图、饼图、气泡图、柱状图、等多种图表
  - 在dataset中设置需要显示的统计数据
  - 在renderer中指明图表的样式，比如每个元素的颜色，标题的大小，背景颜色等等
  - 使用ChartFactory.getLineChartIntent()取得图表
  
数据存储

- 程序使用IO流将信息直接保存在SD卡上
- 使用Environment类的getExternalStorageState()方法判断是否存在sdcard
  - 如果存在，则使用追加的方式向文本文件中保存内容
  - 若不存在，则使用Toast组件向用户提示错误信息

数据上传

- 将需要同步的数据放入DropBox文件夹中，实现与云端的同步

## 成功展示

Link to [Source](https://github.com/chennanni/diabetes-control-app)

打卡界面

![project-android-diabetes-presentation-1](/source/img/project-android-diabetes-presentation-1.png)

记录界面

![project-android-diabetes-presentation-2](/source/img/project-android-diabetes-presentation-2.png)

显示界面

![project-android-diabetes-presentation-3](/source/img/project-android-diabetes-presentation-3.png)

上传界面

![project-android-diabetes-presentation-4](/source/img/project-android-diabetes-presentation-4.png)
