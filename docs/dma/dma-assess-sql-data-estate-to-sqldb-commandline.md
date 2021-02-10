---
title: DMACMD：评估要迁移到 Azure SQL SQL Server 准备情况
titleSuffix: Data Migration Assistant
description: 了解如何使用数据迁移助手命令行工具 (DMACMD) 评估要迁移到 Azure SQL 的 SQL Server 数据空间
ms.date: 10/02/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: ''
ms.openlocfilehash: 8688fa61a1ccaede3bd370e1d19ccd66531f5739
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060932"
---
# <a name="dmacmd-assess-readiness-of-a-sql-server-data-estate-migrating-to-azure-sql"></a>DMACMD：评估迁移到 Azure SQL 的 SQL Server 数据空间的准备情况 

在尝试迁移到 Azure 的多个组织中，评估现有的本地 SQL Server 实例并识别 azure Vm 上的 azure sql 数据库、Azure SQL 托管实例或 SQL Server 至关重要，这一点非常重要。 

[数据迁移助手 (DMA) ](dma-overview.md) 可帮助评估特定 Azure sql 目标的 SQL Server 实例，并将 SQL Server 数据库迁移到 Azure sql 的准备工作。 将 DMA 评估结果上载到 Azure Migrate 中心，以获得整个数据空间的集中式就绪性视图。 

本文介绍如何使用 DMA 命令行界面 (DMACMD) ，大规模执行评估，并将结果上传到 Azure Migrate 中心。 或者，您可以使用 [DMA GUI](dma-assess-sql-data-estate-to-sqldb.md) 来执行评估。

若要了解详细信息，请参阅以下 Channel9 视频：

>
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/How-to-Assess-Readiness-of-SQL-Server-Data-Estate-Migrating-to-Azure-SQL/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a>必备条件 

若要使用 DMACMD 执行评估并将结果上传到 Azure Migrate 中心，需要以下各项： 

