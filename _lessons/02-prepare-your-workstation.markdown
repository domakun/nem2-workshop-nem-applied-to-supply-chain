---
layout: post
title:  "准备工作环境"
permalink: /lessons/prepare-your-workstation/
---

![4-layered-architecture]({{ site.baseurl }}/assets/images/four-layer-architecture.png)

## 运行投石机服务器引导

**Catapult Server nodes**（第1层）用于构建对等的区块链网络。 **Catapult Rest nodes**（第2层）用于提供应用程序访问区块链或区块链功能的API网关。

您可以在不到5分钟的时间里通过[投石机服务器指南](https://github.com/tech-bureau/catapult-service-bootstrap)来学习 **在本地运行私有链** 。此服务将在本地运行Catapult server实例和Catapult REST nodes。

<strong class='tit'>1\. 在运行以下命令前，确认您已经安装了[docker](https://docs.docker.com/install/) 和 [docker compose](https://docs.docker.com/compose/install/)</strong>

{% highlight bash %}
git clone https://github.com/tech-bureau/catapult-service-bootstrap.git --branch v0.1.0
cd catapult-service-bootstrap
docker-compose up
{% endhighlight %}

<strong class='tit'>2\. 下载好映像并运行服务后，请先检查是否可以获取第一个块信息：</strong>

{% highlight bash %}
curl localhost:3000/block/1
{% endhighlight %}


## 下载项目文件
本次学习以项目为基础，您将为现有项目添加一些新功能。

<strong class='tit'>1\. 下载相关资源</strong>

{% highlight bash %}
git clone https://github.com/nemtech/nem2-workshop-nem-applied-to-supply-chain.git
{% endhighlight %}

在``project``文件夹下，有一个``dashboard(仪表板)``和一个使用Express框架的小型``server（服务器）``。它们都安装了 **NEM2 Software Development Kit（NEM2软件开发套件）**（第3层）。NEM2-SDK是用于创建其他工具，库或应用程序等NEM2组件的主要软件开发工具。

在本次学习中，我们选择使用 **Typescript SDK**。


<strong class='tit'>2\. 全局安装 **typescript**</strong>

{% highlight bash %}
npm install -g typescript
{% endhighlight %}

<strong class='tit'>3\. 启动``server（服务器）``.</strong>

{% highlight bash %}
cd <name>/project/server
npm install
npm start
{% endhighlight %}

<strong class='tit'>4\. 打开新的命令行,并启动``dashboard（仪表板）``.</strong>

{% highlight bash %}
cd <name>/project/dasbhoard
npm install
npm start
{% endhighlight %}

## 安装NEM2-CLI

**NEM2-CLI** 能帮助您在终端中方便地使用常用命令，您可以用它与区块链进行交互，建立帐户，发送资金等。

通过npm安装 **nem2-cli**

{% highlight bash %}
npm i -g nem2-cli
{% endhighlight %}
