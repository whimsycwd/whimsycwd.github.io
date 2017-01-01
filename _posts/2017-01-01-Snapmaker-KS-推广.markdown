---
layout: post
title:  "Snapmaker Kickstarter 推广"
date:   2017-01-01 12:00:00 +0800
categories: microservice
---


## 0. 背景

Snapmaker 准备  在KickStater 上众筹。 
这需要较大的受众群体， 现代没有所谓的酒香不怕巷子深。 应该在广告推广投入合理的预算。
对流量进行引流， 从而促进众筹的成功。 

## 1. 目标
正式上线前， 进行推广预热， 收集邮箱信息。 
上线第一天， 希望能够有较好的众筹成绩。 

## 2. 推广的方法


### 2.1 运营

1. [Facebook 主页](https://www.facebook.com/snapmakerinc/?ref=notif&notif_t=page_admin&notif_id=1483069743498087)运营， 粉丝运营。
2. youbube 频道运营 
3. 邮件列表维护(mb base(9000) + )
	-  需要扩充邮件列表基数 
4. 社交网络驱动
	- 朋友+

## 2.2 广告推广
1. FB 广告投放
	- 根据3D打印机兴趣定向人群
	- 根据邮件列表自定义人群
	- 根据自定义人群查找相似人群
2. 激励二次推广
	- 比如通过discount 的方式， 激励客户在自己的社交媒体中进行二次推广? @TODO




## 3. 广告推广效果定义

### 3.1 ROI
```
       advertising-revenue - invesetment
ROI= --------------------------------------
                 investment

```
ROI > 0 那么广告投入就是值得的。 但是我们定义的ROI 的计算方式可能有误差， 所以当ROI > 100% 时， 我们才可以比较确认地说， 
广告投入得到了比较好的效果。 

### Landing Page
落地页， 吸引用户点击广告之后地内容详情页。 这个页面在众筹正式开始的时候后是  KS project page.  这个页面最终目的是说服用户Pledge
在众筹开始前预投放的时候是 `海报页面`, 这个页面的目的是使得， 用户关注我们产品， 留下邮箱。 

### 3.2 Impression
广告展现， 这个广告给多少用户看到

### 3.3 Click 
广告内容让用户感兴趣， 用户点击了广告， 跳转到了 landpage.  如此就算一个引流


### 3.4 Purchase(Pledge)
成功把用户吸引到落地页面之后， 用户对内容感兴趣， 完成了购买行为。 


## 4. 广告推广的方法

### 4.1 预算

通过跟踪 Impression(品牌效果， 触达), Click(广告点击， 产品吸引程度), Purchase(购买行为， 受众匹配度， 产品的市场效果). 量化地计算一个ROI.  如果ROI效果好， 则加大投入。 否则则减少广告投入。 


大概以这样一个基准: 

-   预投放时， 每天花费 50$. 
-   正式投放是， 每天花费 300$


### 4.2 调试广告
针对 物料创意  ＋ 人群设置约 + Lead Ad 的方式设置 20  组广告。 

```
Phase 1:
物料(2) x Lead Ad 的方式 x (Look Alike 人群 & 3D Print 人群) = 8 组， 每组每天花费 6$, 持续一周。 


Phase 2:
基于Phase 1 的结果，分析物料， Lead Ad 和 人群的影响方式

重新设置 10 组广告 每组每天花费 6$ 持续一周

Phase 3: 
基于 Phase 2 的结果， 选取 3 组表现最好的设置， 每组每天花费 100$. 
根据ROI动态调整广告投入。 
```

## 5. 广告推广计划的实现

目前主要关注这次Campaign



### 5.1 概述

![概览](/image/ks_digital_marcketing/overview.png)

1. 用Kickstarter 提供的Dashboard 来跟踪收入， 和基本概览指标
2. 用自定义的 GA Analyntics 来分析， 详情的 conversion & click & ROI
	- 用 url-builder 来区分推广的方法
3. 用 bitly 来缩短url， **跟踪实时的点击数据**
4. 用 Facebook Ad Manager 来进行广告投放， 进行预算控制， 以及， 基本的展现和， 点击指标。 

### 5.1 工具
1. [bitly](https://app.bitly.com/default/bitlinks/2hQCmDS)  用来跟踪url点击， i.e 广告点击。 **实时** 
2. [campaign-url-builder/](https://ga-dev-tools.appspot.com/campaign-url-builder/) GA 用来分析 source & medium & campaign 
3. [FB Ad Manager](https://www.facebook.com/ads/manager/creation/creation/?act=323897594428277&pid=p1) 进行广告投放, 自定义人群的上传， 物料上传
4. [GA](https://analytics.google.com/analytics/web/#report/conversions-goals-overview/a85243049w127327271p131010527/) 广告效果跟踪， 分source/medium/campaign -> 展现， 点击， 转化， 购买
5. [KS default report](https://www.kickstarter.com/blog/introducing-google-analytics-and-an-inside-look-at-the-creator-d)

### 5.2 Implementation

1. KS 不支持creator 自己上传js, 代码， 因此我们不能部署 Facebook pixel 来检测投放效果
2. [KS 集成了 GA Analytics](https://www.kickstarter.com/blog/introducing-google-analytics-and-an-inside-look-at-the-creator-d), 把UA-id 加到 KS的的设置里面， 等价于插入了GA代码
3. 针对不同的的广告设置(物料+人群), 通过使用[campaign-url-builder/](https://ga-dev-tools.appspot.com/campaign-url-builder/) 来设置标签, 从而跟踪广告效果
4. 使用bitly 对上面产生的url 进行缩短， 同时跟踪实时的点击数据
5. 在GA Analytics 上设置 `begin with ${xxx_url} ` 的Goal 来跟踪一些 Micro Gaols. 
6. 利用收集的邮箱在 Fb Ad Manager 上设置自定义人群。 以及寻找`相似人群`进行投放。 

8. 在 Facebook Ad Manger 管理`预算`
9. 在 Facebook Ad Manager 跟踪广告展现量
10. bitly + Facebook Ad Manager + GA Analytics 上跟踪 `点击量`
11. GA Analytics 上跟踪` Goal 转化率 `
12. 在 KS GA Dashboard 跟踪具体的`Pledge 数量`
13. 根据报表结果， `调整物料和人群的设置`

### 5.3 参考资料

1. [2015 4.30 KS GA integration](https://www.kickstarter.com/blog/introducing-google-analytics-and-an-inside-look-at-the-creator-d)
2. [A Simple Guide to Using Google Analytics for Your Kickstarter Campaign](http://www.crowdcrux.com/simple-guide-using-google-analytics-kickstarter-campaign/)
3. 2015 4.18 GNARBOX
	- [gnarbox blog](http://www.gnarbox.com/blogs/gnarbox-news/88115267-google-analytics-kickstater-building-a-data-model-for-tracking-kickstarter)
	- [gnarbox campus](https://www.kickstarter.com/campus/questions/building-a-data-model-for-tracking-kickstarter)





