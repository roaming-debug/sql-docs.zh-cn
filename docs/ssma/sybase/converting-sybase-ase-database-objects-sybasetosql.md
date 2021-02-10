---
description: 转换 Sybase ASE 数据库对象 (SybaseToSQL)
title: 将 Sybase ASE 数据库对象转换 (SybaseToSQL) |Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Converting Database Objects
ms.assetid: 509cb65d-2f54-427a-83d7-37919cc4e3e3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 07560faaa3fb3316864216859f79cd5e1b7ff950
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056832"
---
# <a name="converting-sap-ase-database-objects-sybasetosql"></a>将 SAP ASE 数据库对象转换 (SybaseToSQL) 
连接到 SAP 自适应服务器企业 (ASE) 、连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE sql 并设置项目和数据映射选项后，可以将 SAP 自适应服务器企业 (ASE) 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure SQL 数据库对象。  
  
## <a name="the-conversion-process"></a>转换过程  
转换数据库对象将从 ASE 获取对象定义，将它们转换为类似 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 的对象，然后将此信息加载到 SSMA 元数据。 它不会将信息加载到实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 中。 然后，可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 元数据资源管理器查看对象及其属性。
  
在转换过程中，SSMA 会将输出消息打印到 "输出" 窗格，并将错误消息打印到 " **错误列表** " 窗格。 使用输出和错误信息来确定是否必须修改 ASE 数据库或转换过程以获取所需的转换结果。  
  
