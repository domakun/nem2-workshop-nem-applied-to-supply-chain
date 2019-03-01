---
layout: post
title:  "数据建模"
permalink: /lessons/data-modelling/
---

区块链是一项具有革命意义的技术。现有企业往往发现替换其已有的核心流程是很困难的。即使是新兴行业公司或刚起步的公司也可能在定义大范围分散应用程序时出现问题。

当我们在在开发软件时，我们往往使用 **不同的技术**。

我们可以使用NEM区块链有效地解决某些特殊问题。对于剩下的问题，我们完全可以通过使用关系数据库和非关系数据库，或者像IPFS一样使用分布式存储系统来解决。

这个模块解释了NEM区块链中可用的内置功能的子集。请花几分钟时间阅读它们，并思考如何在我们之前设计的系统中使用这些功能。

## Account（账户）

![account]({{ site.baseurl }}/assets/images/concept-account.png)

[account(账户)](https://nemtech.github.io/concepts/account.html)是存储在NEM区块链上的，与可变状态相关联的密钥对（私钥和公钥）。简单来说，你在区块链上有个只有您可以使用密钥对来进行操作的保险箱。顾名思义，私钥必须始终保密。任何有权访问私钥的人都对该账户享有控制权。

NEM accounts（帐户）被看作区块链中资产的容器。与大多数区块链一样，帐户可以简单地表示为用户的账户中装满了硬币。但它也可以代表一个必须唯一且可更新的单个对象：比如将被运输的包裹，房屋的契约或要公证的文件。

## Namespace（命名空间）和Mosaics（马赛克）

![mosaic]({{ site.baseurl }}/assets/images/concept-mosaic.png)

[Namespaces（命名空间）](https://nemtech.github.io/concepts/namespace.html)允许您在NEM区块链上为您的业务和资产开辟一个独一无二的空间。Namespaces（命名空间）以您选择的名称开头，类似于Internet域名。

注册命名空间后，您可以定义subnamespaces(子命名空间)和马赛克。

[Mosaics（马赛克）](https://nemtech.github.io/concepts/mosaic.html)是让智能资产系统变得与众不同与灵活的一部分原因。Mosaics（马赛克）是NEM区块链上的固定资产，可以表示一组不会发生变化的等同事物。马赛克可以是一种代币，但它也可以代表一系列更专门的资产，如奖励的积分，股票，签名，状态标志，投票甚至其他货币。

## Transfer Transaction（转账交易）

![transfer-transaction]({{ site.baseurl }}/assets/images/concept-transfer.png)

[Transfer transactions（转账交易）](https://nemtech.github.io/concepts/transfer-transaction.html) 用于在两个帐户之间发送马赛克。Transfer transactions（转账交易）可以包括最多1024个字符的消息。
