---
title: 将 MySQL 数据库转换 (MySQLToSQL) |Microsoft Docs
description: 了解如何在连接和设置项目和数据映射选项后，通过 SSMA 将 MySQL 数据库对象转换为 SQL Server 或 Azure SQL 数据库对象。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: ac21850b-fb32-4704-9985-5759b7c688c7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 61298bb7e2c65e85959b52ab3750dc49a5105504
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068979"
---
# <a name="converting-mysql-databases-mysqltosql"></a>转换 MySQL 数据库 (MySQLToSQL)
连接到 MySQL、连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，并设置项目和数据映射选项后，可以将 MySQL 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 Azure SQL 数据库对象。  
  
## <a name="the-conversion-process"></a>转换过程  
转换数据库对象将获取 MySQL 中的对象定义，然后将其转换为类似 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 的对象，然后将此信息加载到 SSMA 元数据。 它不会将信息加载到的实例中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 然后，可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器查看对象及其属性。  
  
在转换过程中，SSMA 会将输出消息打印到 "输出" 窗格，并将错误消息打印到 "错误列表" 窗格。 使用输出和错误信息来确定是否必须修改 MySQL 数据库或转换过程以获取所需的转换结果。  
  
## <a name="setting-conversion-options"></a>设置转换选项  
转换对象之前，请在 " **项目设置** " 对话框中查看项目转换选项。 使用此对话框，可以设置 SSMA 转换表和索引的方式。 有关详细信息，请参阅 [MySQLToSQL&#41; &#40;的项目设置 &#40;转换&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
  
## <a name="conversion-results"></a>转换结果  
下表显示了转换哪些 MySQL 对象以及生成的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象：  
  
|MySQL 对象|生成的 SQL Server 对象|  
|-|-|  
|具有依赖对象（如索引）的表|SSMA 创建具有依赖对象的表。 表将与所有索引和约束一起转换。 索引将转换为单独 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的对象。<br /><br />**空间数据类型映射** 只能在表节点级执行。<br /><br />有关表转换设置的详细信息，请参阅 [转换设置](conversion-settings-mysqltosql.md)|  
|函数|如果该函数可以直接转换为 Transact-sql，则 SSMA 将创建一个函数。 在某些情况下，该函数必须转换为存储过程。 可以通过在项目设置中使用 **函数转换** 来完成此操作。 在这种情况下，SSMA 创建存储过程和调用存储过程的函数。<br /><br />**提供的选项：**<br /><br />根据项目设置进行转换<br /><br />转换为函数<br /><br />转换为存储过程<br /><br />有关函数转换设置的详细信息，请参阅 [转换设置](conversion-settings-mysqltosql.md)|  
|过程|如果可以将该过程直接转换为 Transact-sql，SSMA 会创建一个存储过程。 在某些情况下，必须在自治事务中调用存储过程。 在这种情况下，SSMA 创建两个存储过程：一个用于实现过程，另一个用于调用实现存储过程。|  
|数据库转换|作为 MySQL 对象的数据库不是由 SSMA for MySQL 直接转换而成。 MySQL 数据库处理起来更像架构名称，并且在转换过程中所有物理参数都将丢失。 SSMA for MySQL 使用 [映射 mysql SQL Server 数据库 &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md) 将对象从 MySQL 数据库映射到适当的 SQL Server 数据库/架构对。|  
|触发器转换|**SSMA 基于以下规则创建触发器：**<br /><br />在触发器转换为 INSTEAD of T-sql 触发器之前<br /><br />触发器在 T-sql 触发器转换为之后，在 T-sql 触发器中，每行都有或不包含迭代。|  
|视图转换|SSMA 创建具有依赖对象的视图|  
|语句转换|-每个 SQL 语句对象可能包含单个 MySQL 语句 (如 DDL、DML 和其他类型的语句) 或 BEGIN .。。结束块。<br />-   多 **语句转换： BEGIN .。。结束块转换** SQL 语句还可以包含 BEGIN .。。END 块，如过程、函数或触发器定义中的一个。 对于单个 MySQL 语句对象，应以相同的方式转换这些块。|  
  
## <a name="converting-mysql-database-objects"></a>转换 MySQL 数据库对象  
若要转换 MySQL 数据库对象，请首先选择要转换的对象，然后让 SSMA 执行转换。 若要在转换过程中查看输出消息，请在 " **视图** " 菜单上选择 " **输出**"。  
  
**将 MySQL 对象转换为 SQL Server 或 SQL Azure 语法**  
  
1.  在 MySQL 元数据资源管理器中，展开 MySQL 服务器，然后展开 " **数据库**"。  
  
2.  选择要转换的对象：  
  
    -   若要转换所有架构，请选中 " **数据库**" 旁边的复选框。  
  
    -   若要转换或省略数据库，请选中数据库名称旁边的复选框。  
  
    -   若要转换或省略对象的类别，请展开一个架构，然后选中或清除该类别旁边的复选框。  
  
    -   若要转换或省略单个对象，请展开 category 文件夹，然后选中或清除该对象旁边的复选框。  
  
3.  若要转换所有选定的对象，请右键单击 " **数据库** "，然后选择 " **转换架构**"。  
  
    您还可以通过右键单击对象或其父文件夹，然后选择 " **转换架构**"，来转换各个对象或对象类别。  
  
## <a name="viewing-conversion-problems"></a>查看转换问题  
某些 MySQL 对象可能不会转换。 您可以通过查看摘要转换报告来确定转换成功率。  
  
**查看摘要报表**  
  
1.  在 MySQL 元数据资源管理器中，选择 " **数据库**"。  
  
2.  在右侧窗格中，选择 " **报表** " 选项卡。  
  
    此报表显示已评估或转换的所有数据库对象的摘要评估报告。 您还可以查看单个对象的摘要报表：  
  
    -   若要查看单个架构的报表，请在 MySQL 元数据资源管理器中选择该数据库。  
  
    -   若要查看单个对象的报表，请在 MySQL 元数据资源管理器中选择该对象。 具有转换问题的对象具有红色错误图标。  
  
对于失败转换的对象，可以查看导致转换失败的语法。  
  
**查看各个转换问题**  
  
1.  在 MySQL 元数据资源管理器中，展开 " **数据库**"。  
  
2.  展开显示红色错误图标的数据库。  
  
3.  在数据库下，展开一个包含红色错误图标的文件夹。  
  
4.  选择包含红色错误图标的对象。  
  
5.  在右侧窗格中，单击 " **报表** " 选项卡。  
  
6.  在 " **报表** " 选项卡的顶部是一个下拉列表。 如果列表显示 " **统计信息**"，请将所选内容更改为 " **源**"。  
  
    SSMA 将显示源代码，并将多个按钮直接显示在代码上方。  
  
7.  单击 " **下一问题** " 按钮。 这是一个红色的错误图标，其中有一个指向右侧的箭头。  
  
    SSMA 将突出显示在当前对象中找到的第一个有问题的源代码。  
  
对于无法转换的每个项，必须确定要对该对象执行哪些操作：  
  
-   可以修改 MySQL 数据库中的对象，以删除或修改有问题的代码。 若要将更新的代码加载到 SSMA 中，必须更新元数据。 有关详细信息，请参阅 [连接到 MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
-   可以从迁移中排除对象。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器和 MySQL 元数据资源管理器中，先清除项旁边的复选框，然后将对象加载到中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 并从 MySQL 迁移数据。  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是将 [转换的数据库对象加载到 SQL Server &#40;MySQLToSQL 中&#41;](../../ssma/mysql/loading-converted-database-objects-into-sql-server-mysqltosql.md)  
  
## <a name="see-also"></a>另请参阅  
[将 MySQL 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
