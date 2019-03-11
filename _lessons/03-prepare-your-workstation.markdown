---
layout: post
title:  "准备工作环境"
permalink: /lessons/prepare-your-workstation/
---

在开始写代码前，你需要：

  * 本地安装的测试投石机服务器；
  * 我们将要开展的项目；
  * 命令行客户端。

## 启动投石机服务器
您可以在不到5分钟的时间里通过[投石机服务器指南](https://github.com/tech-bureau/catapult-service-bootstrap)来学习 **在本地运行私有链** 。
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

<strong class='tit'>1\. 下载关联资源</strong>

  {% highlight bash %}
  git clone https://github.com/nemtech/nem2-workshop-document-notarization.git
  {% endhighlight %}

在 ``project`` 文件夹下的是您将要在工作空间中编辑的代码。它附带 **NEM2 Software Development Kit** (第三层)。NEM2-SDK是创建其他工具，库或应用程序等NEM2组件的主要软件开发工具。

在本次工作空间中，我们将使用 **Typescript SDK**。

<strong class='tit'>2\. 全局安装 **typescript**</strong>

  {% highlight bash %}
  npm install -g typescript
  {% endhighlight %}

<strong class='tit'>3\. 启动 ``project``</strong>

  {% highlight bash %}
  cd project
  npm install
  npm start
  {% endhighlight %}

## 安装NEM2-CLI

**NEM2-CLI** 能帮助您在终端中方便地使用常用命令，即使用它与区块链交互，设置和帐户，发送资金等。
通过npm安装 **nem2-cli**

  {% highlight bash %}
  npm i -g nem2-cli
  {% endhighlight %}
