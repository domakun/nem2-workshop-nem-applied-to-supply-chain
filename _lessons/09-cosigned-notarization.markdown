---
layout: post
title:  "多重签署公证"
permalink: /lessons/co-signed-notarization/
---

## 背景

![recipient-strategy-sink]({{ site.baseurl }}/assets/images/diagram-multisig.png)

Alice和Bob已达成协议，并在数字文件中将协议正式化。他们希望共同为文档添加签名和时间戳。

NEM可以生成 **multisignature accounts（多重签署公证）**。当你将账户转换为[multisig（多重签名）](https://nemtech.github.io/concepts/multisig-account.html)后,这个账户就不能独自发布交易。它需要其他帐户（称为共同签署者cosignatories）一起来宣布它们的交易。

大多数时候，我们不强制要求所有共同签署者（cosignatories）来参与交易事务。合约制定时规定了可达成协议需要的最少共同签署者数量。之后可以改变这些属性以满足几乎所有需求。

要牢记的一些重要事项：

* 将帐户转换为multisig（多重签名）后，您将无法再从该帐户启动交易。只有共同签署者cosignatories可以启动multisig帐户的交易。
* NEM目前对multisig的实现是 **"M-of-N"**，意味着M可以是等于或小于N的任何数字，即，1-of-2,4-of-2,4-of-9 ，9-of-10，等等。
* Multisig帐户最多可以有10个签名者。
* 一个帐户最多可以是5个多重帐户的签名者。
* 批准(approve)交易和删除(remove)共同签名的最小共同签署者(cosignature)的数量是 **可更改的**。
* multisig帐户可以有另一个multisig作为协同者，最多3个级别。


您将创建一个 **2-of-2 multisig帐户** 来代表数字资产，其中共同签署者(cosignature)为Alice和Bob。

## 实现方法

根据我们已知解读记录的方式，公证被认为是有效的。在这个案例中，我们认为只有从同一公证帐户发送的邮件才有效。

<strong class='tit'>1\. 打开``Create deterministic account``选项卡。为数字文档创建一个新的确定性帐户，并使用Alice的私钥对其进行签名。</strong>

<strong class='tit'>2\. 为Bob创建一个账户。</strong>

<strong class='tit'>3\. 打开``Create Multisig Account``选项卡。创建一个multisig帐户:</strong>

我们正在使用一种叫做 [aggregate transaction（汇总交易）](https://nemtech.github.io/concepts/aggregate-transaction.html)的新型交易。你可以[在这里](https://github.com/nemtech/nem2-workshop-document-notarization/blob/v0.1.0/project/src/app/components/createCosignedNotarization/createCosignedNotarization.component.ts#L48)查看。

- **Multisig private key**（多重签名私钥）: 公证私钥（确定性）；
- **Cosignatories to add**（将要添加的共同签署者）: Alice和 Bob's私钥；
- **Min Approval**（最小赞同人数）: 2, 意味着需要两个签名才能宣布此帐户的交易；
- **Min Removal**（最小剔除人数）: 1，意味着从multisig帐户中删除另一个共同签署者(cosignature)只需要一个共同签名（cosignatory）。


![screenshot-multisig-account]({{ site.baseurl }}/assets/images/screenshot-multisig-account.png)

静待交易得到确认。

<strong class='tit'>4\. 打开``Cosigned Notarization``标签，发布签署的公证书。</strong>

- **File**（文件）:test.txt；
- **Multisig public key**（Multisig 公钥）:公证公钥（确定性）；
- **Signer private key**（签名者私钥）:Alice's的私钥。

点击``Notarize``然后等待 **"Notarization pending to be cosigned with hash A8...E6"** 这条消息地返回。


<strong class='tit'>5\. Bob也必须参与交易事务，因为我们将``minApproval``设置为2。</strong>

{% highlight bash %}
nem2-cli transaction cosign --profile bob --hash A855F0C49B78100AFB733DF53FD6758615132E7DBBF74C7B856E4CBACF0946E6
{% endhighlight %}

如果一切顺利，状态应该更改为：**"Notarization pending to be cosigned with hash A8...E6"**。

<strong class='tit'>6\. 前往 ``Get Notarization`` 标签页,并在确认状态更改后，搜索交易哈希码。</strong>
