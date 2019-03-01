---
layout: post
title:  "添加其他管理者"
permalink: /lessons/adding-another-operator/
---

## 背景

由于技术进步，该公司提高了生产力。然后，该公司雇用了另一位仓库操作员。

虽然他们可以使用相同的帐户发送安全封条，但公司要求明确是谁执行了某项操作。

该公司还要查询该过程应用的条件逻辑：如果一个操作员同意发送封条，则另一个不用进行重复操作。

![use-case-nem-adding-operator]({{ site.baseurl }}/assets/images/use-case-nem-adding-operator.png)

### 多重签名账户

NEM可以生成 **multisignature accounts（多重签署公证）**。当你将账户转换为[multisig（多重签名）](https://nemtech.github.io/concepts/multisig-account.html)后，这个账户就不能独自发布交易。它需要其他帐户（称为共同签署者cosignatories）一起来宣布它们的交易。

大多数时候，我们不强制要求所有共同签署者（cosignatories）来参与交易事务。合约制定时规定了可达成协议需要的最少共同签署者数量。之后您也可以通过改变这些属性以满足几乎所有需求。

要牢记的一些重要事项：
* 将帐户转换为multisig（多重签名）后，您将无法再从该帐户启动交易。只有共同签署者（cosignatories）可以启动多重签名（multisig）帐户的交易。
* NEM目前对多重签名（multisig）的实现是 **"M-of-N"**，意味着M可以是等于或小于N的任何数字，即，1-of-2,4-of-2,4-of-9，9-of-10，等等。
* 多重签名（multisig）帐户最多可以有10个签名者。
* 一个帐户最多可以是5个多重帐户的签名者。
* 批准(approve)交易和删除(remove)共同签名的最小共同签署者(cosignature)的数量是 **可更改的**。
* multisig帐户可以有另一个multisig作为协同者，最多3个级别。

## 解决办法

### 新角色：仓库操作员2

为新的仓库操作员创建一个账户

{% highlight bash %}
  nem2-cli account generate --network MIJIN_TEST --url http://localhost:3000 --profile operator2 --save
{% endhighlight %}

### 安全部门和仓库：Multisig（多重签名）帐户

<strong class='tit'>1\.创建两个帐户来分别代表仓库和安全部门。</strong>

您可以在导航栏的右上角执行``Multisig Service``服务。

![screenshot-multisig-service]({{ site.baseurl }}/assets/images/screenshot-multisig-service.png)

<strong class='tit'>2\. 打开 ``project/dashboard/src/app/services/multisig.service.ts`` ，找到``createMultisig`` 函数。</strong>

<strong class='tit'>3\. 使用以下代码替换警报</strong>

{% highlight typescript %}
createMultisigAccountTransaction(multisigCandidate: PublicAccount,
                               minApproval: number,
                               minRemoval: number,
                               cosignatories: PublicAccount[]): AggregateTransaction {

    const newCosignatories = cosignatories.map( cosignatory => {
      const publicAccount = PublicAccount.createFromPublicKey(cosignatory.publicKey, NetworkType.MIJIN_TEST);
      return new MultisigCosignatoryModification(MultisigCosignatoryModificationType.Add, publicAccount);
    });

    const modifyMultisigAccountTransaction = ModifyMultisigAccountTransaction.create(
      Deadline.create(),
      minApproval,
      minRemoval,
      newCosignatories,
      NetworkType.MIJIN_TEST);

    return AggregateTransaction.createComplete(
      Deadline.create(),
      [modifyMultisigAccountTransaction.toAggregate(multisigCandidate)],
      NetworkType.MIJIN_TEST,
      []);
}
{% endhighlight %}
现在我们创建一个``ModifyMultisigAccountTransaction（修改多重签名账户交易）``，为``minApproval（最小剔除人数）``设置参数，并将cosignatories（共同签署者）列表设置为``MultisigCosignatoryModification``。

<strong class='tit'>4\. 返回仪表盘，将 **仓库** 帐户转换为1-of-2 multisig（多重签名）, 将 **仓库管理者1和仓库管理者2设置为cosignatories（共同签署者）**。</strong>

![screenshot-multisig-service-warehouse]({{ site.baseurl }}/assets/images/screenshot-multisig-service-warehouse.png)

<strong class='tit'>5\. 接下来，将 **安全部门** 的帐户转换为2-of-2 multisig（多重签名）。把 **仓库和传感器的帐户设置为cosignatories（共同签署者）** 。</strong>

![screenshot-multisig-service-company]({{ site.baseurl }}/assets/images/screenshot-multisig-service-safety-department.png)

<strong class='tit'>6\. 为 ``safety deparment（安全部门）``账户上加载1.000 ``company.safety:seal``。</strong>

{% highlight bash %}
$> nem2-cli transaction transfer --profile company
Introduce the recipient address: SCSG23-J2BVLU-S4JA4N-ZKMYKV-JR5LTK-M76KJ7-Q2V3
Introduce the mosaics in the format namespaceName:mosaicName::absoluteAmount, add multiple mosaics splitting them with a comma:
> company.safety:seal::1000

{% endhighlight %}

### 修改汇总事务

<strong class='tit'>1\. 打开``project/dashboard/src/app/services/safetySeal.service.ts``文件, 找到``createSafetySealTransaction``函数。</strong>

<strong class='tit'>2\.把 ``safety deparment(安全部门)``的公共帐户添加到汇总绑定交易的必需签名者中去。</strong>

只有不是multisig（多重签名）帐户的帐户才能发布和签署交易。因此，我们现在选择通过操作员的帐户发布交易。


{% highlight typescript %}
createSafetySealTransaction(productAddress: Address, operatorAccount: PublicAccount): AggregateTransaction {

    const safetyDeparmentAccountPublicKey = ''; // Todo: Paste company account public key
    const safetyDeparmentPublicAccount = PublicAccount.createFromPublicKey(safetyDeparmentAccountPublicKey, NetworkType.MIJIN_TEST);

    const safetyDepartmentToProductTransaction = TransferTransaction.create(
      Deadline.create(),
      productAddress,
      [new Mosaic(new MosaicId('company.safety:seal'), UInt64.fromUint(1))],
      EmptyMessage,
      NetworkType.MIJIN_TEST
    );

    return AggregateTransaction.createBonded(
      Deadline.create(),
      [safetyDepartmentToProductTransaction.toAggregate(safetyDeparmentPublicAccount)],
      NetworkType.MIJIN_TEST);
}
{% endhighlight %}

<strong class='tit'>3\.打开``project/server/.env``文件, 添加安全部门的地址。</strong>

{% highlight bash %}
SAFETY_DEPARTMENT_ADDRESS='SCSG23-J2BVLU-S4JA4N-ZKMYKV-JR5LTK-M76KJ7-Q2V3'
{% endhighlight %}

这是必需的的步骤，因为传感器监听着待签名的汇总绑定事务。

## 测试修改

<strong class='tit'>1\. 重新启动服务器以应用更改。</strong>

<strong class='tit'>2\. 发布一个安全封条</strong>

<strong class='tit'>3\. 转到产品详细信息页面。现在产品通过安全测试了吗？用不同的产品重复几次，直到其中一个获得安全封条。</strong>
