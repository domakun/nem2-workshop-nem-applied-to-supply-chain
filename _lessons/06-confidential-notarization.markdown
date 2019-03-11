---
layout: post
title:  "机密文件的公证"
permalink: /lessons/confidential-notarization/
---

在接下来的模块中,任何拥有区块链读取权限的人都可以看到带时间戳的文档内容。此外，字符长度的限制使得我们无法对大型文档进行公证。

现在，Alice想公证 **confidential document（机密文件）**，也就是说她希望将文件的内容保密。但是，她必须能够证明文件有效性且她是文件所有者。

## 背景

**hash function（哈希函数）** 用于将任意大小的数据映射到固定大小的数据。哈希函数只能以一种方式工作，这表示没有人能够恢复原始内容。例如，把sha256哈希算法应用于任何文件后，此文件将被转换为256位长度的字符串。

    ℹ️ 虽然无法恢复原始内容，但简单的单词和短句总是生成相同的256位哈希值。

![confidential-notarization]({{ site.baseurl }}/assets/images/diagram-confidential-notarization.png)

在本练习中，您将使用哈希函数对文件的内容进行操作。然后，我们可以发出具有固定长度哈希值的交易（transaction）。这可以实现使用在单次交易中对大文件添加时间戳。

## 实现方法

<strong class='tit'>1\. 打开`project/src/app/components/createNotarization.component.ts`, 并修改 ``onFileChange()``函数。此函数将在文件拖入公证面板后调用，将SHA256算法把哈希函数作用于文件的内容。</strong>

{% highlight typescript %}
  onFileChange(){
    this.notarizationService
      .readFile(this.file)
      .subscribe( message =>{
        this.notarizationForm.patchValue({'message': crypto.SHA256(message).toString(crypto.enc.Hex) });
        this.notarizationForm.get('message')!.markAsDirty();
      })
  }
{% endhighlight %}

<strong class='tit'>2\. 保存文件后，使用面板公证文档。</strong>

* **File**（文件）: test.txt；
* **Notarization address**（公证地址）: Alice的公证钱包地址；
* **Signer private key**（私钥签名）:  Alice的账户私钥。

![screenshot-private-notarization]({{ site.baseurl }}/assets/images/screenshot-private-notarization.png)

<strong class='tit'>3\. 在``Get Notarization``标签下搜索交易。</strong>

![screenshot-get-private-notarization]({{ site.baseurl }}/assets/images/screenshot-get-private-notarization.png)

每次将哈希函数应用于文件时，输出哈希应与存储在事务消息中的哈希值一致。
