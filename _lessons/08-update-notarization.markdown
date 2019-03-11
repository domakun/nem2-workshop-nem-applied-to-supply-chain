---
layout: post
title:  "启用公证更新"
permalink: /lessons/notarization-updates/
---

## 背景

![recipient-strategy-asset]({{ site.baseurl }}/assets/images/diagram-recipient-strategy-asset.png)

Alice决定创建一个唯一代表公证数字资产的帐户。 您可以通过向同一帐户宣布交易来跟踪更新。

## 实现方法

<strong class='tit'>1\. 编辑您正在公证的txt文件，将文本更改为"Lorem Ipsum"。</strong>


<strong class='tit'>2\. 在浏览器中打开交易仪表板（Notarization dashboard）并发布已编辑的文件，将其作为地址来发送之前模块中已创建账户的公证。然后，用Alice的私钥签署公证</strong>

![screenshot-update-notarization]({{ site.baseurl }}/assets/images/screenshot-update-notarization.png)

<strong class='tit'>3\. 确认好后,前往``Get transactions``标签页。使用文件的账户公钥来获取所有交易，你现在应该能看见两项交易。</strong>

![screenshot-get-updated-notarizations]({{ site.baseurl }}/assets/images/screenshot-get-updated-notarizations.png)

## 注意

您已将交易发送到代表公证的帐户。您现在应该清楚的是，网络中的每个人都能够向此帐户发送交易。

我们如何知道谁是资产的所有者，或者谁被允许执行更新操作？

**这种逻辑不通过链来实现** 您可以在第一条消息中声明一个地址。只有从该地址帐户写入的邮件才有效。 另一种实现方式是，只有从资产帐户（asset account）发送的交易才有效。

    ℹ️  未来版本将增加阻止指定帐户交易的可能性。 现在，实现它的最佳方法是过滤掉交易。
