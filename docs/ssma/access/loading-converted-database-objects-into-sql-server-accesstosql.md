---
description: '将转换后的数据库对象加载到 SQL Server (AccessToSQL) '
title: 将转换后的数据库对象加载到 SQL Server (AccessToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases, loading converted objects into SQL Azure
- Access databases, loading converted objects into SQL Server
- data, securing
- loading objects into SQL Azure
- loading objects into SQL Server
- migrating objects into SQL Azure
- migrating objects into SQL Server
- moving objects into SQL Azure
- moving objects into SQL Server
- schemas, loading into SQL Azure
- schemas, loading into SQL Server
- scripting converted objects
- securing data
- SQL Azure, loading objects into
- SQL Server, loading objects into
- synchronizing metadata with SQL Azure
- synchronizing metadata with SQL Server
- uploading objects into SQL Azure
- uploading objects into SQL Server
ms.assetid: 4e854eee-b10c-4f0b-9d9e-d92416e6f2ba
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4fb81a05cebfaa1a2c59f7e2b90d8289412e140a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071113"
---
# <a name="loading-converted-database-objects-into-sql-server-accesstosql"></a>将转换后的数据库对象加载到 SQL Server (AccessToSQL) 
将 Access 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 后，可以将生成的数据库对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 中。 您可以使用 SSMA 创建这些对象，也可以编写对象脚本并自己运行脚本。 此外，SSMA 允许你用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库的实际内容更新目标元数据。  
  
## <a name="choosing-between-synchronization-and-scripts"></a>在同步和脚本之间进行选择  
如果要在不进行修改的情况下将转换后的数据库对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，则可以让 SSMA 直接创建或重新创建数据库对象。 这种方法既简单又简单，但不允许自定义用于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 定义或 SQL Azure 对象的代码（而不是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程）。  
  
如果要修改 [!INCLUDE[tsql](../../includes/tsql-md.md)] 用于创建对象的，或者如果想要对对象创建进行更多的控制，请使用 SSMA 创建脚本。 然后，你可以修改这些脚本，单独创建每个对象，甚至使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理来计划创建这些对象。  
  
## <a name="using-ssma-to-synchronize-objects-with-sql-server"></a>使用 SSMA 将对象与 SQL Server 同步  
若要使用 SSMA 创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库对象，请在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器 "中选择对象，然后将对象与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 同步，如以下过程中所示。 默认情况下，如果对象已存在于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 中，并且 SSMA 元数据具有对这些对象的定义的一些本地更改或更新，则 SSMA 将更改或 SQL Azure 中的对象定义 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 您可以通过编辑 **项目设置** 来更改默认行为。  
  
> [!NOTE]  
> 你可以选择 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不是从 Access 数据库转换的现有或 AZURE SQL 数据库对象。 但是，SSMA 不会重新创建或更改这些对象。  
  
**与 SQL Server 或 SQL Azure 同步对象**  
  
1.  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器中，展开顶部 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure "节点，然后展开" **数据库**"。  
  
2.  选择要处理的对象：  
  
    -   若要同步完整数据库，请选中数据库名称旁边的复选框。  
  
    -   若要同步或忽略对象的单个对象或类别，请选中或清除对象或文件夹旁边的复选框。  
  
3.  选择要处理的对象 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 元数据资源管理器后，右键单击 " **数据库**"，然后单击 " **与数据库同步**"。  
  
    您还可以通过右键单击对象或其父文件夹，然后单击 " **与数据库同步**"，来同步各个对象或对象类别。  
  
    然后，SSMA 将显示与 **数据库同步** 对话框，您可以在其中看到两组项。 在左侧，SSMA 显示了树中表示的所选数据库对象。 在右侧，可以看到表示 SSMA 元数据中的相同对象的树。 可以通过单击右侧或左侧的 "+" 按钮展开树。 同步方向显示在两个树之间放置的 "操作" 列中。  
  
    操作签名可以有三种状态：  
  
    -   向左箭头表示元数据的内容将保存在数据库 (默认的) 中。  
  
    -   向右箭头表示数据库内容将覆盖 SSMA 元数据。  
  
    -   交叉符号表示不会执行任何操作。  
  
    单击操作符号以更改状态。 当单击 "**与数据库同步**" 对话框中的 **"确定"** 按钮时，将执行实际同步。  
  
## <a name="scripting-objects"></a>脚本对象  
如果要保存 [!INCLUDE[tsql](../../includes/tsql-md.md)] 转换后的数据库对象的定义，或者您想要更改对象定义并自己运行脚本，则可以将转换后的数据库对象定义保存到 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本中。  
  
**将一个或多个对象保存到脚本**  
  
1.  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] "元数据资源管理器" 中，展开顶部节点 (服务器名称) 然后展开 " **数据库**"。  
  
