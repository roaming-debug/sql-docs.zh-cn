---
title: 减轻生产服务器优化负荷 | Microsoft Docs
description: 了解数据库引擎优化顾问如何依赖于查询优化器分析工作负载并在 SQL Server 中提供优化建议。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- overhead [Database Engine Tuning Advisor]
- tuning overhead [SQL Server]
- reducing production server tuning load
- Database Engine Tuning Advisor [SQL Server], test servers
- test servers [Database Engine Tuning Advisor]
- production servers [SQL Server]
- offload tuning overhead [SQL Server]
ms.assetid: bb95ecaf-444a-4771-a625-e0a91c8f0709
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c8d71b13755f1412c87f0f9a6a663a4fa47c4675
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469178"
---
# <a name="reduce-the-production-server-tuning-load"></a>减轻生产服务器优化负荷
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问依赖于查询优化器分析工作负荷并提供优化建议。 在生产服务器上执行此分析会增加服务器负荷，并且可能会在优化会话过程中影响服务器的性能。 通过除了使用生产服务器以外，再使用一台测试服务器，可以减小在优化会话过程中对服务器负荷的影响。  
  
## <a name="how-database-engine-tuning-advisor-uses-a-test-server"></a>数据库引擎优化顾问如何使用测试服务器  
 使用测试服务器的传统方法是将所有数据从生产服务器复制到测试服务器，优化测试服务器，然后在生产服务器上实现建议。 此过程可以消除对生产服务器的性能影响，但这不是最佳解决方案。 例如，将大量数据从生产服务器复制到测试服务器可能消耗大量时间和资源。 此外，测试服务器硬件很少像生产服务器中部署的硬件那样功能强大。 优化进程依赖于查询优化器，而它生成的建议部分依赖于基础硬件。 如果测试服务器的硬件和生产服务器的硬件不相同， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问建议的质量就会降低。  
  
 为避免出现此类问题， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问通过将大部分优化负荷转移到测试服务器来优化生产服务器上的数据库。 它通过使用生产服务器硬件配置信息，而不是真正地将数据从生产服务器复制到测试服务器，来执行该操作。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问不会将实际数据从生产服务器复制到测试服务器中。 它仅复制元数据和必要的统计信息。  
  
 下列步骤概要介绍了用于在测试服务器上优化生产数据库的过程：  
  
1.  确保两台服务器上都存在要使用测试服务器的用户。  
  
     开始之前，请确保两台服务器上都存在要使用测试服务器来优化生产服务器上的数据库的用户。 这就需要您在测试服务器上创建用户及其登录帐户。 如果您在两台计算机上都是 **sysadmin** 固定服务器角色成员，将不需要执行此步骤。  
  
2.  优化测试服务器上的工作负荷。  
  
     若要优化测试服务器上的工作负荷，必须通过 **dta** 命令行实用工具使用 XML 输入文件。 在 XML 输入文件中，在 **TuningOptions** 父元素下使用 **TestServer** 子元素指定测试服务器的名称，并为其他子元素指定值。  
  
     在优化进程中，数据库引擎优化顾问将在测试服务器上创建 Shell 数据库。 若要创建此 Shell 数据库并对其进行优化，数据库引擎优化顾问需要在下列情况下调用生产服务器：  
  
    1.  [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问将元数据从生产数据库导入到测试服务器 Shell 数据库。 此元数据包括空表、索引、视图、存储过程和触发器等。 这使得对测试服务器 Shell 数据库执行工作负荷查询成为可能。  
  
    2.  [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问从生产服务器导入统计信息，以便查询优化器可以准确优化对测试服务器的查询。  
  
    3.  [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问从生产服务器导入指定处理器数和可用内存量的硬件参数，为查询优化器提供生成查询计划所需的信息。  
  
3.  在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问完成优化测试服务器 Shell 数据库后，将生成优化建议。  
  
4.  将通过优化测试服务器得到的建议应用于生产服务器。  
  
 下图显示了测试服务器和生产服务器方案：  
  
 ![数据库引擎优化顾问测试服务器用法](../../relational-databases/performance/media/testsvr.gif "数据库引擎优化顾问测试服务器用法")  
  
> [!NOTE]  
>  [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问图形用户界面 (GUI) 不支持测试服务器优化功能。  
  
## <a name="example"></a>示例  
 首先，请确保测试服务器和生产服务器上都存在要执行优化的用户。  
  
 将用户信息复制到测试服务器后，您即可在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问 XML 输入文件中定义测试服务器优化会话。 下面的 XML 输入文件示例演示如何使用 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问来指定测试服务器优化数据库。  
  
 在此示例中， `MyDatabaseName` 数据库在 `MyServerName`上进行优化。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本（即 `MyWorkloadScript.sql`）用作工作负荷。 此工作负荷包含对 `MyDatabaseName`执行的事件。 查询优化器对此数据库的大部分调用操作（作为优化进程的一部分发生）是由驻留在 `MyTestServerName`上的 Shell 数据库实现的。 Shell 数据库由元数据和统计信息构成。 此进程会将优化开销转移到测试服务器。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问使用此 XML 输入文件生成其优化建议时，它应只考虑索引 (`<FeatureSet>IDX</FeatureSet>`) 而不考虑分区，并且不需要在 `MyDatabaseName`中保留任何现有的物理设计结构。  
  
```  
<?xml version="1.0" encoding="utf-16" ?>  
<DTAXML xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="https://schemas.microsoft.com/sqlserver/2004/07/dta">  
  <DTAInput>  
    <Server>  
      <Name>MyServerName</Name>  
      <Database>  
        <Name>MyDatabaseName</Name>  
      </Database>  
    </Server>  
    <Workload>  
      <File>MyWorkloadScript.sql</File>  
    </Workload>  
    <TuningOptions>  
      <TestServer>MyTestServerName</TestServer>  
      <FeatureSet>IDX</FeatureSet>  
      <Partitioning>NONE</Partitioning>  
      <KeepExisting>NONE</KeepExisting>  
    </TuningOptions>  
  </DTAInput>  
</DTAXML>  
```  
  
## <a name="see-also"></a>另请参阅  
 [使用测试服务器的注意事项](../../relational-databases/performance/considerations-for-using-test-servers.md)   
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
