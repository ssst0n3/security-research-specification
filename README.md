# Security Research Specification

## Vulnerability Research

* [vulnerability report](./vulnerability-research/vulnerability-report.md)
  * example: TODO
* [漏洞挖掘报告](./vulnerability-research/%E6%BC%8F%E6%B4%9E%E6%8C%96%E6%8E%98%E6%8A%A5%E5%91%8A.md)
  * 例子: TODO
* [漏洞分析与复现](./vulnerability-research/%E6%BC%8F%E6%B4%9E%E5%88%86%E6%9E%90%E4%B8%8E%E5%A4%8D%E7%8E%B0.md)
  * 例子: TODO

## 安全使用

* [安全加固](./%E5%AE%89%E5%85%A8%E4%BD%BF%E7%94%A8/%E5%AE%89%E5%85%A8%E5%8A%A0%E5%9B%BA.md)
    * 例子: TODO

## Cloud Native Security News

* [Researcher Introduction](./cloud-native-security-news/researcher-introduction.md)
* [Security Conference Talk Learning](./cloud-native-security-news/security-conference-talk-learning.md)
* [云漏洞案例](./cloud-native-security-news/%E4%BA%91%E6%BC%8F%E6%B4%9E%E6%A1%88%E4%BE%8B.md)
    * [例子：伪装Azure Cloud Shell服务窃取用户token](./example/%E4%BC%AA%E8%A3%85Azure%20Cloud%20Shell%E6%9C%8D%E5%8A%A1%E7%AA%83%E5%8F%96%E7%94%A8%E6%88%B7token.md)
    * [例子：Azure：从子域名接管到内部账户接管](./example/Azure%EF%BC%9A%E4%BB%8E%E5%AD%90%E5%9F%9F%E5%90%8D%E6%8E%A5%E7%AE%A1%E5%88%B0%E5%86%85%E9%83%A8%E8%B4%A6%E6%88%B7%E6%8E%A5%E7%AE%A1.md)
* [项目推荐](cloud-native-security-news/项目推荐.md)
    * [例子: 工具推荐：kubeletctl](./example/%E5%B7%A5%E5%85%B7%E6%8E%A8%E8%8D%90%EF%BC%9Akubeletctl.md)

## CHANGELOG

### 安全加固

| version | change                    | date       |
|---------|---------------------------|------------|
| v0.2.0  | 增加基线metadata信息；增加章节四：加固案例 | 2023-02-15 |
| v0.1.0  | 初始版本                      | 2023-02-02 |

### Vulnerability Research

#### Vulnerability Report

| version | change           | date       |
|---------|------------------|------------|
| v0.1.0  | Initial Research | 2023-03-30 |


#### 漏洞挖掘报告

| version | change      | date       |
|---------|-------------|------------|
| v0.2.0  | 新增章节6. 时间线  | 2023-02-03 |
| v0.1.2  | 修订：细化1.基本信息 | 2023-02-03 |
| v0.1.1  | 初始版本        | 2023-02-02 |

#### 漏洞分析与复现

