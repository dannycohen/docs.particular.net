---
title: NServiceBus transports comparison
summary: Comparison of supported NServiceBus transports.
tags:
- Transports
---

NServiceBus has the concept of transport, a transport is the underlying infrastructure required to deliver a message from one endpoint to another. Both endpoints need to be configured to use the same transport.

NServiceBus supports the following transports:

* MSMQ;
* SQL Server;
* RabbitMQ;
* Microsoft Azure Service Bus;
* Microsoft Azure Storage Queues;

Choosing the correct transport can be challenging and the choice can depend on several factors, the following table list a set of transport attributes and details the support each transport provides:

 |MSMQ|SQL Server|RabbitMQ|Azure ServiceBus|Azure Storage Queues
|---              |---         |---               |---              |---                           |---
|Type|Store & Forward|Broker|Broker|Broker|Broker
|Local Transactions|&#10004;|&#10004;|&#10006;|&#10004;|&#10006;
|MSDTC|&#10004;|&#10004;|&#10006;|&#10006;|&#10006;
|Ordering|Fi-Fo|Fi-Fo|&#10006;|Fi-Fo \*|&#10006;
|De-Duplication|&#10006;|&#10006;|&#10006;|&#10004;\*|&#10006;
|Durable|&#10004;|&#10004;|&#10004;\*|&#10004;|&#10006;
|Delivery guarantee|Once|Once|At-Least-Once|At-Least-Once / At-Most-Once \*|At-Least-Once
|Dead-lettering|&#10004;|&#10006;|&#10004;|&#10004;|&#10006;
|poison messages support|&#10004;|&#10006;|&#10006;|&#10004;|&#10004;
|max queue size|All Queues on a single machine 4Gb (by default)|?|User defined|1Gb to 80Gb|200TB
|max message size|4Mb|?|Unlimited|256Kb|64Kb
|max message TTL|Unlimited|?|Unlimited|Unlimited|7 days
|max number of queues|HW Dependent|?|Unlimited|10,000\*|Unlimited
|max number of concurrent clients|Unlimited|?|Configurable|Unlimited|Unlimited
|Authentication|&#10006;|?|Plugin based|Symmetric key|Symmetric key
|Access control model|ACL|?|Host & Resource based|Delegated access via SAS tokens|RBAC via ACS
|HA support|&#10006;|User|&#10004;\*|&#10004;|&#10004;
|Required external SDK|&#10006;|&#10006;|RabbitMQ Client|Azure SDK|Azure SDK


### NServiceBus features

NServiceBus does its best to support all its built-in feature on all the supported transports, the following is the list of the NServiceBus major feature and the support provided on each transport:

  |MSMQ|SQL Server|RabbitMQ|Azure ServiceBus|Azure Storage Queues
|---              |---         |---               |---              |---                           |---
|Outbox|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|De-Duplication (When Outbox is enabled)|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|ServiceControl Integration|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|DataBus|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Gateway|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Timeouts|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|First Level Retries|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Second Level Retries|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Pub/Sub|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Sagas|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Express messages|&#10004;|&#10006;|&#10006;|&#10006;|&#10006;|


### MSMQ

Transport package: *not required, built-in the NServiceBus core*;

##### pros

* windows built-in;
* NSB built-in and default;
* DTC support;
* distributed by nature no Single Point of Failure in the system;
* post & forward support

##### cons

* DTC in heterogeneous network and/or with high latency;
* no scale out support built-in needs the distributor;
* elevated account to install and create queue;
* does not work with Active Directory integration enabled
	
##### IT/Ops Management tools:

* Windows management console;
* 3rd party queue explorer;

### SQL Server Transport

Transport package: [NServiceBus.SqlServer](https://www.nuget.org/packages/NServiceBus.SqlServer/)

##### pros

* scalable using SQL native solutions;
* supports the DTC;
* when the same database is also used as application data storage can benefit of local transactions only;
* support for SQL 2014 AlwaysOn;

##### cons

* any?

##### IT/Ops Management tools:

* SQL Server Management Studio;

### RabbitMQ

Transport package: [NServiceBus.RabbitMQ](https://www.nuget.org/packages/NServiceBus.RabbitMQ/)

##### pros:

* cluster, scale and shard support;
* cross network boundaries via shovels and/or federation;

##### cons

* no support for transactions;
* requires the Outbox thus requires all endpoints to run at least v5;
* requires Erlang on the server hosting the RabbitMQ broker;
* hard HA

##### IT/Ops Management tools:

* Built-in management web app;

### Azure ServiceBus

Transport package: [NServiceBus.Azure](https://www.nuget.org/packages/NServiceBus.Azure/)

##### pros

* fully managed by Azure;
* no maintenance;
* fully scaled and HA by Azure;
* local transactions

##### cons

* requires Azure, and to be used from on-premise connectivity to Azure;
* requires on the client side the Azure SDK whose versioning is not always easy

##### IT/Ops Management tools:

* Azure portal;
* PowerShell cmdlets;

### Azure Storage Queues

Transport package: [NServiceBus.Azure](https://www.nuget.org/packages/NServiceBus.Azure/)

##### pros

* fully managed by Azure;
* no maintenance;
* fully scaled and HA by Azure;
* cheap

##### cons

* no transactions;
* requires an Azure subscription, and to be used from on-premise connectivity to Azure;
* requires on the client side the Azure SDK whose versioning is not always easy;

##### IT/Ops Management tools:

* PowerShell cmdlets;
* 3rd party [Cerebrata](http://www.cerebrata.com) tools;
