---
title: 将转换后的数据库对象加载到 SQL Server (OracleToSQL) |Microsoft Docs
description: 了解如何使用 SSMA for Oracle 将从 Oracle 转换的数据库对象加载到 SQL Server 实例中。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Synchronization, Securing Objects in SQL Server
- Synchronization,Scripting Objects
ms.assetid: a8ae33b2-1883-4785-922b-ea0e31c0b37a
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 4b315b0b26f771a2e215f03e1abcfeb5f829e86b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080798"
---
# <a name="loading-converted-database-objects-into-sql-server-oracletosql"></a>将转换数据库对象加载到 SQL Server (OracleToSQL)
将 Oracle 架构转换为 SQL Server 后，可以将生成的数据库对象加载到 SQL Server 中。 您可以使用 SSMA 创建这些对象，也可以编写对象脚本并自己运行脚本。 此外，SSMA 使你可以通过 SQL Server 数据库的实际内容来更新目标元数据。  
  
## <a name="choosing-between-synchronization-and-scripts"></a>在同步和脚本之间进行选择  
如果要在不进行修改的情况下将转换后的数据库对象加载到 SQL Server，则可以让 SSMA 直接创建或重新创建数据库对象。 这种方法既简单又简单，但不允许自定义 [!INCLUDE[tsql](../../includes/tsql-md.md)] 除存储过程之外的 SQL Server 对象的代码自定义。  
  
如果要修改 [!INCLUDE[tsql](../../includes/tsql-md.md)] 用于创建对象的，或者如果想要对对象创建进行更多的控制，请使用 SSMA 创建脚本。 然后，你可以修改这些脚本，单独创建每个对象，甚至使用 SQL Server 代理来计划创建这些对象。  
  
## <a name="using-ssma-to-synchronize-objects-with-sql-server"></a>使用 SSMA 将对象与 SQL Server 同步  
若要使用 SSMA 创建 SQL Server 数据库对象，请在 SQL Server 元数据资源管理器中选择对象，然后将对象与 SQL Server 同步，如以下过程中所示。 默认情况下，如果对象已存在于 SQL Server 中，并且 SSMA 元数据比 SQL Server 中的对象新，则 SSMA 将更改 SQL Server 中的对象定义。 您可以通过编辑 **项目设置** 来更改默认行为。  
  
> [!NOTE]  
> 您可以选择不是从 Oracle 数据库转换的现有 SQL Server 数据库对象。 但是，SSMA 不会重新创建或更改这些对象。  
  
**将对象与 SQL Server 同步**  
  
1.  在 SQL Server 元数据资源管理器 "中，展开顶部 SQL Server" 节点，然后展开 " **数据库**"。  
  
2.  选择要处理的对象：  
  
    -   若要同步完整数据库，请选中数据库名称旁边的复选框。  
  
    -   若要同步或忽略对象的单个对象或类别，请选中或清除对象或文件夹旁边的复选框。  
  
3.  在 SQL Server 元数据资源管理器中选择要处理的对象之后，右键单击 " **数据库**"，然后单击 " **与数据库同步**"。  
  
    您还可以通过右键单击对象或其父文件夹，然后单击 "  **与数据库同步**"，来同步各个对象或对象类别。  
  
    然后，SSMA 将显示与 **数据库同步** 对话框，您可以在其中看到两组项。 在左侧，SSMA 显示了树中表示的所选数据库对象。 在右侧，可以看到表示 SSMA 元数据中的相同对象的树。 可以通过单击右侧或左侧的 "+" 按钮展开树。 同步方向显示在两个树之间放置的 "操作" 列中。  
  
    操作签名可以有三种状态：  
  
    -   向左箭头表示元数据的内容将保存在数据库 (默认的) 中。  
  
    -   向右箭头表示数据库内容将覆盖 SSMA 元数据。  
  
    -   交叉符号表示不会执行任何操作。  
  
单击操作符号以更改状态。 当单击 "**与数据库同步**" 对话框中的 **"确定"** 按钮时，将执行实际同步。  
  
## <a name="scripting-objects"></a>脚本对象  
若要保存 [!INCLUDE[tsql](../../includes/tsql-md.md)] 转换后的数据库对象的定义，或自行更改对象定义并运行脚本，可以将转换后的数据库对象定义保存到 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本中。  
  
**将对象保存为脚本**  
  
1.  选择要保存到脚本中的对象后，右键单击 " **数据库**"，然后单击 " **另存为脚本**"。  
  
    您还可以通过右键单击对象或其父文件夹，然后单击 " **另存为脚本**" 来编写单个对象或对象类别的脚本。  
  
2.  在 " **另存为** " 对话框中，找到要将脚本保存到的文件夹，在 **"文件名" 框中** 输入文件名，然后单击 "确定" SSMA 将追加 .sql 文件扩展名。  
  
### <a name="modifying-scripts"></a>修改脚本  
将 SQL Server 对象定义作为一个或多个脚本保存后，可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 来查看和修改脚本。  
  
**修改脚本**  
  
1.  在“文件”[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 菜单中，指向“打开”，再单击“文件”。  
  
2.  在 " **打开** " 对话框中，选择脚本文件，然后单击 "确定"。
  
3.  使用查询编辑器编辑脚本文件。  
  
    有关查询编辑器的详细信息，请参阅 SQL Server 联机丛书中的 "编辑器便捷命令和功能"。  
  
4.  若要保存脚本，请在 "文件" 菜单上单击 " **保存**"。  
  
### <a name="running-scripts"></a>运行脚本  
您可以在中运行脚本或单独的语句 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。  
  
**运行脚本**  
  
1.  在“文件”[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 菜单中，指向“打开”，再单击“文件”。  
  
2.  在 " **打开** " 对话框中，选择脚本文件，然后单击 "确定"  
  
3.  若要运行完整的脚本，请按 **F5** 键。  
  
4.  若要运行一组语句，请在 "查询编辑器" 窗口中选择语句，然后按 **F5** 键。  
  
有关如何使用查询编辑器来运行脚本的详细信息，请参阅 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] [!INCLUDE[tsql](../../includes/tsql-md.md)] SQL Server 联机丛书中的 "查询"。  
  
还可以通过使用 **sqlcmd** 实用程序和 SQL Server 代理从命令行运行脚本。 有关 **sqlcmd** 的详细信息，请参阅 SQL Server 联机丛书中的 "sqlcmd 实用工具"。 有关 SQL Server 代理的详细信息，请参阅 SQL Server 联机丛书中的 "自动执行管理任务 (SQL Server 代理) "。  
  
## <a name="securing-objects-in-sql-server"></a>在 SQL Server 中保护对象  
将转换后的数据库对象加载到 SQL Server 后，您可以对这些对象授予和拒绝权限。 在将数据迁移到 SQL Server 之前，最好先执行此操作。 有关如何帮助保护 SQL Server 中对象的信息，请参阅 SQL Server 联机丛书中的 "数据库和数据库应用程序的安全注意事项"。  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是将 [数据迁移到 SQL Server](migrating-oracle-data-into-sql-server-oracletosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[将 Oracle 数据库迁移到 SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