## <a name="setting-conversion-options"></a>设置转换选项  
转换对象之前，请在 " **项目设置** " 对话框中查看项目转换选项。 使用此对话框，可以设置 SSMA 如何转换函数和全局变量。 有关详细信息，请参阅 [SybaseToSQL&#41;&#40;转换&#41; &#40;项目设置 ](../../ssma/sybase/project-settings-conversion-sybasetosql.md)。  
  
## <a name="converting-ase-database-objects"></a>转换 ASE 数据库对象  
若要转换 ASE 数据库对象，请首先选择要转换的对象，然后让 SSMA 执行转换。 若要在转换过程中查看输出消息，请在 " **视图** " 菜单上选择 " **输出**"。  
  
**将 ASE 对象转换为 SQL Server 或 SQL Azure 语法**  
  
1.  在 "Sybase 元数据资源管理器" 中，展开 ASE 服务器，然后展开 " **数据库**"。  
  
2.  选择要转换的对象：  
  
    -   若要转换所有数据库，请选中 " **数据库**" 旁边的复选框。  
  
    -   若要转换或省略数据库，请选中或清除数据库名称旁边的复选框。  
  
    -   若要转换或省略单个架构，请展开数据库，展开 " **架构**"，然后选中或取消选中架构旁边的复选框。  
  
    -   若要转换或省略对象的类别，请展开该架构，然后选中或清除该类别旁边的复选框。  
  
    -   若要转换或省略单个对象，请展开 category 文件夹，然后选中或清除该对象旁边的复选框。  
  
3.  若要转换所有选定的对象，请右键单击 " **数据库**"，然后选择 " **转换架构**"。  
  
    您还可以通过右键单击对象或其包含文件夹，然后选择 " **转换架构**"，来转换各个对象或对象类别。  
  
> [!NOTE]  
> 某些 SAP ASE 系统功能与行为中的等效 SQL Server 系统函数并不完全匹配。 为了模拟 SAP ASE 行为，SSMA 会在名为 "2ss" 的架构下的已转换 SQL Server 数据库中生成用户定义的函数。 根据项目设置，某些 SQL Server 系统函数将替换为这些模拟函数。 SSMA 创建以下用户定义函数：  

:::row:::
    :::column:::
        **char_length_nvarchar**  
        **char_length_varchar**  
        **charindex_nvarchar**  
        **charindex_varchar**  
        **hextoint**  
        **index_colorder**  
    :::column-end:::
    :::column:::
        **inttohex**  
        **ssma_current_time**  
        **ssma_datediff**  
        **ssma_datepart**  
        **substring_nvarchar**  
        **substring_varbinary**  
    :::column-end:::
    :::column:::
        **substring_varchar**  
        **to_unichar**  
        **uhighsurr**  
        **ulowsurr**  
    :::column-end:::
:::row-end:::

## <a name="objects-not-supported-in-azure-sql"></a>Azure SQL 中不支持的对象  
SSMA for SAP ASE 在转换为本地 SQL Server 时使用以下 T-sql 关键字，但 SQL Azure T-sql 语法不支持这些关键字：  

:::row:::
    :::column:::
        CHECKPOINT  
        CREATE/ALTER/DROP DEFAULT  
        CREATE/DROP RULE  
        DBCC TRACEOFF  
        DBCC TRACEON  
    :::column-end:::
    :::column:::
        GRANT/REVOKE/DENY ALL  
        KILL  
        READTEXT  
        SELECT INTO  
        SET OFFSETS  
    :::column-end:::
    :::column:::
        SETUSER  
        SHUTDOWN  
        WRITETEXT  
    :::column-end:::
:::row-end:::

## <a name="viewing-conversion-problems"></a>查看转换问题  
某些 SAP ASE 对象可能不会转换。 您可以通过查看摘要转换报告来确定转换成功率。  
  
**查看摘要报表**  
  
1.  在 Sybase 元数据资源管理器中，选择 " **数据库**"。  
  
2.  在右侧窗格中，选择 " **报表** " 选项卡。  
  
    此报表显示已评估或转换的所有数据库对象的摘要评估报告。 您还可以查看单个对象的摘要报表：  
  
    -   若要查看单个数据库的报表，请在 Sybase 元数据资源管理器中选择该数据库。  
  
    -   若要查看单个数据库对象的报表，请在 Sybase 元数据资源管理器中选择该对象。 具有转换问题的对象具有红色错误图标。  
  
对于失败转换的对象，可以查看导致转换失败的语法。  
  
**查看各个转换问题**  
  
1.  在 Sybase 元数据资源管理器中，展开 " **数据库**"。  
  
2.  展开显示红色错误图标的数据库。  
  
3.  展开 " **架构** " 文件夹，然后展开显示红色错误图标的架构。  
  
4.  在该架构下，展开一个包含红色错误图标的文件夹。  
  
5.  选择包含红色错误图标的对象。  
  
6.  在右侧窗格中，选择 " **报表** " 选项卡。  
  
7.  在 " **报表** " 选项卡的顶部是一个下拉列表。 如果列表显示 " **统计信息**"，请将所选内容更改为 " **源**"。  
  
    SSMA 将显示源代码，并将多个按钮直接显示在代码上方。  
  
8.  选择 **下一个问题**，其中有一个指向右侧箭头的红色错误图标。  
  
    SSMA for SAP ASE 将突出显示在当前对象中找到的第一个有问题的源代码。  
  
对于无法转换的每个项，必须确定要对该对象执行哪些操作：  
  
-   您可以在 " **SQL** " 选项卡上编辑过程和触发器的源代码。  
  
-   可以更改 SAP ASE 对象，以删除或修改有问题的代码。 若要将更新的代码加载到 SSMA 中，必须更新元数据。 有关详细信息，请参阅 [连接到 SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md)。  
  
-   可以从迁移中排除对象。 在 " [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] AZURE Sql 元数据资源管理器" 和 "Sybase 元数据资源管理器" 中，清除项旁边的复选框，然后将对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure SQL，并从 SAP ASE 迁移数据。  
  
## <a name="next-steps"></a>后续步骤  
迁移过程的下一步是将已 [转换的数据库对象加载到 SQL Server/SQL Azure (SybaseToSQL) ](./loading-converted-database-objects-into-sql-server-sybasetosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[将 SAP ASE 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
