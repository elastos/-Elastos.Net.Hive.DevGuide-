# Introduction

Hive Node 服务节点是Hive去中心化存储网络的服务节点，并需要24\*7常驻运行，通过前端SDKs向用户和应用提供丰富的数据存储服务。Hive Node是开放源代码项目，任何社区成员和机构组织根据安装指南进行部署运行。

Hive Node service node is the service node of Hive decentralized memory network, and it needs to run 24\*7 hours a day, providing rich data memory services to users and applications through front-end SDKs. Hive Node is an open-source project, and any community member and institutions can deploy and run according to the installation guide.

所有运行的Hive Node节点构成整个Hive存储网络。任何Hive存储结构提供的功能特性都是一致的，但是由于部署时使用不同的资源配置，所提供的服务质量应该也有差异。

All the running Hive Node nodes constitute the whole Hive memory network. The functional characteristics provided by any Hive memory structure are consistent, but the quality of service provided should be different due to different resource configurations during deployment.

## Internal Services

Hive Node 后台运行由以下一组子服务组成：

Hive Node background operation consists of the following sub-services:

* Http Service
* IPFS node
* MongoDB service

其中Http Servic负责与前端Hive SDKs通过Http协议响应数据存储请求。Hive Node会将文件数据存储到本地的IPFS Node，而将非结构化数据交由MongoDB服务负责存储。

Http Service is responsible for responding to data memory requests with front-end Hive SDKs through Http protocol. Hive Node will save the file data to the local IPFS Node, while the unstructured data will be saved by MongoDB service.

本章主要向社区人员和用户指导如何搭建部署Hive Node节点：

This chapter mainly guides community personnel and users how to build and deploy Hive Node nodes:

* 节点配置
* Node configuration
* 节点部署
* Node deployment

以及提供 Http Restful APIs 接口规范，期望社区人员实现更多语言版本的前端SDKs库，甚至不同语言版本的Hive Node实现。

And provide Http Restful APIs interface specification, expecting community personnel to realize more language versions of front-end SDKs library, and even different language versions of Hive Node.

* HTTP API Reference.

## Repository

Hive Node 是 OpenSource 项目实现，代码仓库托管在 Github，请参见这里： https://github.com/elastos/Elastos.Hive.Node

Hive Node is the implementation of OpenSource project, and the code repository is hosted in Github. See:https://github.com/elastos/Elastos.Hive.Node

## Supports

Elastos Hive Node are supported by Trinity Tech and the community developers through GitHub issues. Also, we would love to get your contributions, whether they are in form of bug reports, Requests for Enhancement (RFE), documentation, or code patches.

## License

```
Copyright (c) 2021 Elastos Foundation @ MIT

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