2.  执行以下一项或多项操作：  
  
    -   若要为完整数据库编写脚本，请选中数据库名称旁边的复选框。  
  
    -   若要编写或省略单个视图，请展开数据库，展开 " **视图**"，然后选中或清除视图旁边的复选框。  
  
    -   若要编写或省略单个表，请展开数据库，展开 " **表**"，然后选中或清除表旁边的复选框。  
  
    -   若要为表编写或省略单个索引，请展开该表，展开 " **索引**"，然后选择或清除索引。  
  
3.  右键单击 " **数据库** "，然后选择 " **另存为脚本**"。  
  
    你还可以编写单个对象的脚本。 若要为对象编写脚本，无论选择了哪个对象，右键单击该对象并选择 " **另存为脚本**"。  
  
4.  在 " **另存为** " 对话框中，找到要将脚本保存到的文件夹，在 **"文件名" 框中** 输入文件名，然后单击 **"确定"**。  
  
    SSMA 将追加 .sql 文件扩展名。  
  
### <a name="modifying-scripts"></a>修改脚本  
将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 对象定义保存为脚本后，可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 来修改脚本。  
  
**修改脚本**  
  
1.  在“文件”[!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 菜单中，指向“打开”，再单击“文件”。  
  
2.  在 " **打开** " 对话框中，找到并选择脚本文件，然后单击 **"确定"**。  
  
3.  使用查询编辑器编辑脚本文件。  
  
    有关查询编辑器的详细信息，请参阅联机丛书中的 "编辑器便捷命令和功能" [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
4.  若要保存脚本，请在 "文件" 菜单上选择 " **保存**"。  
  
### <a name="running-scripts"></a>运行脚本  
您可以在中运行脚本或单独的语句 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。  
  
**运行脚本**  
  
1.  在 " [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **文件** " 菜单上，指向 " **打开** "，然后单击 " **文件**"。  
  
2.  在 " **打开** " 对话框中，找到并选择脚本文件，然后单击 **"确定"**。  
  
3.  若要运行完整的脚本，请按 **F5** 键。  
  
4.  若要运行一组语句，请在 "查询编辑器" 窗口中选择语句，然后按 **F5** 键。  
  
有关如何使用查询编辑器来运行脚本的详细信息，请参阅 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] [!INCLUDE[tsql](../../includes/tsql-md.md)] 联机丛书中的 "查询" [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
还可以通过使用 **sqlcmd** 实用程序和代理从命令行运行脚本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关 **sqlcmd** 的详细信息，请参阅联机丛书中的 "sqlcmd 实用工具" [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关代理的详细信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请参阅联机丛书中的 " ( 代理) 自动执行管理任务 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] " [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="securing-objects-in-sql-server"></a>在 SQL Server 中保护对象  
将转换后的数据库对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中后，您可以对这些对象授予和拒绝权限。 在将数据迁移到之前，最好先执行此操作 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关如何帮助保护中的对象的信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请参阅联机丛书中的 "数据库和数据库应用程序的安全注意事项" [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是将 [数据迁移到 SQL Server](migrating-access-data-into-sql-server-azure-sql-db-accesstosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
