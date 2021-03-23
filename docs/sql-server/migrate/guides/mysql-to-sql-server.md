---
title: 将 MySQL 迁移到 SQL Server
description: '本指南介绍如何使用适用于 MySQL 的 SQL Server 迁移助手 (SSMA for MySQL) 将 MySQL 数据库迁移到 Microsoft SQL Server。 '
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: f944cca6674abf9488729c945dbd4a53aee9ef27
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2021
ms.locfileid: "103603288"
---
# <a name="migration-guide-mysql-to-sql-server"></a>迁移指南：从 MySQL 迁移到 SQL Server

本指南可帮助你将 MySQL 数据库迁移到 SQL Server。 

有关其他迁移指南，请参阅[数据库迁移](https://datamigration.microsoft.com/)。 

## <a name="prerequisites"></a>先决条件 

若要将 MySQL 数据库迁移到 SQL Server，你需要：

- 连接到源 MySQL 数据库来运行评估和转换。
- [适用于 MySQL 的 SQL Server 迁移助手](https://aka.ms/ssmaformysql)。

## <a name="pre-migration"></a>迁移前 

满足先决条件后，就可发现 MySQL 环境和评估迁移的可行性了。
在使用 SSMA 开始迁移之前，你必须：

1.  创建新项目。  
2.  连接到 MySQL 数据库。
3.  成功连接后，MySQL 架构将出现在 MySQL 元数据资源管理器中。 在 MySQL 元数据资源管理器中右键单击对象，来执行创建报表以评估到 SQL Server 的转换等任务。

### <a name="assess"></a>评估 


若要使用 [SSMA for MySQL](https://aka.ms/ssmaformysql) 创建评估，请执行以下步骤： 

1. 打开适用于 MySQL 的 SQL Server 迁移助手。 
1. 选择“文件”，然后选择“新建项目” 。 提供项目名称、项目的保存位置和迁移目标。
1. 在“迁移到”选项中选择“SQL Server” 。 
1. 在“连接到 MySQL”对话框中提供连接详细信息，然后连接到 MySQL 服务器。 
1. 在 MySQL 元数据资源管理器中右键单击 MySQL 架构，然后选择“创建报表” 。 或者，可从顶行导航栏中选择“创建报表”。 
1. 查看包含转换统计信息、错误和警告的 HTML 报表。 分析此报表，了解转换问题及其解决方案。 

   还可从 SSMA 项目文件夹访问此报表，如第一个屏幕中所选。 在上例中，从以下位置找到 report.xml 文件：

   `drive:\Users\<username>\Documents\SSMAProjects\MySQLMigration\report\report_2016_11_12T02_47_55\`
 
   在 Excel 中打开它，来获取 MySQL 对象的清单和执行架构转换所需的工作量。

### <a name="validate-type-mappings"></a>验证类型映射

执行架构转换之前，请验证默认数据类型映射，或者根据要求更改这些映射。 为此，可导航到“工具”菜单并选择“项目设置”；你也可在 MySQL 元数据资源管理器中选择表，来更改每个表的类型映射  。

若要详细了解 SSMA 中的转换设置，请查看[项目设置](../../../ssma/mysql/project-settings-conversion-mysqltosql.md)

### <a name="convert-schema"></a>转换架构

转换数据库对象时，会从 MySQL 中获取对象定义，将这些定义转换为类似的 SQL Server 对象，然后将此信息加载到 SSMA 元数据。 它不会将信息加载到 SQL Server 的实例中。 然后，可使用 SQL Server 元数据资源管理器查看对象及其属性。

在转换期间，SSMA 会将输出消息打印到“输出”窗格，并将错误消息打印到“错误列表”窗格。 使用输出和错误信息来确定是否必须修改 MySQL 数据库或转换过程以获取所需的转换结果。

要转换架构，请执行以下步骤：

1. （可选）若要转换动态或即席查询，请右键单击节点，然后选择“添加语句”。 
1. 在顶行导航栏中选择“连接到 SQL Server”，并提供 SQL Server 详细信息。 可选择连接到现有数据库，也可提供新的名称。若是后者，则会在目标服务器上创建一个数据库。
1. 在 MySQL 元数据资源管理器中右键单击 MySQL 架构，然后选择“转换架构” 。 或者，可从顶行导航栏中选择“转换架构”。 
1. 比较并查看架构的结构来确定潜在问题。 

   架构转换后，你可在本地保存此项目供离线架构修正练习使用。 在“文件”菜单中选择“保存项目”。 这样，你就有机会在可将架构发布到 SQL Server 之前，对源和目标架构进行脱机评估并执行相应修正。

若要了解详细信息，请查看[转换 MySQL 数据库](../../../ssma/mysql/converting-mysql-databases-mysqltosql.md)

## <a name="migration"></a>迁移 

在满足必需的先决条件并完成与迁移前阶段相关的任务后，你就可执行架构和数据迁移了。

若要迁移数据，会发生两种情况：

- **客户端数据迁移：**
     -   若要执行客户端数据迁移，请在“项目设置”对话框中，选择“客户端数据迁移引擎”选项  。

    > [!NOTE]
    > 当 SQL Express Edition 用作目标数据库时，只允许客户端数据迁移，不支持服务器端数据迁移。

- **服务器端数据迁移：**
    -   在服务器端执行数据迁移之前，请确保：
        - SQL Server 的实例上已安装适用于 MySQL 扩展包的 SSMA。
        - SQL Server 代理服务当前未在 SQL Server 的实例运行。 
    -   若要执行服务器端数据迁移，请在“项目设置”对话框中，选择“服务器端数据迁移引擎”选项  。

> [!IMPORTANT]  
> 如果正在使用的引擎是服务器端数据迁移引擎，那么在迁移数据之前，必须在运行 SSMA 的计算机上安装适用于 MySQL 扩展包的 SSMA 和 MySQL 提供程序。 SQL Server 代理服务也必须正在运行中。 若要详细了解如何安装扩展包，请查看[在 SQL Server 上安装 SSMA 组件（MySQL 到 SQL）](../../../ssma/mysql/installing-ssma-components-on-sql-server-mysqltosql.md)  

若要发布架构并迁移数据，请执行以下步骤： 

1. 右键单击 SQL Server 元数据资源管理器中的数据库，然后选择“与数据库同步” 。  此操作会将 MySQL 架构发布到 SQL Server 实例。
1. 在 MySQL 元数据资源管理器中右键单击 MySQL 架构，然后选择“迁移数据” 。  或者，可从顶行导航栏中选择“迁移数据”。  
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
> 若要更详细地了解这些问题及其具体缓解步骤，请查看[迁移后验证和优化指南](https://docs.microsoft.com/sql/relational-databases/post-migration-validation-and-optimization-guide)。

## <a name="migration-assets"></a>迁移资产 

若在完成此迁移方案时需要更多协助，请查看以下资源，它们就是为支持实际迁移项目操作而编写的。

| 标题/链接                    | 说明            |
| ----------------------------- | ---------------------- |
| [数据工作负载评估模型和工具](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | 此工具为给定工作负载提供了建议的“最佳”目标平台、云就绪性和应用程序/数据库修正级别。 它提供简单的一键式计算和报表生成功能，通过提供统一的自动化目标平台决策过程，极大地帮助加速大规模评估。                |

这些资源是作为 Data SQL Ninja 计划的一部分开发的，该计划由 Azure 数据组工程团队提供赞助。 Data SQL Ninja 计划的核心宗旨是解锁和加速复杂的现代化进程，并争取数据平台向 Microsoft Azure 数据平台迁移的机会。 如果你认为贵组织有意参与 Data SQL Ninja 计划，请联系帐户团队并让他们提交提名。

## <a name="next-steps"></a>后续步骤

- 若要详细了解如何将 MySQL 数据库迁移到 SQL Server，请查看[适用于 MySQL 的 SSMA 文档](../../../ssma/mysql/sql-server-migration-assistant-for-mysql-mysqltosql.md)

- 如需在执行各种数据库和数据迁移方案及专门任务时可为你提供帮助的 Microsoft 与第三方服务和工具的矩阵，请查看[数据迁移服务和工具](https://docs.microsoft.com/azure/dms/dms-tools-matrix)一文。

- 有关其他迁移指南，请参阅[数据库迁移](https://datamigration.microsoft.com/)。 

