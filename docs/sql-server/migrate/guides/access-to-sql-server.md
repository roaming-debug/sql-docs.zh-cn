---
title: 迁移指南：从 Access 迁移到 SQL Server
description: '本指南介绍如何使用适用于 Access 的 SQL Server 迁移助手 (SSMA for Access) 将 Microsoft Access 数据库迁移到 Microsoft SQL Server。 '
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 3e49909b26d845c0edfaf9e5d50528b077fb2593
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/16/2021
ms.locfileid: "103603290"
---
# <a name="migration-guide-access-to-sql-server"></a>迁移指南：从 Access 迁移到 SQL Server
[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

本迁移指南介绍如何使用适用于 Access 的 SQL Server 迁移助手 (SSMA for Access) 将 Microsoft Access 数据库迁移到 SQL Server。 

有关其他迁移指南，请参阅[数据库迁移](https://datamigration.microsoft.com/)。 

## <a name="prerequisites"></a>先决条件 

若要将 Access 数据库迁移到 SQL Server，你需要：

- 验证源环境是否受支持。 
- [适用于 Access 的 SQL Server 迁移助手](https://www.microsoft.com/download/details.aspx?id=54255)。 


## <a name="pre-migration"></a>预迁移 


满足先决条件后，就可以发现环境的拓扑并评估迁移的可行性。


### <a name="assess"></a>评估

通过适用于 Access 的 SQL Server 迁移助手 (SSMA)，你可评审数据库对象和数据，并评估数据库是否适合迁移。 若要详细了解此工具，请查看[适用于 Access 的 SQL Server 迁移助手](/sql/ssma/access/sql-server-migration-assistant-for-access-accesstosql)。

要创建评估，请执行以下步骤：

1. 打开[适用于 Access 的 SQL Server 迁移助手](https://www.microsoft.com/download/details.aspx?id=54255)。 
1. 选择“文件”，然后选择“新建项目”。 为迁移项目提供一个名称。 
1. 选择“添加数据库”，然后选择要添加到项目中的数据库。 
1. 在 Access 元数据资源管理器中，右键单击要评估的数据库，然后选择“创建报表” 。 
1. 查看评估报表。 例如： 

### <a name="convert"></a>转换 

若要转换数据库对象，请执行以下步骤： 

1. 选择“连接到 Azure SQL 数据库”并提供连接详细信息。 
1. 在 Access 元数据资源管理器中右键单击该数据库，然后选择“转换架构” 。  
1. （可选）若要转换单个对象，请右键单击该对象，再选择“转换架构”。 已转换的对象以粗体形式显示在 Access 元数据资源管理器中： 
1. 在“输出”窗格中选择“查看结果”，然后在“错误列表”窗格中查看错误 。 


## <a name="migrate"></a>Migrate

完成对数据库的评估并解决任何分歧后，下一步就是执行迁移过程。 迁移数据是一个大容量加载操作，它在事务中将多行数据移动到 SQL Server。 每个事务中加载到 SQL Server 中的行数是在项目设置中配置的。

若要使用 SSMA for Access 迁移数据，请执行以下步骤： 

1. 如果尚未连接，请选择“连接到 SQL Server”并提供连接详细信息。 
1. 在 Azure SQL 数据库元数据资源管理器中右键单击数据库，然后选择“与数据库同步” 。 此操作会将 MySQL 架构发布到 Azure SQL 数据库。
1. 使用 Access 元数据资源管理器勾选要迁移的项目旁边的框。 如果要迁移整个数据库，请选中该数据库旁边的框。 
1. 右键单击要迁移的数据库或对象，然后选择“迁移数据”。 
   若要迁移整个数据库的数据，请选中数据库名称旁边的复选框。 若要从单个表中迁移数据，请展开数据库，展开表，然后选中表旁边的复选框。 若要忽略单个表中的数据，请清除对应的复选框。


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

> [!Note]
> 若要更详细地了解这些问题及其具体缓解步骤，请查看[迁移后验证和优化指南](https://docs.microsoft.com/sql/relational-databases/post-migration-validation-and-optimization-guide)。

## <a name="migration-assets"></a>迁移资产 

若在完成此迁移方案时需要更多协助，请查看以下资源，它们就是为支持实际迁移项目操作而编写的。

| **标题/链接** | **说明** |
| -------------- | --------------- |
| [数据工作负载评估模型和工具](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | 此工具为给定工作负载提供了建议的“最佳”目标平台、云就绪性和应用程序/数据库修正级别。 它提供简单的一键式计算和报表生成功能，通过提供统一的自动化目标平台决策过程，极大地帮助加速大规模评估。 |

这些资源是作为 Data SQL Ninja 计划的一部分开发的，该计划由 Azure 数据组工程团队提供赞助。 Data SQL Ninja 计划的核心宗旨是解锁和加速复杂的现代化进程，并争取数据平台向 Microsoft Azure 数据平台迁移的机会。 如果你认为贵组织有意参与 Data SQL Ninja 计划，请联系帐户团队并让他们提交提名。

## <a name="next-steps"></a>后续步骤

迁移后，请查看[迁移后验证和优化指南](/sql/relational-databases/post-migration-validation-and-optimization-guide)。 

有关在执行各种数据库和数据迁移方案及专门任务时可为你提供帮助的 Microsoft 与第三方服务和工具的矩阵，请参阅[数据迁移服务和工具](/azure/dms/dms-tools-matrix)。

有关其他迁移指南，请参阅[数据库迁移](https://datamigration.microsoft.com/)。 

有关视频内容，请参阅：
- [迁移历程概述](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)
