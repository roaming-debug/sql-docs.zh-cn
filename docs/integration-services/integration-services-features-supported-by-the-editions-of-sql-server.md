---
description: SQL Server 各个版本支持的 Integration Services 功能
title: SQL Server 各个版本支持的 Integration Services 功能 | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: e5018225-68bb-4f34-ae4a-ead79d8ad13a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2a8b60c36831622713103786727c4225ac0c351c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095638"
---
# <a name="integration-services-features-supported-by-the-editions-of-sql-server"></a>SQL Server 各个版本支持的 Integration Services 功能

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


 本主题提供有关不同版本的 [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)]所支持的 SQL Server Integration Services (SSIS) 功能的详细信息。  

有关开发人员版和评估版支持的功能，请参阅下表中列出的企业版功能。
  
有关最新的发布说明和新增功能的信息，请参阅以下各项：
-   [SQL Server 2016 发行说明](../sql-server/sql-server-2016-release-notes.md)
-   [SQL Server 2016 Integration Services 中的新增功能](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md)
-   [SQL Server 2017 Integration Services 中的新增功能](../integration-services/what-s-new-in-integration-services-in-sql-server-2017.md)
    
**试用 SQL Server 2016！**    

可在 180 天的试用期内使用 SQL Server 评估版。  
    
> [![从评估中心下载](/analysis-services/analysis-services/media/download.png)](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) [从评估中心下载 SQL Server 2016](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)    
    
## <a name="new-integration-services-features-in-sql-server-2017"></a><a name="ISNew"></a>SQL Server 2017 中的新 Integration Services 功能
  
|Feature|Enterprise|Standard|Web|Express with Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Scale Out 主要角色|是|||||
|Scale Out 辅助角色|是|是 <sup>1</sup>|TBD|TBD|TBD|
|支持 OData 组件 <sup>2</sup> 中的 Microsoft Dynamics AX 和 Microsoft Dynamics CRM|是|是||||
|Linux 支持|是|是|||是|

<sup>1</sup> 如果在 Scale Out 中运行的包需要仅限企业版的功能，则 Scale Out Worker 必须也在 SQL Server 企业版实例上运行。

<sup>2</sup> SQL Server 2016 Service Pack 1 中也支持此功能。

## <a name="sql-server-import-and-export-wizard"></a><a name="IEWiz"></a>SQL Server 导入和导出向导

|Feature|Enterprise|Standard|Web|Express with Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|SQL Server 导入和导出向导|是|是|是|是<sup>1</sup>|是<sup>1</sup>|

<sup>1</sup> Linux 上的 SQL 不提供 DTSWizard.exe。 但是，可以使用 Linux 上的 dtexec 来执行 Windows 上的 DTSWizard 创建的包。


## <a name="integration-services"></a><a name="IS"></a> Integration Services  
  
|Feature|Enterprise|Standard|Web|Express with Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|内置数据源连接器|是|是|||| 
|内置任务和转换|是|是||||  
|ODBC 源和目标 |是|是|||| 
|Azure 数据源连接器和任务|是|是||||  
|Hadoop/HDFS 连接器和任务|是|是||||  
|基本数据探查工具|是|是|||| 

## <a name="integration-services---advanced-sources-and-destinations"></a><a name="ISAA"></a>Integration Services - 高级源和目标  
  
|Feature|Enterprise|Standard|Web|Express with Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Attunity 提供的高性能 Oracle 源和目标|是|||||  
|Attunity 提供的高性能 Teradata 源和目标|是|||||  
|SAP BW 源和目标|是|||||  
|数据挖掘模型定型目标|是|||||  
|维度处理目标|是|||||  
|分区处理目标|是|||||  
  
## <a name="integration-services---advanced-tasks-and-transformations"></a><a name="ISAT"></a>Integration Services - 高级任务和转换  
  
|Feature|Enterprise|Standard|Web|Express with Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Attunity <sup>1</sup> 提供的变更数据捕获组件|是|||||  
|数据挖掘查询转换|是|||||  
|模糊分组和模糊查找转换|是|||||  
|术语提取和术语查找转换|是|||||  

<sup>1</sup> 由 Attunity 提供的变更数据捕获组件需要企业版。 不过变更数据捕获服务和变更数据捕获设计器不需要企业版。 可以在未安装 SSIS 的计算机上使用设计器和服务。