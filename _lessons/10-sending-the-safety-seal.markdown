---
layout: post
title:  "发送安全封条"
permalink: /lessons/sending-the-safety-seal/
---

## 背景

假设操作员已经彻底测试了产品，且确保它已经贴上了公司的安全封条。

实现此流程包括以下步骤：

1）创建一个转让交易，添加一个company.safety：seal；

2）使用操作员的帐户签署转账交易；

3) 发布转让交易。

![use-case-nem]({{ site.baseurl }}/assets/images/use-case-nem.png)

我们现在应该关注并跟踪是谁发送了封条。出于这个原因，我们将直接从web app发布交易，并使用操作员帐户进行签名。

## 实现方法

<strong class='tit'>1\. 打开 ``Send Safety Seal``标签页，尝试点击这个按钮：您可能回获得一个错误信息，因为这个触发功能还没有被完善。</strong>

![send-safety-seal]({{ site.baseurl }}/assets/images/screenshot-send-safety-seal.png)

<strong class='tit'>2\. 打开 ``project/dashboard/src/app/services/safetySeal.service.ts``, 找到 ``createSafetySealTransaction`` 函数。</strong>

在上一个模块中，您已经发布了一个转让事务，但没有看到它的实现。

<strong class='tit'>3\. 创建一个转让交易</strong>

* **Deadline**（最后期限）:在交易确定保存在块（block）里之前，有多少块（blocks）可以通过;
* **Recipient**（接受者）:产品地址；
* **Mosaics**（马赛克）: 1 company.safety:seal；
* **Network**（网络）:在本次练习中我们使用MIJIN_TESTNET。

{% highlight typescript %}
createSafetySealTransaction(productAddress: Address): TransferTransaction {

    return TransferTransaction.create(
      Deadline.create(),
      productAddress,
      [new Mosaic(new MosaicId('company.safety:seal'), UInt64.fromUint(1))],
      EmptyMessage,
      NetworkType.MIJIN_TEST
    );
}
{% endhighlight %}

## 测试更改

<strong class='tit'>4\. 保存更改，现在,你可以按下 ``Confirm Safety Seal``按钮。</strong>

如果操作有效, 事务将以 **unconfirmed status（未确认状态）** 到达网络。

![screenshot-unconfirmed-added]({{ site.baseurl }}/assets/images/screenshot-unconfirmed-added.png)

不要在状态未经证实的交易上抱有太多信任。因为我们尚不明确它是否已经被保存在一个block(块)中。


一旦交易被 **确认**，它就会被保存在一个block(块)；一旦交易被包含在一个区块中，该交易就被 **确认**。如果是转让交易，则所述金额将从发件人的帐户转移到收件人的帐户。

![screenshot-confirmed-transaction]({{ site.baseurl }}/assets/images/screenshot-confirmed.png)

如果转让交易通知您有一些验证错误，您需要注意``Status Error``部分，请您打开[Error description（错误描述）](https://nemtech.github.io/api/websockets.html) 查看这个错误代表的含义。

![screenshot-status-error]({{ site.baseurl }}/assets/images/screenshot-status-error.png)

<strong class='tit'>5\. 返回 ``Create Product`` 标签页，打开你已经发送了safety seal（安全封条）的产品，检查该产品是否有以下马赛克：</strong>

![screenshot-product-detail-seal]({{ site.baseurl }}/assets/images/screenshot-product-detail-seal.png)

## 接下来

恭喜您，您已完成建议的用例！在接下来的模块中，我们将使用新的NEM内置功能为我们的解决方案做一些修改。
