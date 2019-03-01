---
layout: post
title:  "授权建模"
permalink: /lessons/authorisation-modelling/
---

## 谁来做，怎么做？

在这一点上，我们不考虑使用特定技术来达到我们的需求。

在开发开始之前，我们将定义和允许谁做什么。换句话说，我们要缩小现实世界和数字世界之间的距离。

* [实体-控制-关系 图](http://www.cs.sjsu.edu/~pearce/modules/patterns/enterprise/ecb/ecb.htm)

![use-case]({{ site.baseurl }}/assets/images/create-product.png)

![use-case]({{ site.baseurl }}/assets/images/send-safety-seal.png)

**Actors（角色）**：一个用于和我们已构建系统实现交互的身份。

* 仓库管理者。

**Boundaries（关系）**：这些actors(角色)如何与系统实现交互。仓库管理者使用操作员面板与系统实现交互。

**Entities（实体）**：我们在系统中进行抽象的数据模型。

* 产品
* 安全封条

**Controllers（控制器）**: 每个actor（角色）触发的对entities（实体）执行更改的操作。

* **创建产品**：在系统中储存新产品。
* **发放安全封条**：在检查产品是否通过手动安全控制后，操作员选择是否发放安全封条。

下一个模块将向您展示如何使用NEM Smart Assets System（NEM智能资产系统）把实体和角色进行映射。
