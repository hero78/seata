<img src="https://github.com/fescar-group/fescar-samples/blob/master/doc/img/seata.png"  height="100" width="426">

# Seata: Simple Extensible Autonomous Transaction Architecture

[![Build Status](https://travis-ci.org/seata/seata.svg?branch=develop)](https://travis-ci.org/seata/seata)
[![codecov](https://codecov.io/gh/seata/seata/branch/develop/graph/badge.svg)](https://codecov.io/gh/seata/seata)
![license](https://img.shields.io/github/license/seata/seata.svg)
![maven](https://img.shields.io/maven-central/v/com.alibaba.fescar/fescar-all.svg)

## What is Seata?

A **distributed transaction solution** with high performance and ease of use for **microservices** architecture.

### Distributed Transaction Problem in Microservices

Let's imagine a traditional monolithic application. Its business is built up with 3 modules. They use a single local data source.

Naturally, data consistency will be guaranteed by the local transaction.

![Monolithic App](https://cdn.nlark.com/lark/0/2018/png/18862/1545296770244-4cedf37e-9dc6-4fc0-a97f-f4240b9d8640.png) 

Things have changed in microservices architecture. The 3 modules mentioned above are designed to be 3 services on top of 3 different data sources ([Pattern: Database per service](http://microservices.io/patterns/data/database-per-service.html)). Data consistency within every single service is naturally guaranteed by the local transaction. 

**But how about the whole business logic scope?**

![Microservices Problem](https://cdn.nlark.com/lark/0/2018/png/18862/1545296781231-4029da9c-8803-43a4-ac2f-6c8b1e2ea448.png) 

### How Seata do?

Seata is just a solution to the problem mentioned above. 

![Seata solution](https://cdn.nlark.com/lark/0/2018/png/18862/1545296791074-3bce7bce-025e-45c3-9386-7b95135dade8.png)

Firstly, how to define a **Distributed Transaction**?

We say, a **Distributed Transaction** is a **Global Transaction** which is made up with a batch of **Branch Transaction**, and normally **Branch Transaction** is just **Local Transaction**.

![Global & Branch](https://cdn.nlark.com/lark/0/2018/png/18862/1545015454979-a18e16f6-ed41-44f1-9c7a-bd82c4d5ff99.png) 

There are 3 basic components in Seata: 

- **Transaction Coordinator(TC):** Maintain status of global and branch transactions, drive the global commit or rollback.
- **Transaction Manager(TM):** Define the scope of global transaction: begin a global transaction, commit or rollback a global transaction.
- **Resource Manager(RM):** Manage resources that branch transactions working on, talk to TC for registering branch transactions and reporting status of branch transactions, and drive the branch transaction commit or rollback.

![Model](https://cdn.nlark.com/lark/0/2018/png/18862/1545013915286-4a90f0df-5fda-41e1-91e0-2aa3d331c035.png) 

A typical lifecycle of Seata managed distributed transaction:

1. TM asks TC to begin a new global transaction. TC generates an XID representing the global transaction.
2. XID is propagated through microservices' invoke chain.
3. RM register local transaction as a branch of the corresponding global transaction of XID to TC. 
4. TM asks TC for committing or rollbacking the corresponding global transaction of XID.
5. TC drives all branch transactions under the corresponding global transaction of XID to finish branch committing or rollbacking.

![Typical Process](https://cdn.nlark.com/lark/0/2018/png/18862/1545296917881-26fabeb9-71fa-4f3e-8a7a-fc317d3389f4.png) 

For more details about principle and design, please go to [Seata wiki page](https://github.com/seata/seata/wiki). 

### History

- **TXC**: Taobao Transaction Constructor. Alibaba middleware team start this project since 2014 to meet distributed transaction problem caused by application architecture change from monolithic to microservices.
- **GTS**: Global Transaction Service. TXC as an Aliyun middleware product with new name GTS was published since 2016.
- **Fescar**: we start the open source project Fescar based on TXC/GTS since 2019 to work closely with the community in the future.

## Maven dependency
```xml
<fescar.version>0.4.0</fescar.version>

<dependency>
    <groupId>com.alibaba.fescar</groupId>
    <artifactId>fescar-spring</artifactId>
    <version>${fescar.version}</version>
</dependency>

<!--dependency for Apache Dubbo-->
<dependency>
    <groupId>com.alibaba.fescar</groupId>
    <artifactId>fescar-dubbo</artifactId>
    <version>${fescar.version}</version>
</dependency>

<!--dependency for Alibaba Dubbo-->
<dependency>
    <groupId>com.alibaba.fescar</groupId>
    <artifactId>fescar-dubbo-alibaba</artifactId>
    <version>${fescar.version}</version>
</dependency>

```
## Quick Start

[Quick Start](https://github.com/seata/seata/wiki/Quick-Start)

## Documentation

You can view the full documentation from the wiki: [Seata wiki page](https://github.com/seata/seata/wiki).

## Reporting bugs

Please follow the [template](https://github.com/seata/seata/blob/develop/.github/ISSUE_TEMPLATE/BUG_REPORT.md) for reporting any issues.


## Contributing

Contributors are welcomed to join the FEATS project. Please check [CONTRIBUTING](./CONTRIBUTING.md) about how to contribute to this project.


## Contact

* [Twitter](): TBD. Follow along for latest Fescar news on Twitter.

* Mailing list: 
  * dev-fescar@googlegroups.com , for dev/user discussion. [subscribe](mailto:dev-fescar+subscribe@googlegroups.com), [unsubscribe](mailto:dev-fescar+unsubscribe@googlegroups.com), [archive](https://groups.google.com/forum/#!forum/dev-fescar)
  
**Dingtalk**

<img src="https://upload-images.jianshu.io/upload_images/4420767-4e95b186a1a1bfba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240"  height="300" width="300">


## Seata ecosystem

* [Seata Ecosystem Entry](https://github.com/fescar-group) - A GitHub group `seata-group` to gather all Seata relevant projects not appropriate in [seata](https://github.com/seata) group yet
* [Seata Samples](https://github.com/fescar-group/fescar-samples) - Samples for Seata
* [Seata Docker](https://github.com/fescar-group/fescar-docker) - Seata integration with docker
* [Seata K8s](https://github.com/fescar-group/fescar-k8s) - Seata integration with k8s
* [Awesome Seata](https://github.com/fescar-group/awesome-fescar) - Description of Seata related projects 
* [Seata Website](https://github.com/fescar-group/fescar.io) - Seata official website （***In the process of design***）

## Contributors

This project exists thanks to all the people who contribute. [[Contribute](CONTRIBUTING.md)].
<a href="https://github.com/seata/seata/graphs/contributors"><img src="https://opencollective.com/fescar/contributors.svg?width=890&button=false" /></a>

## License

Seata is under the Apache 2.0 license. See the [LICENSE](https://github.com/seata/seata/blob/master/LICENSE) file for details.
