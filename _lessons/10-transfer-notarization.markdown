---
layout: post
title:  "转让公证"
permalink: /lessons/transfer-notarization/
---

## 背景

Alice想把公证转交给Bob。公证类型是 **2-of-2 多重签名（2-of-2 multisig）**。通过回顾前一个模块，我们知道只有共同签署者（cosignatories）能够在多重签名（multisig）账号发布交易。如果Alice把她自己从共同签署者（cosignatories）中删除，那么Bob将成为唯一一个允许其发布交易的人。

multisig合约是 **可编辑的**。Alice可以通过更改多重签名帐户（multisig account）合约，把自己从账户中删除。我们把``minRemoval``设置为1后，从帐户中删除某人只需要一个cosignature（共同签署者）。

从此时起，Bob成为了唯一允许从该帐户转移交易的人。公证类型将变成 **1-of-1 multisig（1-of-1 多重签名）**。

![diagram-transfer]({{ site.baseurl }}/assets/images/diagram-transfer.png)

## 实现方法

<strong class='tit'>1\. 打开 ``Create multisig``标签页并点击``Edit multisig``。<strong class='tit'>

{% highlight bash %}
$> nem2-cli account generate
Introduce network type (MIJIN_TEST, MIJIN, MAIN_NET, TEST_NET): MIJIN_TEST
Do you want to save it? [y/n]: y
Introduce NEM 2 Node URL. (Example: http://localhost:3000): http://localhost:3000
Insert profile name (blank means default and it could overwrite the previous profile): bob
New Account:    SBTFDZE4RTETKCTGNLIV4HL6ACLZIHUDBVAKACOB
Public Key:     EB770736BC20FF07682849A067423E679C7E2167BAB3CA33DE2EF9B0D088F4EF
Private Key:    88...92
{% endhighlight %}

<strong class='tit'>2\. 填表:</strong>

* **Signer private key**（签名者私钥）: Alice的私钥；
* **Multisig public key**（多重签名公钥）: 公证的公钥（确定性）；
* **Cosignatories to remove**（要删除的签名者）: Alice的公钥；
* **Min Approval Delta**（最小赞同增量）:-1。 当我们删除其中一个cosignatories，并且minApproval设置为2时，减去1使账户成为 1-of-1。

![screenshot-edit-multisig-account]({{ site.baseurl }}/assets/images/screenshot-edit-multisig-account.png)


<strong class='tit'>3\. 单击``Edit multisig``按钮。 把``minRemoval``设置成1,Bob已经没有签署交易的需求了。等待直到收到网络确认交易的信息。</strong>
