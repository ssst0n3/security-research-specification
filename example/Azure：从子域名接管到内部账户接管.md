---
tags: 云原生安全资讯,云漏洞案例
version: v0.1.0
---

## 1. 云漏洞案例: Azure：从子域名接管到内部账户接管

* 原文地址: https://www.binarysecurity.no/posts/2022/11/azure-devops-takeover

### 1.1 简介

binarysecurity团队发现了azure的一个子域名接管漏洞，并且接管了Microsoft Disaster Recovery 账户。通常我们认为子域名接管的主要危害在于品牌受损、钓鱼等，但这篇文章介绍了另一种危害，即子域名可能接收业务流量，有时这些流量还会包含敏感信息。

### 1.2 发现过程

1. 作者发现 visualstudio.com 指向 cloudapp.azure.com 的一些子域名处于dangling状态，可以被接管
2. 作者通过Azure的云服务注册了`feedsprodwcus0drdr.westus2.cloudapp.azure.com`等域名，间接控制了`feedsprodwcus0dr.feeds.visualstudio.com`
3. 监控发送至`feedsprodwcus0drdr.westus2.cloudapp.azure.com`的流量，收到一些业务流量，其中包含Microsoft Disaster Recovery 账户的认证凭据。

### 1.3 根本原因

典型的子域名接管漏洞，即一些废弃的子域名，未解除DNS记录。

### 1.4 利用过程

作者发现了两种利用：

1. visualstudio.com的登陆请求支持callback，可以把请求转发至特定地址，可以用于钓鱼。 `hxxps://app.vssps.visualstudio.com/_signin?realm=app.vsaex.visualstudio.com&reply_to=https://feedsprodwcus0dr.feeds.visualstudio.com/&redirect=1`
2. 监控流量，可以收到一些业务流量，特别是包含认证凭据的流量。

### 1.5 时间线

* 2021-02-18: 作者向微软报告
* 2021-05-19: 微软认为子域名接管不属于奖励计划
* 2022-03-11: 微软修复漏洞

### 1.6 总结 

漏洞原理简单，但效果惊艳。