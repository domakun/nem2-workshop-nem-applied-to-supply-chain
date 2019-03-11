---
layout: post
title:  "你的第一次公证"
permalink: /lessons/first-notarization/
---

Alice的电脑上有一份文件，她想证明这份文件在某个特殊时间点已经存在且她拥有文件访问权限。这份文件可以是任意有公证价值的数字文件。比如有价值的合同，电影剧本或专利。

在第一次尝试中,您将在区块链中保存文件的全部内容。一般来说，我们不推荐在区块链上储存大量文件数据。第一种方法将指导我们在以下练习中升级解决方案。

## 背景

NEM基于[account（账户）](https://nemtech.github.io/concepts/account.html)，由[transaction(交易)](https://nemtech.github.io/concepts/transaction.html)驱动。一个账户具备以下权能:
  * 持有资产；
  * 拥有唯一标识性；
  * 通过向其他账户发布[assets（资产）](https://nemtech.github.io/concepts/mosaic.html)交易和信息来改变区块链状态。

一个账户主要由三个部分组成：

  * **Private key（私钥）**: 一串对整个账户持有控制权限的字符串。
  * **Public key（公钥）**: 公钥由私钥派生，可网络中识别您的帐户。与任何人分享您的公钥是安全的。
  * **Address（地址）**: 地址由公钥派生，你将经常选择对外分享派生的地址，因为地址更简短并且包含了更多信息。

![diagram-notarization]({{ site.baseurl }}/assets/images/diagram-notarization.png)

## 实现方法

现在我们将创建两个账户

* A: Alice的账户；
* B: Alice用于保存公证的钱包。

接下来，A将对B发布一次交易，并把要公证的文件内容添加为消息。
如果这次交易有效，它将被保存在[块（block）](https://nemtech.github.io/concepts/block.html)里。在顶部添加新块（block）时，您可以想到，交易将在区块链中长久保存。
在区块链中执行操作会产生费用，因为向检验和保护网络的人提供激励是必要的。费用将通过NEM网络的基础加密货币XEM来支付。

    ℹ️ 在私有网络（private network）中，您可以将交易费用设置为0。

现在我们不创建新帐户，而是使用已有XEM的帐户。

<strong class='tit'>1\. 打开命令行并进入到已下载Catapult Bootstrap Service的目录。</strong>
{% highlight bash %}

$> cd  build/generated-addresses/
$> cat addresses.yaml
{% endhighlight %}

在nemesis_addresses下，您可以找到包含XEM的密钥对。

在NEM2-CLI中将第一个帐户作为配置文件加载，这个帐户代表了Alice。

  {% highlight bash %}
  $> nem2-cli profile create
  Introduce network type (MIJIN_TEST, MIJIN, MAIN_NET, TEST_NET): MIJIN_TEST
  Introduce NEM 2 Node URL. (Example: http://localhost:3000): http://localhost:3000
  Insert profile name (blank means default and it could overwrite the previous profile): alice
  New Account:    SBFSSN-MF5DOC-Q7S62D-ALYPXT-6KZM44-RT367B-VWSE
  Public Key:     3F2842ABC234D068B06625D01224D9E62D9767C79E4DF7BB5F562869DC6539FD
  Private Key:    6D...80
  {% endhighlight %}

为Alice的公证（notarizations）创建新的配置文件。

  {% highlight bash %}
  $> nem2-cli account generate
  Introduce network type (MIJIN_TEST, MIJIN, MAIN_NET, TEST_NET): MIJIN_TEST
  Do you want to save it? [y/n]: y
  Introduce NEM 2 Node URL. (Example: http://localhost:3000): http://localhost:3000
  Insert profile name (blank means default and it could overwrite the previous profile): alice_notarizations_wallet
  New Account:    SC5ZOIOYKHKEIJOXEFJQK5KRRE5KNZFDZTZA43BI
  Public Key:     EBEC67B604C1C549674B4B021228A0DE8975A29071E496FCCE4446DEBA15F20B
  Private Key:    41...F6
  {% endhighlight %}


您可以通过运行以下任意方式获取帐户的信息：

  {% highlight bash %}
  $> nem2-cli profile list
  {% endhighlight %}

<strong class='tit'>2\. 创建一个新的 .txt 文件,并在里面输入"Hello World"。需要注意的是，NEM中信息（messages）长度需要控制在``1024``个字符以内。</strong>

您可以选择把内容拆分为多个传输交易（transactions），但最好不要在区块链中存储大量数据。在下一个练习中，您将看到如何解决这个问题。

<strong class='tit'>3\. 在浏览器中打开 [公证仪表板（Notarization dashboard）](http://localhost:4200/) 您将看到以下内容:</strong>

![screenshot-notarization-panel]({{ site.baseurl }}/assets/images/screenshot-notarization-panel.png)

您是否已经尝试使用面板进行公证？到现在为止，它仍然无法正常工作。接下来您将编写NEM集成代码。

<strong class='tit'>4\. 回顾``project/src/app/components/createNotarization.component.ts``中的 `notarize()`函数</strong>

您将可以看到公证交易（transaction）已创建，然后由帐户签名并最终在网络上发布。

{% highlight typescript %}
  notarize(form) {
    const account = Account.createFromPrivateKey(form.privateKey, NetworkType.MIJIN_TEST);
    const recipient = Address.createFromRawAddress(form.address);
    const message = PlainMessage.create(form.message);

    const notarization = this.notarizationService.createNotarizationTransaction(recipient, message);
    const signedNotarization = account.sign(notarization!);

    [..]

    this.transactionHttp
      .announce(signedNotarization)
      .subscribe(announcedTransaction => console.log(announcedTransaction),
       err => console.log(err);
  }
{% endhighlight %}


一旦帐户发布交易，服务器将返回OK响应。

交易消息传输到网络后立即获得``未确认状态（unconfirmed status）``状态。只有当交易（transaction）信息保存在一个块（block）中时，它才会收到确认。

收到OK响应并不意味着交易有效。我们建议在宣布交易有效前密切监控交易状态。

{% highlight typescript %}

    this.listener.open().then(() => {

      this.listener
        .status(account.address)
        .pipe(
          filter((transaction) => transaction.hash === signedNotarization.hash)
        )
        .subscribe(errorStatus => {
          this.progress = {'message': errorStatus.status, 'code': 'ERROR'};
        }, err => this.progress = {'message': err, 'code': 'ERROR'});

      this.listener
        .confirmed(account.address)
        .pipe(
          filter((transaction) =>
            transaction.transactionInfo !== undefined
            && transaction.transactionInfo.hash === signedNotarization.hash)
        )
        .subscribe(ignored => {
          this.progress = {'message': 'Notarization confirmed with hash ' + signedNotarization.hash, 'code': 'CONFIRMED'};
        }, err => this.progress = {'message': err, 'code': 'ERROR'});

        [...]
    });
  }

{% endhighlight %}


<strong class='tit'>5\. 打开 ``project/src/app/services/notarization.service.ts`` 并通过``createNotarizationTransaction``函数创建转让交易（transaction）。</strong>
一次交易需要：

  * **Deadline**（最后期限）:在交易确定保存在块（block）里之前，有多少块（blocks）可以传输;
  * **Recipient**（接受者）:Alice将要发送公证的账户地址；
  * **Message**(消息):采用文件内容作为消息（message）；
  * **Network**（网络）:在本次练习中我们使用MIJIN_TESTNET。

{% highlight typescript %}
  createNotarizationTransaction(recipient: Address, message: PlainMessage) : TransferTransaction{

    return TransferTransaction.create(
      Deadline.create(),
      recipient,
      [],
      message,
      NetworkType.MIJIN_TEST
    );
  }
{% endhighlight %}


<strong class='tit'>6\.保存更改并返回到公证面板。拖入你的.txt文件,并粘贴 Alice的公证钱包地址(B)和Alice的私钥(A)来签署公证。接下来,点击``Notarize``将公证发布在网络上。</strong>

![screenshot-public-notarization]({{ site.baseurl }}/assets/images/screenshot-public-notarization.png)

<strong class='tit'>7\. 等待状态消息表示 **"Transaction confirmed"** 后,将交易的哈希码拷贝到新的文本文件中。</strong>

![screenshot-notarization-confirmed]({{ site.baseurl }}/assets/images/screenshot-notarization-confirmed.png)
