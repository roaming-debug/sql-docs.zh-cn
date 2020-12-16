---
description: 复制代理可执行文件概念
title: 复制代理可执行文件概念 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- programming interfaces [SQL Server replication]
- programming [SQL Server replication], agents
- replication [SQL Server], agents and profiles
- agents [SQL Server replication], executables
- command prompt [SQL Server replication]
ms.assetid: cba476df-d4ea-44c9-bb86-81488971e328
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 41525854d161d029beae6e0956bc11e536ab813b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460300"
---
# <a name="replication-agent-executables-concepts"></a>复制代理可执行文件概念
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  可以使用下列方法以编程方式控制复制代理：  
  
-   使用 <xref:Microsoft.SqlServer.Replication> 命名空间中的托管代理编程接口。  
  
-   在命令提示符下使用提供的一组参数来调用代理可执行文件。  
  
 如果直接从命令提示符调用复制代理，则可以从批处理文件中的命令行脚本，以编程方式访问代理。 从命令提示符调用代理时，代理在调用代理或启动批处理文件的用户的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 安全帐户下运行。  
  
 可使用可执行文件运行下列复制代理的实例。  
  
-   [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
-   [复制日志读取器代理](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
-   [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
-   [复制队列读取器代理](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
-   [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
 调用复制代理时，您可以使用性能配置文件，以自动向代理可执行文件传递一组已定义的参数。 有关详细信息，请参阅 [Replication Agent Profiles](../../../relational-databases/replication/agents/replication-agent-profiles.md)。  
  
## <a name="examples"></a>示例  
 下面的示例说明如何从命令提示符调用复制代理。 复制代理也可以使用复制管理对象 (RMO) 来调用。 有关详细信息，请参阅[同步订阅（复制）](../../../relational-databases/replication/synchronize-data.md)。  
  
> [!NOTE]  
>  为便于阅读，这些示例中添加了换行符。 但在批处理文件中，命令必须位于一行中。  
  
### <a name="running-the-snapshot-agent"></a>运行快照代理  
 此示例批处理文件从命令提示符调用快照代理，以生成 **AdvWorksSalesOrdersMerge** 发布的快照。 （下面的脚本使用 [!INCLUDE[ssSQL15_md](../../../includes/sssql15-md.md)] 文件（版本 130）的路径。 你应当调整脚本以指向你的 [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] 版本的文件。）  
  
```  
REM -- Declare variables  
SET Publisher=%InstanceName%;  
SET PublicationDB=AdventureWorks2012;   
SET Publication=AdvWorksSalesOrdersMerge;   
  
REM --Start the Snapshot Agent to generate the snapshot for AdvWorksSalesOrdersMerge.  
"C:\Program Files\Microsoft SQL Server\130\COM\SNAPSHOT.EXE" -Publication %Publication%   
-Publisher %Publisher% -Distributor %Publisher% -PublisherDB %PublicationDB%   
-ReplicationType 2 -OutputVerboseLevel 1 -DistributorSecurityMode 1 ;  
  
```  
  
### <a name="running-the-distribution-agent"></a>运行分发代理  
 此示例批处理文件从命令提示符调用分发代理，从而不断地从 **AdvWorksProductTran** 发布向推送订阅服务器复制更改。  
  
```  
REM -- Declare the variables.  
SET Publisher=%instancename%;  
SET Subscriber=%instancename%;  
SET PublicationDB=AdventureWorks2012;  
SET SubscriptionDB=AdventureWorks2012Replica;   
SET Publication=AdvWorksProductsTran;  
  
REM -- Start the Distribution Agent with four subscription streams.  
REM -- The following command must be supplied without line breaks.  
"C:\Program Files\Microsoft SQL Server\130\COM\DISTRIB.EXE" -Subscriber %Subscriber%   
-SubscriberDB %SubscriptionDB% -SubscriberSecurityMode 1 -Publication %Publication%   
-Publisher %Publisher% -PublisherDB %PublicationDB% -Distributor %Publisher%   
-DistributorSecurityMode 1 -Continuous -SubscriptionType 0 -SubscriptionStreams 4 ;  
  
```  
  
### <a name="running-the-merge-agent"></a>运行合并代理  
 此示例批处理文件从命令提示符调用合并代理，以将请求订阅与 **AdvWorksSalesOrdersMerge** 发布同步。  
  
```  
REM -- Declare the variables.  
SET Publisher=%instancename%;  
SET Subscriber=%instancename%;  
SET PublicationDB=AdventureWorks2012;  
SET SubscriptionDB=AdventureWorks2012Replica;   
SET Publication=AdvWorksSalesOrdersMerge;  
  
REM --Start the Merge Agent with concurrent upload and download processes.  
REM -- The following command must be supplied without line breaks.  
"C:\Program Files\Microsoft SQL Server\130\COM\REPLMERG.EXE" -Publication %Publication%    
-Publisher %Publisher%  -Subscriber  %Subscriber%  -Distributor %Publisher%    
-PublisherDB %PublicationDB%  -SubscriberDB %SubscriptionDB% -PublisherSecurityMode 1    
-OutputVerboseLevel 2  -SubscriberSecurityMode 1  -SubscriptionType 1 -DistributorSecurityMode 1    
-Validate 3  -ParallelUploadDownload 1 ;  
  
```  
  
  