- [数据迁移助手 (DMA) 最新版本](https://www.microsoft.com/en-us/download/details.aspx?id=53595)。
- [Azure Migrate 项目](dma-assess-sql-data-estate-to-sqldb.md#create-a-project-and-add-a-tool)。 
- 参与者角色对 Azure Migrate 项目资源的访问权限。

## <a name="use-dmacmd"></a>使用 DMACMD

使用 XML 文件作为输入，使用命令行界面 ( # A0) 在大规模运行评估。 

使用以下示例命令将 XML 文件传递给 DMACMD 并开始评估：

```console
C:\Program Files\Microsoft Data Migration Assistant\DmaCmd.exe /Action=Assess /AssessmentConfiguration= C:\Demo\ScaleAssessment\Assess-for-AzureSQLMI.xml
```

示例的内容 `Assess-for-AzureSQLMI.xml` 定义用于评估 SQL 托管实例目标 SQL Server 实例的元素： 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<AssessmentConfiguration xmlns="http://microsoft.com/schemas/SqlServer/Advisor/AssessmentConfiguration">
   <AssessmentName>Scale-Assessment-for-AzureSQLManagedInstance</AssessmentName>
   <AssessmentSourcePlatform>SqlOnPrem</AssessmentSourcePlatform>
   <AssessmentTargetPlatform>ManagedSqlServer</AssessmentTargetPlatform>
   <AssessmentDatabases>
      <AssessmentDatabase>Server=ServerName\SQL2017;Integrated Security=true</AssessmentDatabase>
      <AssessmentDatabase>Server=ServerName\SQL2016;Integrated Security=true;Initial Catalog=AdventureWorks2016</AssessmentDatabase>
      <AssessmentDatabase>Server=ServerName\SQL2016;Integrated Security=true;Initial Catalog=TestDB</AssessmentDatabase>
   </AssessmentDatabases>
   <AssessmentResultDma>C:\Demo\ScaleAssessment\AssessmentConfiguration\Scale-Assessment-for-AzureSQLManagedInstance.dma</AssessmentResultDma>
   <AssessmentResultJson>C:\Demo\ScaleAssessment\AssessmentConfiguration\Scale-Assessment-for-AzureSQLManagedInstance.json</AssessmentResultJson>
   <AssessmentResultCsv>C:\Demo\ScaleAssessment\AssessmentConfiguration\Scale-Assessment-for-AzureSQLManagedInstance.csv</AssessmentResultCsv>
   <AssessmentOverwriteResult>true</AssessmentOverwriteResult>
   <AssessmentEvaluateCompatibilityIssues>true</AssessmentEvaluateCompatibilityIssues>
   <AssessmentEvaluateFeatureParity>true</AssessmentEvaluateFeatureParity>
   <AzureCloudEnvironment>Azure</AzureCloudEnvironment>
   <SubscriptionId>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx</SubscriptionId>
   <AzureMigrateProjectName>Scale-Assessment-for-AzureSQLMI</AzureMigrateProjectName>
   <ResourceGroupName>Resource-Group-Name</ResourceGroupName>
   <AzureAuthenticationInteractiveAuthentication>true</AzureAuthenticationInteractiveAuthentication>
   <AzureAuthenticationTenantId>xxxxxxxx-xxxx-xxxxxxxx</AzureAuthenticationTenantId>
   <EnableAssessmentUploadToAzureMigrate>true</EnableAssessmentUploadToAzureMigrate>
</AssessmentConfiguration>
```



## <a name="xml-elements"></a>XML 元素 

下表中定义了传递到 DMACMD 的 XML 元素： 


|**XML 元素** |**定义**  |
|---------|---------|
|`AssessmentName`|评估的名称|
|`AssessmentSourcePlatform`|源 SQL Server 平台。 默认值为 `SqlOnPrem`。|
|`AssessmentTargetPlatform`|目标 SQL Server 平台。  </br> `AzureSqlDatabase` 适用于 Azure SQL 数据库目标。 </br> `ManagedSqlServer` 适用于 Azure SQL 托管实例目标。 </br></br>示例 **AzureSQLMI** 评估 SQL 托管实例目标。|
|`AssessmentDatabases`|如果需要评估实例中的所有数据库，请仅指定实例名称，或者在每行中列出特定的数据库。 </br></br>示例 **AzureSQLMI** 评估实例中的所有数据库 `Servername\SQL2017` 和实例中的两个特定数据库 `Servername\SQL2016` 。  |
|`AssessmentResultDma` </br> `AssessmentResultJson` </br> `AssessmentResultCsv` | 指定结果文件的格式。 `.DMA`分别是、 `.JSON` 和 `.CSV` 。 双击 `.DMA` 以在 DMA UI 中打开。 <br> `AssessmentResultDma` 需要将评估结果上载到 Azure Migrate 中心。  |
|`AssessmentOverwriteResult`| 指示是否用与、或相同的路径覆盖任何现有评估结果 `AssessmentResultJson` 文件 `AssessmentResultDma` `AssessmentResultCsv` 。|
|`AssessmentEvaluateCompatibilityIssues` </br> `AssessmentEvaluateFeatureParity` |执行评估，分别评估兼容性问题和功能奇偶校验问题。|
|`AzureCloudEnvironment`|要连接到的 azure 云环境，默认为 Azure 公有云。 </br></br> 支持的值： </br>`Azure (default)`, `AzureChina`, `AzureGermany`, `AzureUSGovernment`.|
|`SubscriptionId`|Azure 订阅 ID。|
|`AzureMigrateProjectName`|要将评估结果上载到 Azure Migrate 项目名称。|
|`ResourceGroupName`|Azure Migrate 资源组名称。|
|`AzureAuthenticationInteractiveAuthentication`|设置为 `true` 以弹出身份验证窗口。|
|`AzureAuthenticationTenantId`|Azure Active Directory 租户 ID。 </br></br>从 [Azure 门户](https://portal.azure.com)中 Azure Active Directory 的 "**概述**" 边栏选项卡获取此项。 |
|`EnableAssessmentUploadToAzureMigrate`| 将设置为，将 `true` 评估结果上载并发布到 Azure Migrate 中心。|
|   |   |


## <a name="results"></a>结果 

DMACMD 成功完成后，将输出状态。 


下面是一个示例结果输出： 

```console
Assessment finished for project: Scale-Assessment-for-AzureSQLManagedInstance
DATABASES:
Succeeded             : 4
Failed                : 0
SERVER INSTANCES:
Succeeded             : 2
Failed                : 0
CSV result file       : C:\Demo\ScaleAssessment\Scale-Assessment-for-AzureSQLManagedInstance.csv
JSON result file      : C:\Demo\ScaleAssessment\Scale-Assessment-for-AzureSQLManagedInstance.json
--------------------------------------------------------------------------------
```

查看上传的结果，以便为整个数据空间的集中视图 [Azure Migrate](dma-assess-sql-data-estate-to-sqldb.md#view-target-readiness-assessment-results) 。 . 

## <a name="best-practices"></a>最佳做法 

使用 DMACMD 时，请考虑以下最佳做法： 

- 逻辑上根据应用程序将目标 SQL Server 实例和数据库组合在一起，而不是对整个数据空间中的所有 SQL Server 实例进行评估。 
- 为每个 Azure SQL 目标创建一个单独的 Azure Migrate 项目，以避免覆盖结果。 
- 运行评估的时间取决于数据库对象的数量。 如果可能，请避免在生产系统上运行评估并将其卸载到虚拟机或过渡服务器，尤其是对于具有大量对象的数据库。 


## <a name="see-also"></a>另请参阅

* [数据迁移助手 (DMA)](../dma/dma-overview.md)
* [数据迁移助手：配置设置](../dma/dma-configurationsettings.md)
* [数据迁移助手：最佳实践](../dma/dma-bestpractices.md)

