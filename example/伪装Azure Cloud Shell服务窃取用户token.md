---
tags: 云原生安全资讯,云漏洞案例
version: v0.1.0
---

## 伪装Azure Cloud Shell服务窃取用户token

* 原文地址：https://blog.lightspin.io/azure-cloud-shell-command-injection-stealing-users-access-tokens

### 1.1 简介

lightspin公司的研究员[Gafnit Amiga](https://blog.lightspin.io/author/gafnit-amiga)，报告了Azure云Cloud Shell服务的一个用户凭据窃取漏洞。攻击者可以利用Azure App Service 伪造Cloud Shell服务，诱导受害者访问，窃取其token。这是一个用于钓鱼的漏洞，但整个过程中，受害者很难发现被攻击，因为其间利用了用户对Azure的信任、Azure对部分域名的信任。

### 1.2 利用过程

1. 攻击者利用Azure App Service 注册 cloudshell-df.azurewebsites.net 域名，并伪装成Azure Cloud Shell服务
2. 诱导受害者访问 https://cloudshell-df.azurewebsites.net/
3. js代码通过postMessage向加载的cloudshell frame发送message，cloudshell会把message存储在浏览器的localStorage的InjectedCommands字段
4. 浏览器跳转至cloudshell的真正域名 https://portal.azure.com/#cloudshell/
5. cloudshell服务执行存储在浏览器localStorage的InjectedCommands字段，这个字段在第3步被设置为窃取用户token的命令
6. 用户的token被发送至攻击者

### 1.3 根本原因

与老生常谈的S3域名抢占问题类似，Serverless服务也存在域名抢占问题，如果某个云服务信任的域名可以被攻击者抢占，则可能存在利用空间。

### 1.4 发现过程

作者详细的介绍了漏洞具体的发现过程，值得细读。这里提炼其大致过程：

**1. iframe**
cloudshell既可以作为一个单独的服务访问，也可以被嵌入至其他云服务页面，方便用户在相关服务随时启动一个shell。

cloudshell作为iframe被嵌入到云服务页面，iframe地址为 `https://ux.console.azure.com?region=westeurope&trustedAuthority=https%3A%2F%2Fportal.azure.com&l=en.en-us&feature.azureconsole=true`。其中，`ux.console.azure.com`为cloudshell的实际地址，`portal.azure.com`是嵌入cloudshell的云服务地址。

注意到参数`trustedAuthority`，就是嵌入cloudshell的云服务的域。这里存在一个信任关系，即cloudshell信任部分域，允许被嵌入。能否修改这个参数，在攻击者控制的域名嵌入cloudshell呢？

**2. 信任域**

作者通过搜索前端代码，发现信任域包括：
```
var allowedParentFrameAuthorities = ["localhost:3000", "localhost:55555", "localhost:6516", "azconsole-df.azurewebsites.net", "cloudshell-df.azurewebsites.net", ...];
```

其中`cloudshell-df.azurewebsites.net`没有被占用，用户可以在Azure App Service 注册。于是，攻击者可以在`cloudshell-df.azurewebsites.net`域名通过iframe加载`ux.console.azure.com`。

**3. postMessage**

在cloudshell被云服务作为iframe加载后，云服务可能需要cloudshell执行某些操作，包括传递token，传递配置，执行命令等。这个操作是通过浏览器的postMessage机制实现的。作者分析前端代码发现，允许执行的命令包括`wget, go, ./aztfy`等。这些命令将被cloudshell存储在浏览器的localstorage中，将来cloudshell会执行这些命令。

### 1.5 时间线

* 2022-08-20: 漏洞报告给MSRC
* 2022-08-24: MSRC确认漏洞，奖金为$10,000
* 2022-08-29: 微软修复漏洞
* 2022-09-20: 作者发布文章披露详细过程

### 1.6 总结

#### 1.6.1 未来的域名抢占问题

cloudshell信任的域名还有很多个，如果将来某个域名被弃用，但未在cloudshell的列表里删除，则又存在同样的问题。

#### 1.6.2 彻底解决的方法

我认为，要彻底解决这类域名抢占的问题，需要能区分内、外部服务，可以为云厂商内部服务设置保留关键字，如xxx-internal.csp.com, 不允许外部用户注册相关关键字。