| version | change                                                               | date       |
|---------|----------------------------------------------------------------------|------------|
| v0.2.19 | 修订：文首添加changelog                                                     | 2023-05-10 |
| v0.2.18 | 修订：三、漏洞作者：update to v0.1.9                                           | 2023-05-04 |
| v0.2.17 | 修订：三、漏洞作者：update to v0.1.8                                           | 2023-04-11 |
| v0.2.16 | 修订：三、漏洞作者：update to v0.1.7                                           | 2023-04-10 |
| v0.2.15 | 修订：三、漏洞作者：update to v0.1.6                                           | 2023-04-10 |
| v0.2.14 | 修订：文首添加文档进度                                                          | 2023-04-06 |
| v0.2.13 | 修订：文末添加规范链接                                                          | 2023-04-06 |
| v0.2.12 | 修订：三、漏洞作者：update to v0.1.5                                           | 2023-03-30 |
| v0.2.11 | translate directory name '云原生安全资讯' to 'cloud-native-security-news'   | 2023-03-25 |
| v0.2.10 | 修订：三、漏洞作者：update to v0.1.3                                           | 2023-03-20 |
| v0.2.9  | 修订：三、漏洞作者：update to v0.1.2                                           | 2023-03-16 |
| v0.2.8  | 修订：三、漏洞作者：直接引用research-introduction；更新至v0.1.1                        | 2023-03-15 |
| v0.2.7  | 修订：三、漏洞作者：增加book条目                                                   | 2023-03-15 |
| v0.2.6  | 修订：三、漏洞作者：增加mastodon条目; 明确只统计目标项目下的commits                           | 2023-03-14 |
| v0.2.5  | 修订：三、漏洞作者：增加gitlab,reddit,facebook等条目                                | 2023-03-14 |
| v0.2.4  | 修订：三、漏洞作者：增加社交媒体条目                                                   | 2023-03-10 |
| v0.2.3  | 修订：三、漏洞作者：在各子标题下添加作者名称                                               | 2023-03-08 |
| v0.2.2  | 修订：三、漏洞作者-1. discoverer: 增加website条目                                 | 2023-03-08 |
| v0.2.1  | 修订：调整一、基本信息的顺序；增加Intelligence Gathering Date条目                       | 2023-03-08 |
| v0.2.0  | 新增章节：十一、漏洞情报；后续章节序号递延；增加metadata：doc-version                         | 2023-03-08 |
| v0.1.6  | 修订：细化章节“三、漏洞作者-3. fixer”内容                                           | 2023-01-31 |
| v0.1.5  | 修订：细化章节“三、漏洞作者-2. introducer”内容                                      | 2023-01-30 |
| v0.1.4  | 修订：增加“参考链接”章节; 增加“4. poc构造”章节的描述                                     | 2023-01-11 |
| v0.1.3  | 修订：在章节“七、漏洞分析”下增加“4. poc构造 [Optional]”；<br>将“1. 原始特性分析”标记为[Optional] | 2023-01-11 |
| v0.1.2  | 修订：在章节“四、漏洞详情/2.2 危害”添加CVSS打分                                        | 2023-01-11 |
| v0.1.1  | 修订：在章节“十二、遗留事项”中预置了对漏洞相关人跟踪的3项遗留工作                                   | 2023-01-09 |
| v0.1.0  | 初始版本                                                                 | 2023-01-09 |

### 云原生安全资讯

#### Researcher Introduction

| version | change                                                             | date       |
|---------|--------------------------------------------------------------------|------------|
| v0.1.9  | Add item dockerhub                                                 | 2023-05-04 |
| v0.1.8  | Add item instagram                                                 | 2023-04-11 |
| v0.1.7  | Add item blog                                                      | 2023-04-10 |
| v0.1.6  | Add item asciinema,article                                         | 2023-04-10 |
| v0.1.5  | Add item bitbucket                                                 | 2023-03-30 |
| v0.1.4  | translate directory name '云原生安全资讯' to 'cloud-native-security-news' | 2023-03-25 |
| v0.1.3  | Add item telegram                                                  | 2023-03-20 |
| v0.1.2  | Add item slideshare                                                | 2023-03-16 |
| v0.1.1  | Add item medium                                                    | 2023-03-15 |
| v0.1.0  | Initial Release                                                    | 2023-03-15 |

#### Security Conference Talk Learning

| version | change          | date       |
|---------|-----------------|------------|
| v0.1.0  | Initial Release | 2023-03-25 |

#### 云漏洞案例

| version | change                                                             | date       |
|---------|--------------------------------------------------------------------|------------|
| v0.1.1  | translate directory name '云原生安全资讯' to 'cloud-native-security-news' | 2023-03-25 |
| v0.1.0  | 初始版本                                                               | 2023-01-09 |

#### 项目推荐

| version | change                                                             | date       |
|---------|--------------------------------------------------------------------|------------|
| v0.2.1  | translate directory name '云原生安全资讯' to 'cloud-native-security-news' | 2023-03-25 |
| v0.2.0  | 新增章节1.5 Source Code[Optional]; 标记1.2 Why为 [Optional]               | 2023-03-03 |
| v0.1.0  | 初始版本                                                               | 2023-02-17 |