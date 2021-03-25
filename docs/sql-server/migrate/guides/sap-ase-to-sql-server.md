---
title: 从 SAP ASE 迁移到 SQL Server：迁移指南
description: '本指南介绍如何使用适用于 SAP ASE 的 SQL Server 迁移助手 (SSMA for SAP ASE) 将 SAP ASE 数据库迁移到 Microsoft SQL Server。 '
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: a549b0e28da092bc1320f621c29307772fc5d69b
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104611028"
---
# <a name="migration-guide-sap-ase-to-sql-server"></a>迁移指南：从 SAP ASE 迁移到 SQL Server
[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

本指南介绍如何使用适用于 SAP ASE 的 SQL Server 迁移助手将 SAP ASE 数据库迁移到 SQL Server。

有关其他方案，请参阅[数据库迁移指南](https://datamigration.microsoft.com/)。

## <a name="prerequisites"></a>先决条件 

若要将 SAP SE 数据库迁移到 SQL Server，你需要：

- 验证源环境是否受支持。 
- [适用于 SAP Adaptive Server Enterprise（之前称为 SAP Sybase ASE）的 SQL Server 迁移助手](https://www.microsoft.com/download/details.aspx?id=54256)。 

## <a name="pre-migration"></a>预迁移

满足先决条件后，就可以发现环境的拓扑并评估迁移的可行性。

### <a name="assess"></a>评估

使用[适用于 SAP Adaptive Server Enterprise（之前称为 SAP Sybase ASE）的 SQL Server 迁移助手 (SSMA)](https://www.microsoft.com/download/details.aspx?id=54256) 来评审数据库对象和数据、评估数据库是否适合迁移、将 Sybase 数据库对象迁移到 SQL Server，然后将数据迁移到 SQL Server。 若要了解详细信息，请查看 [适用于 Sybase 的 SQL Server 迁移助手 (SybaseToSQL)](../../../ssma/sybase/sql-server-migration-assistant-for-sybase-sybasetosql.md)。

要创建评估，请执行以下步骤： 

1. 打开 **SSMA for Sybase**。 
1. 选择“文件”，然后选择“新建项目”。 
1. 提供项目名称和项目的保存位置，然后从下拉列表中选择 SQL Server 作为迁移目标。 选择“确定”。
1. 在“连接到 Sybase”对话框上，为 SAP 连接详细信息输入相应的值。 
1. 右键单击要迁移的 SAP 数据库，然后选择“创建报表”。 这会生成一个 HTML 报表。
1. 查看 HTML 报表，了解转换统计信息以及任何错误或警告。 另外，还可在 Excel 中打开报表，来获取 AP ASE 对象的清单和执行架构转换所需的工作量。 报表的默认位置在 SSMAProjects 内的报表文件夹中。

   例如：`drive:\<username>\Documents\SSMAProjects\MyDB2Migration\report\report_<date>`。 


### <a name="validate-type-mappings"></a>验证类型映射

执行架构转换之前，请验证默认数据类型映射，或者根据要求更改这些映射。 为此，可导航到“工具”菜单并选择“项目设置”；你也可在 SAP ASE 元数据资源管理器中选择表，来更改每个表的类型映射  。


### <a name="convert-schema"></a>转换架构

要转换架构，请执行以下步骤：

1. （可选）若要转换动态或即席查询，请右键单击节点，然后选择“添加语句”。 
1. 在顶行导航栏中选择“连接到 SQL Server”，并提供 SQL Server 详细信息。 可选择连接到现有数据库，也可提供新的名称。若是后者，则会在目标服务器上创建一个数据库。
1. 在 Sybase 元数据资源管理器中右键单击 SAP ASE 架构，然后选择“转换架构” 。 或者，可从顶行导航栏中选择“转换架构”。 
1. 比较并查看架构的结构来确定潜在问题。 

   架构转换后，你可在本地保存此项目供离线架构修正练习使用。 在“文件”菜单中选择“保存项目”。 这样，你就有机会在可将架构发布到 SQL Server 之前，对源和目标架构进行脱机评估并执行相应修正。

若要了解详细信息，请查看[转换架构](../../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md)


## <a name="migrate"></a>Migrate 

在满足必需的先决条件并完成与迁移前阶段相关的任务后，你就可执行架构和数据迁移了。

若要发布架构并迁移数据，请执行以下步骤： 

1. 右键单击 SQL Server 元数据资源管理器中的数据库，然后选择“与数据库同步” 。  此操作会将 SAP ASE 架构发布到 SQL Server 实例。
1. 在 SAP ASE 元数据资源管理器中右键单击 SAP ASE 架构，然后选择“迁移数据” 。  或者，可从顶行导航栏中选择“迁移数据”。  
1. 迁移完成后，查看数据迁移报表： 
1. 使用 SQL Server Management Studio (SSMS) 评审 SQL Server 实例上的数据和架构，从而验证迁移。


## <a name="post-migration"></a>迁移后 

成功完成迁移阶段后，需要执行一系列的迁移后任务，来确保所有内容都尽量顺畅、高效地正常运行。

### <a name="remediate-applications"></a>修正应用程序

将数据迁移到目标环境后，以前使用源的所有应用程序都需要开始使用目标。 在某些情况下，实现这一点需要对应用程序进行更改。

### <a name="perform-tests"></a>执行测试

数据库迁移的测试方法包括执行以下活动：

1. **开发验证测试**。 要测试数据库迁移，需要使用 SQL 查询。 必须创建针对源数据库和目标数据库运行的验证查询。 验证查询应涵盖已定义的范围。

2. **设置测试环境**。 测试环境应包含源数据库和目标数据库的副本。 请确保隔离测试环境。

3. **运行验证测试**。 针对源和目标运行验证测试，然后分析结果。

4. **运行性能测试**。 针对源和目标运行性能测试，然后分析和比较结果。

### <a name="optimize"></a>优化

迁移后阶段对于协调任何数据准确性问题、验证完整性以及解决工作负载的性能问题至关重要。

> [!NOTE]
> 若要更详细地了解这些问题及其具体缓解步骤，请查看[迁移后验证和优化指南](/sql/relational-databases/post-migration-validation-and-optimization-guide)。


## <a name="migration-assets"></a>迁移资产

若在完成此迁移方案时需要更多协助，请查看以下资源，它们就是为支持实际迁移项目操作而编写的。

| **标题/链接**                                                                                                        | **说明**                                                                                                                                                                                                                                                                                                                                                                                                     |
| --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Optimization Guide for Mainframe App/Data recompiled to .NET & SQL Server](https://aka.ms/dmj-wp-mainframe-optimize)（针对已重新编译到 .NET 和 SQL Server 的大型机应用/数据的优化指南） | 本指南就如何尽可能高效地从 .NET 针对 SQL Server 执行点查找提供优化建议。 如果客户想要从大型机数据库迁移到 SQL Server，那么他们可能希望迁移针对大型机进行了优化的现有设计模式（尤其是在使用 Raincode Compiler 等第三方工具时），从而自动地将大型机代码（例如 COBOL/JCL）迁移到 T-SQL 和 C# .NET。 |

> [!NOTE]
> 这些资源是作为数据迁移启动计划（简称为“DM 启动”）的一部分编写的，该计划由 Azure 数据组工程团队提供赞助。 “DM 启动”计划的核心宗旨是解锁和加速复杂的现代化进程，并争取数据平台向 Microsoft Azure 数据平台迁移的机会。 如果你认为贵组织有意参与“DM 启动”计划，请联系帐户团队并让他们提交提名。

## <a name="next-steps"></a>后续步骤

- 如需 Azure 数据库迁移指南及其所含信息的概述，请查看视频：[如何使用数据库迁移指南](https://azure.microsoft.com/resources/videos/how-to-use-the-azure-database-migration-guide/)。

- 如需在执行各种数据库和数据迁移方案及专门任务时可为你提供帮助的 Microsoft 与第三方服务和工具的矩阵，请查看[数据迁移服务和工具](https://docs.microsoft.com/azure/dms/dms-tools-matrix)一文。

- 有关其他迁移指南，请参阅[数据库迁移](https://datamigration.microsoft.com/)。 

有关视频，请查看： 
- [迁移之旅及推荐用于执行评估和迁移的工具和服务的概述](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)。
