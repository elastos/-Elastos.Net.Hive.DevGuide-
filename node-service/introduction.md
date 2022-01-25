# Introduction

Hive Node service node is the service node of Hive decentralized memory network and needs to run consistently around the clock, providing rich data memory services to users and applications through frontend SDKs. Hive Node is an open-source project where any community member or institution can deploy and run according to the installation guide.

All the running Hive Node nodes constitute the whole Hive memory network. The functional characteristics provided by any Hive memory structure are consistent, but the quality of service provided should be different due to different resource configurations during deployment.

## Internal Services

The Hive Node background operation consists of the following sub-services:

* Http Service
* IPFS node
* MongoDB service

Http Service is responsible for responding to data memory requests with frontend Hive SDKs through the HTTP protocol. Hive Node will save the file data to the local IPFS Node, while the unstructured data will be saved by MongoDB service.

This chapter mainly guides community personnel and users how to build and deploy Hive Nodes:

* Node configuration
* Node deployment

And provide HTTP Restful APIs interface specification, expecting community personnel to realize more language versions of frontend SDKs library, and even different language versions of Hive Node.

* HTTP API Reference

## Repository

Hive Node is the implementation of OpenSource project, and the code repository is hosted in GitHub. Please see: [https://github.com/elastos/Elastos.Hive.Node](https://github.com/elastos/Elastos.Hive.Node)

## Supports

Elastos Hive Nodes are supported by Trinity Tech and the community developers through GitHub issues. We'd love to hear your feedback in any form, whether it's regarding bug reports, Requests for Enhancement (RFE), documentation, or code patches.

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
