---
title: " (AccessToSQL) 中添加和移除 Access 数据库文件 |Microsoft Docs"
description: 了解如何在 SSMA 项目中添加或删除 Access 数据库，以将 Access 数据迁移到 SQL Server 或 Azure SQL 数据库。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- adding Access files
- adding and removing Access databases
- browsing Access metadata
- browsing metadata, Access databases
- connecting, Access databases
- databases
- files, adding and removing
- finding Access databases
- finding database files
- locating database files
- metadata
- metadata, browsing
- metadata, refreshing
- refreshing metadata
- removing Access files
- scanning for database files
- searching for database files
ms.assetid: e944c740-4c8a-4bc1-b0ed-be57bc06dced
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d8443eaceb150bd0cf2c936e44cb29fc2292f89c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042001"
---
# <a name="adding-and-removing-access-database-files-accesstosql"></a>在 AccessToSQL) 中添加和删除 Access 数据库文件 (
若要将访问数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，必须将一个或多个 access 数据库添加到 SSMA 项目。 这些数据库必须是 Access 97 或更高版本。 如果你的数据库来自早期版本的 Access，则必须将数据库转换为较新的版本。 为此，请在 Access 97 或更高版本中打开并保存数据库，然后将它们添加到 SSMA。  
  
## <a name="what-happens-when-you-add-access-database-files"></a>添加 Access 数据库文件后会出现什么情况？  
将 Access 数据库添加到 SSMA 项目时，SSMA 将读取数据库元数据，然后将此元数据添加到项目文件。 此元数据对数据库及其对象进行了说明。 SSMA 在将对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 语法，以及在将数据迁移到或 SQL Azure 时使用元数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 可以在 Access 元数据资源管理器中浏览此元数据，并查看单个数据库对象的属性。  
  
> [!NOTE]  
> Access 数据库可以拆分为多个文件：一个后端数据库，其中包含表和前端数据库，其中包含查询、窗体、报表、宏、模块和快捷方式。 如果要将拆分的数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，请将前端数据库添加到 SSMA。  
  
## <a name="permissions-that-are-required-by-ssma"></a>SSMA 所需的权限  
若要将 Access 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure，用户组和管理员用户必须拥有 "管理" 权限。 有关如何使用工作组保护迁移数据库的信息，请参阅 [准备用于迁移的 Access 数据库](preparing-access-databases-for-migration-accesstosql.md)。  
  
## <a name="selecting-databases-to-add"></a>选择要添加的数据库  
如果要将一个或多个数据库添加到 SSMA 项目，并且这些文件都在一个已知位置，则可以使用以下过程来添加文件。  
  
**添加单个数据库文件**  
  
1.  在 " **文件** " 菜单上，单击 " **添加数据库**"。  
  
2.  在 " **打开** " 对话框中，找到包含数据库文件的文件夹。  
  
3.  选择要添加的文件，然后单击 " **打开**"。  
  
## <a name="finding-databases-to-add"></a>查找要添加的数据库  
如果要将不同文件夹中的多个 Access 数据库添加到 SSMA 项目，或者要添加单个文件但需要查找该文件，则可以按照以下步骤找到一个或多个文件，并将其添加到项目。  
  
**查找和添加数据库**  
  
1.  在 " **文件** " 菜单上，单击 " **查找数据库**"。  
  
2.  在 "查找数据库" 向导中，输入要搜索的驱动器、文件路径或 UNC 路径的名称。 或者，单击 " **浏览** " 找到驱动器或网络文件夹。  
  
3.  单击 " **添加** " 将位置添加到列表。  
  
    重复上述两个步骤以添加更多搜索位置。  
  
4.  （可选）添加搜索条件以优化返回的数据库的列表。  
  
    > [!IMPORTANT]  
    > "文件名" 文本框的 **全部或部分** 内容不支持通配符。  
  
5.  单击 " **扫描**"。  
  
    此时将显示 "扫描" 页。 这会显示已找到的数据库和搜索进度。 若要停止搜索，请单击 " **停止**"。  
  
6.  在 "选择文件" 页上，选择要添加到项目中的数据库。  
  
    您可以使用列表顶部的 " **全选** " 和 " **全部清除** " 按钮来选择或清除所有数据库。 您可以按住 CTRL 键来选择多个数据库，或按住 SHIFT 键以选择数据库的范围。  
  
7.  单击 **“下一步”** 。  
  
8.  在 "验证" 页上，单击 " **完成**"。  
  
## <a name="browsing-access-metadata"></a>浏览访问元数据  
将 Access 数据库添加到项目后，项目元数据将显示在 "Access 元数据资源管理器" 中。 您可以在资源管理器中浏览数据库和数据库对象的层次结构。  
  
**浏览元数据**  
  
1.  在 Access 元数据资源管理器中，展开 " **access-元数据库**"，然后展开 " **数据库**"。  
  
2.  展开要查看的数据库，然后展开 " **查询**"。  
  
    请注意查询列表。 如果选择查询，将在右窗格中显示 **SQL** 选项卡和 " **属性** " 选项卡。  
  
3.  展开 " **表** "，然后选择一个表。  
  
    请注意，将显示以下四个选项卡： **表**、 **类型映射**、 **属性** 和 **数据**。  
  
4.  展开一个表，展开 " **键**"，然后选择一个键。  
  
    键属性将显示在右窗格中。  
  
5.  展开 " **索引**"，然后选择索引。  
  
    索引属性将显示在右窗格中。  
  
## <a name="refreshing-databases"></a>刷新数据库  
如果 Access 数据库在您添加其文件后发生更改，则您可以从 Access 数据库中更新元数据。  
  
**更新访问元数据**  
  
-   在 "Access 元数据资源管理器" 中，右键单击数据库，然后选择 " **从数据库刷新**"。  
  
## <a name="removing-databases"></a>删除数据库  
可以通过执行以下步骤，从项目中删除访问数据库。  
  
**从项目中删除数据库**  
  
1.  在 Access 元数据资源管理器中，展开 " **access-元数据库**"，然后展开 " **数据库**"。  
  
2.  右键单击该数据库，然后选择 " **删除数据库**"。  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是 [连接到 SQL Server](../sybase/connecting-to-sql-server-sybasetosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[创建和管理项目](creating-and-managing-projects-accesstosql.md)  
