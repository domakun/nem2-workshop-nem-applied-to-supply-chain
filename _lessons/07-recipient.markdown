---
layout: post
title:  "选择交易对象"
permalink: /lessons/recipient/
---

之前，我们使用了经过交易签名的同一帐户作为交易对象(recipient)。 我们可以使用哪些其他帐户？接下来，我们来看看其他选项：

![recipient-strategy-sink]({{ site.baseurl }}/assets/images/diagram-recipient-strategy-sink.png)

<strong class='tit'>1\. 继续向 **接收帐户（sink account）** 发送消息，您将使用该帐户存储所有公证。 您可以将其称为公证的钱包。</strong>

**优点**:
* 所有公证都存储在一个地方。
* 所有权可转让。您可以共享私钥或将其转换为[多重签名账户](https://nemtech.github.io/concepts/multisig-account.html).

**缺点**:
* 公证不可更新。
* 任何人都可以向此帐户发送交易。在阅读区块链记录时，应定义交易的有效性。

![recipient-strategy-asset]({{ site.baseurl }}/assets/images/diagram-recipient-strategy-asset.png)

<strong class='tit'>2\. 文件可以代表 **数字可识别资产(digital identifiable asset)**。为每个公证使用一个帐户，唯一地表示每个文件。</strong>

**优点**:
* 公证书由资产分隔。
* 公证可更新。
* 所有权可转让。

**缺点**:
* 任何人都可以向此帐户发送交易。 在阅读区块链记录时，应定义交易的有效性。

帐户的私钥可以以确定的方式生成。私钥是256位整数，可以根据文件特性生成。

    ℹ️ 如果该帐户不是用于发送交易，请选择创建确定性公钥。此时此帐户将无法宣布交易。

例如,**asset_private_key = account.sign(sha256(file_name))**

## 实现方法

<strong class='tit'>1\. 打开``Create deterministic account``标签页, 拖动文件并输入Alice的私钥。在点击创建后，将通过把sha256算法应用于文件名称，然后使用Alice的帐户对其进行签名来创建私钥。</strong>

比如:``asset_private_key = account.sign(sha256(file_name))``

![screenshot-create-deterministic-account]({{ site.baseurl }}/assets/images/screenshot-deterministic-account.png)

将帐户详细信息复制存放到安全的地方。您将在本次学习期间使用它们。

<strong class='tit'>2\. 返回``Create notarization``标签页并拖动.txt文件。添加资产（asset）作为交易的对象，并使用Alice的帐户签署交易。</strong>

在下一个练习中，您将看到如何更新公证。
