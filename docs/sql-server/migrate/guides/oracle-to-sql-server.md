---
title: 从 Oracle 迁移到 SQL Server：迁移指南
description: '本指南介绍如何使用适用于 Oracle 的 SQL Server 迁移助手 (SSMA for Oracle) 将 Oracle 架构迁移到 Microsoft SQL Server。 '
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 675086012398d03e3ed93fbe179de62e0a955cb6
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673468"
---
# <a name="migration-guide-oracle-to-sql-server"></a>迁移指南：从 Oracle 迁移到 SQL Server
[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

本指南介绍如何使用适用于 Oracle 的 SQL Server 迁移助手将 Oracle 数据库迁移到 SQL Server。 

有关其他方案，请参阅[数据库迁移指南](https://datamigration.microsoft.com/)。

## <a name="prerequisites"></a>先决条件 

若要将 Oracle 数据库迁移到 SQL Server，你需要：

- 验证源环境是否受支持。
- 安装 [SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019?filetype=EXE)。
- 下载[适用于 Oracle 的 SQL Server 迁移助手 (SSMA)](https://www.microsoft.com/download/details.aspx?id=54258)。
- [SSMA for Oracle 的必需权限](/sql/ssma/oracle/connecting-to-oracle-database-oracletosql)和[提供程序](/sql/ssma/oracle/connect-to-oracle-oracletosql)。


## <a name="pre-migration"></a>迁移前

准备好迁移到云时，请验证你的源环境是否受到支持且你是否已满足各项先决条件。 这将帮助确保高效成功地完成迁移。

迁移过程的这一部分涉及到对需要迁移的数据库进行清点、评估这些数据库是否存在潜在的迁移问题或阻碍因素，然后处理你可能已发现的任何项目。 

> [!Important]
> 适用于 Oracle 的 SQL Server 迁移助手并不支持迁移所有 Oracle 功能。 如需相关解决方案，请查看[针对所选 Oracle 功能的迁移方法](https://blogs.msdn.microsoft.com/datamigration/2017/05/10/migration-approach-for-oracle-features/)


### <a name="discover"></a>发现

使用 [MAP 工具包](https://go.microsoft.com/fwlink/?LinkID=316883)来确定现有数据源以及你的业务正在使用的功能的详细信息，从而更好地了解和规划迁移。 此过程涉及到扫描网络，以结合正在使用的版本和功能确定组织的所有 Oracle 实例。

若要使用 MAP 工具包执行清单扫描，请执行以下步骤： 

1. 打开 [MAP 工具包](https://go.microsoft.com/fwlink/?LinkID=316883)。
1. 选择“创建/选择数据库”。

   ![选择数据库](./media/oracle-to-sql-server/select-database.png)

1. 选择“创建清单数据库”，输入要创建的新的清单数据库的名称，提供简短说明，然后选择“确定” 。 

   :::image type="content" source="media/oracle-to-sql-server/create-inventory-database.png" alt-text="创建库存数据库":::

1. 选择“收集清单数据”，打开清单和评估向导 。 

   :::image type="content" source="media/oracle-to-sql-server/collect-inventory-data.png" alt-text="收集库存数据":::

1. 在“清单和评估向导”中，选择“Oracle”，然后选择“下一步”  。 

   ![选择 Oracle](./media/oracle-to-sql-server/choose-oracle.png)

1. 选择最适合你的业务需求和环境的计算机搜索选项，然后选择“下一步”： 

   ![选择最适合业务需求的计算机搜索选项](./media/oracle-to-sql-server/choose-search-option.png)

1. 为要浏览的系统输入凭据或创建新凭据，然后选择“下一步”。

    ![输入凭据](./media/oracle-to-sql-server/choose-credentials.png)

1. 设置凭据的顺序，然后选择“下一步”。 

   ![设置凭据顺序](./media/oracle-to-sql-server/set-credential-order.png)  

1. 为要发现的每台计算机指定凭据。 可对每台计算机/机器使用唯一凭据，也可选择使用“所有计算机凭据”列表。  

   ![为要发现的每台计算机指定凭据](./media/oracle-to-sql-server/specify-credentials-for-each-computer.png)

1. 验证你的选择内容摘要，然后选择“完成”。

   ![查看摘要](./media/oracle-to-sql-server/review-summary.png)

1. 扫描完成后，查看“数据收集”摘要报表。 扫描需要几分钟时间，具体取决于数据库的数量。 完成后，选择“关闭”。 

   ![收集摘要报表](./media/oracle-to-sql-server/collection-summary-report.png)

1. 选择“选项”，生成有关 Oracle 评估和数据库详细信息的报表。 （逐一）选择两个选项来生成报表。




### <a name="assess"></a>评估

确定数据源后，使用[适用于 Oracle 的 SQL Server 迁移助手 (SSMA)](https://www.microsoft.com/download/details.aspx?id=54258)来评估 Oracle 实例到 SQL Server VM 的迁移，来了解这两者之间的区别。 借助迁移助手，你可查看数据库对象和数据、评估数据库是否适合迁移、将数据库对象迁移到 SQL Server，然后将数据迁移到 SQL Server。

要创建评估，请执行以下步骤： 

1. 打开[适用于 Oracle 的 SQL Server 迁移助手 (SSMA)](https://www.microsoft.com/download/details.aspx?id=54258)。 
1. 选择“文件”，然后选择“新建项目”。 
1. 提供项目名称和项目的保存位置，然后从下拉列表中选择 SQL Server 迁移目标。 选择“确定”。 

   ![新建项目](./media/oracle-to-sql-server/new-project.png)

1. 在“连接到 Oracle”对话框上，为 Oracle 连接详细信息输入相应的值。

   ![连接到 Oracle](./media/oracle-to-sql-server/connect-to-oracle.png)

   选择要迁移的 Oracle 架构：

   ![选择要加载的架构](./media/oracle-to-sql-server/select-schema.png)

1. 在 Oracle 元数据资源管理器 中，选择 Oracle 架构，然后选择“创建报表”，生成包含转换统计信息和错误/警告（若有）的 HTML 报表 。

   ![创建报表](./media/oracle-to-sql-server/create-report.png)

1. 在 HTML 报表中查看转换统计信息以及错误和警告。 分析报表，了解转换问题和解决方法。

   还可从 SSMA 项目文件夹访问此报表，如第一个屏幕中所选。 在上例中，从以下位置找到 report.xml 文件： 

   `drive:\<username>\Documents\SSMAProjects\MyOracleMigration\report\report_2016_11_12T02_47_55\`

    然后在 Excel 中打开它，来获取 Oracle 对象的清单和执行架构转换所需的工作量。

   ![转换报表](./media/oracle-to-sql-server/conversion-report.png)


### <a name="validate-data-types"></a>验证数据类型

验证默认的数据类型映射，并根据需要对其进行更改（如有必要）。 为此，请执行下列步骤： 

1. 在菜单中，选择“工具”。 
1. 选择“项目设置”。 
1. 选择“类型映射”选项卡。 

   ![类型映射](./media/oracle-to-sql-server/type-mappings.png)

1. 可在 Oracle 元数据资源管理器中选择表，来更改每个表的类型映射。 



### <a name="convert-schema"></a>转换架构

要转换架构，请执行以下步骤： 

1. （可选）若要转换动态或即席查询，请右键单击节点，然后选择“添加语句”。
1. 从顶行导航栏中选择“连接到 SQL Server”，并提供 SQL Server 的连接详细信息。 可选择连接到现有数据库，也可提供新的名称。若是后者，则会在目标服务器上创建一个数据库。

   ![连接到 SQL](./media/oracle-to-sql-server/connect-to-sql.png)

1. 右键单击架构，然后选择“转换架构”。

   ![转换架构](./media/oracle-to-sql-server/convert-schema.png)

1. 完成架构转换后，比较并查看架构的结构来确定潜在问题。

   将转换后的对象与原始对象进行比较： 

   ![转换架构，比较和查看对象代码](./media/oracle-to-sql-server/table-mapping.png)

   将转换后的过程与原始过程进行比较： 

   ![查看转换后的过程](./media/oracle-to-sql-server/procedure-comparison.png)

   可在本地保存此项目供离线架构修正练习使用。 为此，可从“文件”菜单中选择“保存项目” 。 这样，你就有机会在可将架构发布到 SQL Server 之前，对源和目标架构进行脱机评估并执行相应修正。


## <a name="migrate"></a>Migrate

在满足必需的先决条件并完成与迁移前阶段相关的任务后，你就可执行架构和数据迁移了。 迁移涉及到两步操作 - 发布架构和迁移数据。 


若要发布架构并迁移数据，请执行以下步骤： 

1. 从 SQL Server 元数据资源管理器中右键单击数据库，然后选择“与数据库同步” 。 此操作会将 Oracle 架构发布到 SQL Server。 

   ![与数据库同步](./media/oracle-to-sql-server/synchronize-database.png)

   查看与数据库的同步：

   ![与数据库同步 - 查看映射](./media/oracle-to-sql-server/synchronize-database-review.png)

1. 从 Oracle 元数据资源管理器中右键单击 Oracle 架构，然后选择“迁移数据” 。 或者，可从顶行导航中选择“迁移数据”。

   ![迁移数据](./media/oracle-to-sql-server/migrate-data.png)

1. 在对话框中提供 Oracle 和 SQL Server 的连接详细信息。
1. 迁移完成后，查看数据迁移报表：

    ![数据迁移报表](./media/oracle-to-sql-server/data-migration-report.png)

1. 使用 [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) 连接到 SQL Server，来查看 SQL Server 实例上的数据和架构。 

   ![在 SSMA 中验证](./media/oracle-to-sql-server/validate-in-ssms.png)


除了使用 SSMA，还可使用 SQL Server Integration Services (SSIS) 来迁移数据。 若要了解更多信息，请参阅以下文章： 
- 博客 [SQL Server 迁移助手：如何评估数据并将其从非 Microsoft 数据平台迁移到 SQL Server](https://blogs.msdn.microsoft.com/datamigration/2016/11/16/sql-server-migration-assistant-how-to-assess-and-migrate-databases-from-non-microsoft-data-platforms-to-sql-server/)。
- 文章 [SQL Server Integration Services 入门](https://docs.microsoft.com//sql/integration-services/sql-server-integration-services)。
- 白皮书 [SQL Server Integration Services：SSIS for Azure 和混合数据移动](https://download.microsoft.com/download/D/2/0/D20E1C5F-72EA-4505-9F26-FEF9550EFD44/SSIS%20Hybrid%20and%20Azure.docx)。



## <a name="post-migration"></a>迁移后 

成功完成迁移阶段后，需要执行一系列的迁移后任务，来确保所有内容都尽量顺畅、高效地正常运行。

### <a name="remediate-applications"></a>修正应用程序

将数据迁移到目标环境后，以前使用源的所有应用程序都需要开始使用目标。 在某些情况下，实现这一点需要对应用程序进行更改。

[Data Access Migration Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) 是 Visual Studio Code 的一个扩展，你可用它来分析 Java 源代码并检测数据访问 API 调用和查询，从而能够在一个界面查看为支持新的数据库后端而需满足的要求。 若要了解详细信息，请查看[从 Oracle 迁移 Java 应用程序](https://techcommunity.microsoft.com/t5/microsoft-data-migration/migrate-your-java-applications-from-oracle-to-sql-server-with/ba-p/368727)博客。 

### <a name="perform-tests"></a>执行测试

数据库迁移的测试方法包括执行以下活动：

1. **开发验证测试**。 要测试数据库迁移，需要使用 SQL 查询。 必须创建针对源数据库和目标数据库运行的验证查询。 验证查询应涵盖已定义的范围。

2. **设置测试环境**。 测试环境应包含源数据库和目标数据库的副本。 请确保隔离测试环境。

3. **运行验证测试**。 针对源和目标运行验证测试，然后分析结果。

4. **运行性能测试**。 针对源和目标运行性能测试，然后分析和比较结果。


### <a name="optimize"></a>优化

迁移后阶段对于协调任何数据准确性问题、验证完整性以及解决工作负载的性能问题至关重要。

> [!Note]
> 若要更详细地了解这些问题及其具体缓解步骤，请查看[迁移后验证和优化指南](https://docs.microsoft.com//sql/relational-databases/post-migration-validation-and-optimization-guide)。


## <a name="migration-assets"></a>迁移资产 

若在完成此迁移方案时需要更多协助，请查看以下资源，它们就是为支持实际迁移项目操作而编写的。


| **标题/链接**                                                                                                                                          | **说明**                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [数据工作负载评估模型和工具](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | 此工具为给定工作负载提供了建议的“最佳”目标平台、云就绪性和应用程序/数据库修正级别。 它提供简单的一键式计算和报表生成功能，通过提供统一的自动化目标平台决策过程，极大地帮助加速大规模评估。                                                          |
| [Oracle 清单脚本项目](https://github.com/Microsoft/DataMigrationTeam/tree/master/Oracle%20Inventory%20Script%20Artifacts)                 | 该资产包含一个 PL/SQL 查询，它会命中 Oracle 系统表，并按架构类型、对象类型和状态提供对象计数。 它还会提供每个架构中“原始数据”和表大小的粗略估计，结果以 CSV 格式存储。                                                                                                               |
| [自动进行 SSMA Oracle 评估收集和整合](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Automate%20SSMA%20Oracle%20Assessment%20Collection%20%26%20Consolidation)                                             | 这组资源使用 .csv 文件作为输入（项目文件夹中的 sources.csv），以生成在控制台模式中运行 SSMA 评估所需的 xml 文件。 source.csv 是客户根据现有 Oracle 实例的清单提供的。 输出文件为 AssessmentReportGeneration_source_1.xml、ServersConnectionFile.xml 和 VariableValueFile.xml。|
| [SSMA for Oracle 常见错误及其解决方法](https://aka.ms/dmj-wp-ssma-oracle-errors)                                                           | 通过 Oracle，你可在 WHERE 子句中分配非标量条件。 但是，SQL Server 不支持这种类型的条件。 因此，适用于 Oracle 的 SQL Server 迁移助手 (SSMA) 不会在 WHERE 子句中使用非标量条件来转换查询，而是生成错误 O2SS0001。 可在该白皮书中更详细地了解问题及其解决方法。          |
| [Oracle 到 SQL Server 迁移手册](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Oracle%20to%20SQL%20Server%20Migration%20Handbook.pdf)                | 本文档重点介绍在将 Oracle 架构迁移到最新版本的 SQL Server 基础映像时所涉及的任务。 如果迁移要求更改功能/特性，则必须仔细考量对使用数据库的应用程序所作的每项更改可能造成的影响。                                                     |

这些资源是作为 Data SQL Ninja 计划的一部分开发的，该计划由 Azure 数据组工程团队提供赞助。 Data SQL Ninja 计划的核心宗旨是解锁和加速复杂的现代化进程，并争取数据平台向 Microsoft Azure 数据平台迁移的机会。 如果你认为贵组织有意参与 Data SQL Ninja 计划，请联系帐户团队并让他们提交提名。

## <a name="next-steps"></a>后续步骤

迁移后，请查看[迁移后验证和优化指南](../../../relational-databases/post-migration-validation-and-optimization-guide.md)。 

有关在执行各种数据库和数据迁移方案及专门任务时可为你提供帮助的 Microsoft 与第三方服务和工具的矩阵，请参阅[数据迁移服务和工具](/azure/dms/dms-tools-matrix)。

有关其他迁移指南，请参阅[数据库迁移](https://datamigration.microsoft.com/)。 

有关视频内容，请参阅：
- [迁移历程概述](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)
