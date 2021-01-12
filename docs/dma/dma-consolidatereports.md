---
title: 使用数据迁移助手评估企业并整合评估报告
description: 了解如何在升级 SQL Server 或迁移到 Azure SQL 数据库之前，使用 DMA 评估企业并合并评估报告。
ms.date: 06/21/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 4d1bcf2cdc9997ea33be87184c38447b7cd62973
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091603"
---
# <a name="assess-an-enterprise-and-consolidate-assessment-reports-with-dma"></a>使用 DMA 评估企业和合并评估报告

以下分步说明可帮助你使用数据迁移助手来执行用于升级本地 SQL Server 或 SQL Server 在 Azure Vm 上运行或迁移到 Azure SQL 数据库的已成功缩放评估。

## <a name="prerequisites"></a>必备知识

- 指定网络上的工具计算机，DMA 将从该计算机启动。 确保此计算机已连接到 SQL Server 目标。
- 下载和安装：
  - [数据迁移助手](https://www.microsoft.com/download/details.aspx?id=53595) 3.6 或更高版本。
  - [PowerShell](https://aka.ms/wmf5download) 5.0 或更高版本。
  - [.NET Framework](https://www.microsoft.com/download/details.aspx?id=30653) 4.5 或更高版本。
  - [SSMS](../ssms/download-sql-server-management-studio-ssms.md) 17.0 或更高版本。
  - [Power BI 桌面](/power-bi/fundamentals/desktop-get-the-desktop)。
  - [Azure PowerShell 模块](/powershell/azure/install-az-ps)
- 下载并解压缩：
  - [DMA 报告 Power BI 模板](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/4/PowerBI-Reports.zip)。
  - [LoadWarehouse 脚本](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/3/LoadWarehouse1.zip)。

## <a name="loading-the-powershell-modules"></a>加载 PowerShell 模块

将 PowerShell 模块保存到 PowerShell 模块目录中，可调用这些模块，而无需在使用之前显式加载这些模块。

若要加载模块，请执行以下步骤：

1. 导航到 C:\Program Files\WindowsPowerShell\Modules，并创建名为 **DataMigrationAssistant** 的文件夹。
2. 打开 [PowerShell 模块](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/1/PowerShell-Modules2.zip)，然后将它们保存到所创建的文件夹中。

      ![PowerShell 模块](../dma/media//dma-consolidatereports/dma-powershell-modules.png)

    每个文件夹都包含关联的 hbase-runner.psm1 文件，如下图所示：

   ![PowerShell 模块 hbase-runner.psm1 文件](../dma/media//dma-consolidatereports/dma-powershell-modules-psm1-files.png)

   > [!NOTE]
   > 它包含的文件夹和 hbase-runner.psm1 文件必须具有相同的名称。

   > [!IMPORTANT]
   > 将 PowerShell 文件保存到 WindowsPowerShell 目录后，可能需要取消阻止 PowerShell 文件，以确保正确加载这些模块。 若要取消阻止 PowerShell 文件，请右键单击该文件，选择 " **属性**"，选择 " **取消阻止** " 文本框，然后选择 **"确定"**。

   ![hbase-runner.psm1 文件属性](../dma/media//dma-consolidatereports/dma-psm1-file-properties.png)

    PowerShell 现在应在新的 PowerShell 会话启动时自动加载这些模块。

## <a name="create-an-inventory-of-sql-servers"></a><a name="create-inventory"></a> 创建 SQL Server 清单

在运行 PowerShell 脚本以评估你的 SQL Server 之前，你需要生成要评估的 SQL 服务器的清单。

此清单可采用以下两种形式之一：

- Excel CSV 文件
- SQL Server 表

### <a name="if-using-a-csv-file"></a>如果使用 CSV 文件

> [!IMPORTANT]
> 确保清单文件以逗号分隔 (CSV) 文件格式保存。
>
> 对于默认实例，请将实例名称设置为 MSSQLServer。

使用 csv 文件导入数据时，请确保数据 **实例名称** 和 **数据库名称** 只有两列，并且这些列没有标头行。

 ![csv 文件内容](../dma/media//dma-consolidatereports/dma-csv-file-contents.png)

### <a name="if-using-a-sql-server-table"></a>如果使用 SQL Server 表

> [!IMPORTANT]
> 对于默认实例，请将实例名称设置为 MSSQLServer。

创建名为 **EstateInventory** 的数据库和名为 **DatabaseInventory** 的表。 包含此清单数据的表可以具有任意数量的列，只要存在以下四列：

- ServerName
- InstanceName
- DatabaseName
- AssessmentFlag

![如果正在使用 SQL Server 表，则为 SQL Server 表内容的屏幕截图。](../dma/media//dma-consolidatereports/dma-sql-server-table-contents-database-inventory.png)

如果此数据库不在工具计算机上，请确保工具计算机具有与此 SQL Server 实例的网络连接。

对 CSV 文件使用 SQL Server 表的好处是，你可以使用 "评估标志" 列控制为评估选取的实例/数据库，这样可以更轻松地将评估拆分为较小的区块。  然后，你可以跨多个评估 (请参阅本文后面的运行评估一节) ，这比维护多个 CSV 文件更容易。

请记住，根据对象的数量和复杂性，评估可能会花费很长时间 (小时 +) ，因此，将评估划分为可管理的区块是明智之举。

### <a name="if-using-an-instance-inventory"></a>如果使用实例清单

创建名为 **EstateInventory** 的数据库和名为 **InstanceInventory** 的表。 包含此清单数据的表可以具有任意数量的列，只要存在以下四列：

- ServerName
- InstanceName
- 端口
- AssessmentFlag

![如果使用实例清单，则为 SQL Server 表内容的屏幕截图。](../dma/media//dma-consolidatereports/dma-sql-server-table-contents-instance-inventory.png)

## <a name="running-a-scaled-assessment"></a>运行缩放评估

将 PowerShell 模块加载到模块目录并创建清单后，需要通过打开 PowerShell 并运行 dmaDataCollector 函数来运行缩放评估。

  ![dmaDataCollector 函数列表](../dma/media//dma-consolidatereports/dma-dmaDataCollector-function-listing.png)

下表描述了与 dmaDataCollector 函数关联的参数。

|参数  |描述 |
|---------|---------|
|**getServerListFrom** | 你的清单。 可能的值为 **SqlServer** 和 **CSV**。<br/>有关详细信息，请参阅 [创建 SQL server 清点](#create-inventory)。 |
|**csvPath** | CSV 清单文件的路径。  仅当 **getServerListFrom** 设置为  **CSV** 时使用。 |
|**serverName** | 在 **getServerListFrom** 参数中使用 **SqlServer** 时清单的 SQL Server 实例名称。 |
|**databaseName** | 承载库存表的数据库。 |
|**useInstancesOnly** | 位标志，用于指定是否使用实例列表进行评估。  如果设置为0，则 DatabaseInventory 表将用于生成评估目标列表。 |
|**AssessmentName** | DMA 评估的名称。 |
|**TargetPlatform** | 要执行的评估目标类型。  可能的值包括 **AzureSQLDatabase**、 **ManagedSqlServer**、 **SQLServer2012**、 **2014**、 **sqlserver2016-ssei-expr**、 **SQLServerLinux2017**、 **SQLServerWindows2017**、  **SqlServerWindows2019** 和 **SqlServerLinux2019**。  |
|**AuthenticationMethod** | 用于连接到要评估的 SQL Server 目标的身份验证方法。 可能的值为 **和 sqlauth** 和 **WindowsAuth**。 |
|**OutputLocation** | 要在其中存储 JSON 评估输出文件的目录。 评估可能需要很长时间，具体取决于要评估的数据库数和数据库中的对象数。 所有评估完成后，文件将写入。 |

如果出现错误，则将终止此进程启动的命令窗口。  查看错误日志以确定失败的原因。

  ![错误日志位置](../dma/media//dma-consolidatereports/dma-error-log-file-location.png)

## <a name="consuming-the-assessment-json-file"></a>使用评估 JSON 文件

完成评估后，便可以将数据导入到 SQL Server 进行分析。 若要使用评估 JSON 文件，请打开 PowerShell 并运行 dmaProcessor 函数。

  ![dmaProcessor 函数列表](../dma/media//dma-consolidatereports/dma-dmaProcessor-function-listing.png)

下表描述了与 dmaProcessor 函数关联的参数。

|参数  |描述 |
|---------|---------|
|**processTo** | 将处理 JSON 文件的位置。 可能的值为 **SQLServer** 和 **AzureSQLDatabase**。 |
|**serverName** | 数据将处理到的 SQL Server 实例。  如果为 **processTo** 参数指定 **AzureSQLDatabase** ，则仅包含 SQL Server 名称 (不包含 database.windows.net) 。 面向 Azure SQL 数据库时，系统将提示你提供两个登录名;第一种是 Azure 租户凭据，第二种是 Azure SQL Server 的管理员登录名。 |
|**CreateDMAReporting** | 要创建的用于处理 JSON 文件的临时数据库。  如果指定的数据库已经存在，并且将此参数设置为1，则不会创建对象。  此参数可用于重新创建已删除的单个对象。 |
|**CreateDataWarehouse** | 创建 Power BI 报表将使用的数据仓库。 |
|**databaseName** | DMAReporting 数据库的名称。 |
|**warehouseName** | 数据仓库数据库的名称。 |
|**jsonDirectory** | 包含 JSON 评估文件的目录。  如果目录中有多个 JSON 文件，则逐个处理它们。 |

DmaProcessor 函数只需几秒钟即可处理一个文件。

## <a name="loading-the-data-warehouse"></a>正在加载数据仓库

DmaProcessor 完成对评估文件的处理后，数据将加载到 ReportData 表中的 DMAReporting 数据库中。 此时，需要加载数据仓库。

1. 使用 LoadWarehouse 脚本填充维度中缺少的任何值。

    此脚本将从 DMAReporting 数据库中的 ReportData 表获取数据，并将其加载到仓库。  如果在此加载过程中出现任何错误，则这些错误可能是由于维度表中的条目丢失而导致的。

2. 加载数据仓库。

  ![已加载 LoadWarehouse 内容](../dma/media//dma-consolidatereports/dma-load-warehouse-loaded.png)

## <a name="set-your-database-owners"></a>设置数据库所有者

虽然不是必需的，但若要从报表中获得最大的价值，建议你在 " **dimDBOwner** " 维度中设置数据库所有者，然后在 **FactAssessment** 表中更新 **DBOwnerKey** 。  按照此过程操作，可以根据特定数据库所有者对 Power BI 报表进行切片和筛选。

还可以使用 LoadWarehouse 脚本提供用于设置数据库所有者的基本 TSQL 语句。

  ![LoadWarehouse 设置所有者](../dma/media//dma-consolidatereports/dma-load-warehouse-set-owners.png)

## <a name="dma-reports"></a>DMA 报表

1. 在 Power BI Desktop 中打开 DMA 报表 Power BI 模板。
2. 输入指向 **DMAWarehouse** 数据库的服务器详细信息，然后选择 " **加载**"。

   ![已加载 Power BI 模板的 DMA 报表](../dma/media//dma-consolidatereports/dma-reports-powerbi-template-loaded.png)

   在报表刷新 **DMAWarehouse** 数据库中的数据后，会看到如下所示的报表。

   ![DMAWarehouse 报表视图](../dma/media//dma-consolidatereports/dma-DMAWarehouse-report1.png)

   > [!TIP]
   > 如果看不到预期的数据，请尝试更改活动书签。  有关详细信息，请参阅下一节中的详细信息。

## <a name="working-with-dma-reports"></a>使用 DMA 报表

若要使用 DMA 报表，请使用书签和切片器进行筛选：

- 评估类型 (Azure SQL 数据库、Azure SQL 托管实例 SQL Server)  
- Instance Name
- 数据库名称
- 团队名称

若要访问 "书签和筛选器" 边栏选项卡，请在主报表页上选择 "筛选器" 书签：

![DMA 报表书签和筛选器](../dma/media//dma-consolidatereports/dma-report-bookmarks-filters.png)

选择筛选器书签会启用以下边栏选项卡：

![DMA 报告视图边栏选项卡](../dma/media//dma-consolidatereports/dma-report-views-blade.png)

您可以使用书签来切换报表上下文：

- Azure SQL 数据库云评估
- Azure SQL 托管实例云评估
- 本地评估

![DMA 报表视图书签](../dma/media//dma-consolidatereports/dma-report-bookmarks1.png)

若要隐藏 "筛选器" 边栏选项卡，请按 CTRL 并单击 "后退" 按钮：

![DMA 报表视图后退按钮](../dma/media//dma-consolidatereports/dma-report-bookmarks-back.png)

在报表页的左下角有一个提示，用于显示筛选器当前是否适用于以下任何项：

- FactAssessment – InstanceName
- FactAssessment – DatabaseName
- dimDBOwner-DBOwner

![筛选器已应用提示](../dma/media//dma-consolidatereports/dma-filter-applied-prompt.png)

> [!NOTE]
> 如果仅执行 Azure SQL 数据库评估，则仅填充云报表。 相反，如果仅执行本地评估，则仅填充本地报表。 但是，如果执行 Azure 和本地评估，然后将这两个评估加载到仓库中，则可以通过按 CTRL 并单击关联的图标来在云报表和本地报表之间切换。

## <a name="reports-visuals"></a>报表视觉对象

Power BI 报表中显示的详细信息如以下部分所示。

### <a name="readiness-"></a>就绪

  ![DMA 就绪百分比](../dma/media//dma-consolidatereports/dma-readiness-percentage.png)

此视觉对象根据选择上下文进行更新， ("所有"、"实例"、"数据库 [倍数 of] ) "。

### <a name="readiness-count"></a>就绪计数

  ![DMA 就绪计数](../dma/media//dma-consolidatereports/dma-readiness-count.png)

此视觉对象显示已准备好迁移尚未准备好迁移的数据库计数的数据库计数。

### <a name="readiness-bucket"></a>准备情况存储桶

  ![DMA 准备情况存储桶](../dma/media//dma-consolidatereports/dma-readiness-bucket.png)

此视觉对象按以下准备情况存储段显示数据库细目：

- 100% 就绪
- 75-99% 就绪
- 50-75% 就绪
- 尚未就绪

### <a name="issues-word-cloud"></a>颁发 Word Cloud

  ![DMA 问题 WordCloud](../dma/media//dma-consolidatereports/dma-issues-word-cloud.png)

此视觉对象显示当前在选择上下文中出现的问题， (所有内容、实例、数据库 [倍数 of] ) 。 屏幕上显示的字符越大，该类别中的问题就越多。 将鼠标指针悬停在某个字上将显示该类别中出现的问题数。

### <a name="database-readiness"></a>数据库准备情况

  ![DMA 数据库准备情况报告](../dma/media//dma-consolidatereports/dma-database-readiness-report.png)

此部分是报表的主要部分，显示实例数据库的准备情况。 此报告具有以下内容的向下钻取层次结构：

- InstanceDatabase
- ChangeCategory
- 标题
- ObjectType
- ImpactedObjectName

 ![DMA 数据库准备情况报表深化](../dma/media//dma-consolidatereports/dma-database-readiness-report-drilldown.png)

此报表还充当用于创建修正计划报表的筛选点。

若要深入了解更正计划报告，请右键单击此图中的数据点，指向 " **钻取**"，然后选择 " **修正计划**"。

此任务将根据你选择钻取选项的点，将修正计划报表筛选为当前层次结构级别。

  ![已筛选的 DMA 数据库准备情况报表深化](../dma/media//dma-consolidatereports/dma-database-readiness-report-drilldown-filtered.png)

  ![DMA 修正计划报告](../dma/media//dma-consolidatereports/dma-remediation-plan-report.png)

还可以通过使用 " **可视化筛选器** " 边栏选项卡中的筛选器自行使用修补计划报表来生成自定义修正计划。

  ![DMA 修正计划报表筛选器选项](../dma/media//dma-consolidatereports/dma-remediation-plan-report-filter-options.png)

### <a name="script-disclaimer"></a>脚本声明

*任何 Microsoft 标准支持计划或服务均不支持本文中提供的示例脚本。所有脚本都按原样提供，无任何形式的保证。Microsoft 进一步否认所有默示保证，包括但不限于适销性或特定用途适用性的任何默示保证。由于示例脚本和文档的使用或性能而产生的全部风险仍随你一起提供。在任何情况下，Microsoft 及其作者或者，无论是由于使用还是不能使用示例脚本或文档而导致的任何损害（包括但不限于业务利润损失、业务中断、业务信息丢失或其他 pecuniary 丢失) ，无论是由于使用还是不能使用示例脚本或文档，而导致的任何损害）都有责任，无论是否有可能导致此类损害，都应承担任何其他责任。 (搜寻权限，然后在其他站点/存储库/博客上重新发布这些脚本。*