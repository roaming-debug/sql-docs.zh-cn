---
description: " (AccessToSQL) 导出访问清单"
title: " (AccessToSQL) 导出访问清单 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- Access databases, exporting metadata
- exporting
- exporting Access metadata
- exporting, Access metadata
- exporting, querying exported metadata
- inventories of Access databases
- querying exported metadata
ms.assetid: 7e1941fb-3d14-4265-aff6-c77a4026d0ed
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 10bff5f86bec26994c143195fcbb97aa30ba709e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044737"
---
# <a name="exporting-an-access-inventory-accesstosql"></a> (AccessToSQL) 导出访问清单
如果你有多个访问数据库，但不确定要将哪些数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则可以在项目中导出所有 Access 数据库的清单。 然后，可以查看和查询清单元数据，以确定要迁移的数据库中的数据库和对象。 此清单使你可以快速找到问题的答案，例如：  
  
-   最大数据库是什么？  
  
-   谁拥有大部分数据库？  
  
-   哪些数据库包含相同的表？  
  
-   最近六个月内未修改哪些数据库？  
  
-   哪些数据库包含私密信息？  
  
本主题末尾提供了用于回答这些问题的查询示例。  
  
## <a name="exported-metadata"></a>导出的元数据  
SSMA 导出有关访问数据库、表、列、索引、外键、查询、报表、窗体、宏和模块的元数据。 有关这些项的每个类别的元数据都导出到单独的表中。 有关这些表的架构，请参阅 [访问库存架构](access-inventory-schemas-accesstosql.md)。  
  
## <a name="exporting-inventory-data"></a>导出清单数据  
若要导出访问清单，必须首先打开或创建 SSMA 项目，然后添加要分析的 Access 数据库。 将数据库添加到 SSMA 项目后，可以将有关这些数据库的元数据导出到指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库和架构。 如有必要，SSMA 会创建用于存储元数据的表。 然后，SSMA 将有关 Access 数据库的元数据添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。  
  
> [!NOTE]  
> Access 数据库可以拆分为多个文件：一个后端数据库，其中包含表和前端数据库，其中包含查询、窗体、报表、宏、模块和快捷方式。 如果要将拆分的数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请将前端数据库添加到 SSMA。  
  
以下说明介绍了如何创建项目，如何将数据库添加到项目，连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，然后导出清单数据。  
  
**创建项目**  
  
1.  打开 SSMA 以进行访问。  
  
2.  在“文件”菜单中，选择“新建项目”。  
  
    将显示“新建项目”对话框。  
  
3.  在 " **名称** " 框中，输入项目的名称。  
  
4.  在 " **位置** " 框中，为项目输入或选择一个文件夹。  
  
5.  在 " **迁移到** " 组合框中，选择要迁移到的目标版本，然后单击 **"确定"**。  
  
有关创建项目的详细信息，请参阅 [创建和管理项目](creating-and-managing-projects-accesstosql.md)。  
  
**查找和添加数据库**  
  
1.  在 " **文件** " 菜单上，单击 " **查找数据库**"。  
  
2.  在 "查找数据库" 向导中，输入要搜索的驱动器、文件路径或 UNC 路径。 或者，单击 " **浏览** " 选择驱动器或网络文件夹。  
  
3.  单击 " **添加** " 将位置添加到列表框中。  
  
    重复上述两个步骤，添加其他搜索位置。  
  
4.  （可选）添加搜索条件以优化返回的数据库的列表。  
  
    > [!IMPORTANT]  
    > "文件名" 文本框的 **全部或部分** 内容不支持通配符。  
  
5.  单击 " **扫描**"。  
  
    此时将显示 "扫描" 页。 这会显示已找到的数据库和搜索进度。 若要停止搜索，请单击 " **停止**"。  
  
6.  在 "选择文件" 页上，选择要添加到项目中的每个数据库。  
  
    您可以使用列表顶部的 " **全选** " 和 " **全部清除** " 按钮来选择或清除所有数据库。 还可以按住 CTRL 键来选择多个行，或按住 SHIFT 键以选择某一范围的行。  
  
7.  单击 **“下一步”** 。  
  
8.  在 "验证" 页上，单击 " **完成**"。  
  
有关将数据库添加到项目的详细信息，请参阅 [添加和删除 Access 数据库文件](adding-and-removing-access-database-files-accesstosql.md)。  
  
**连接到 SQL Server**  
  
1.  在 " **文件** " 菜单上，选择 " **连接到 SQL Server**"。  
  
2.  在 "连接" 对话框中，输入或选择实例的名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
    -   如果要连接到本地计算机上的默认实例，则可以输入 **localhost** 或点 (**。**) 。  
  
    -   如果要连接到另一台计算机上的默认实例，请输入计算机的名称。  
  
    -   如果要连接到命名实例，请输入计算机名称、反斜杠和实例名称。 例如： MyServer\MyInstance。  
  
3.  在 " **数据库** " 框中，输入导出的元数据的目标数据库的名称。  
  
4.  如果实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置为接受非默认端口上的连接，请 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 " **服务器端口** " 框中输入用于连接的端口号。 对于的默认实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，默认端口号为1433。 对于命名实例，SSMA 尝试从 Browser 服务获取端口号 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
5.  在 " **身份验证** " 下拉菜单中，选择要用于连接的身份验证类型。 若要使用当前的 Windows 帐户，请选择 " **Windows 身份验证**"。 若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，请选择 " **SQL Server 身份验证**"，然后提供用户名和密码。  
  
有关连接到的详细信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请参阅 [连接到 SQL Server &#40;AccessToSQL&#41;](../../ssma/access/connecting-to-sql-server-accesstosql.md)。  
  
**导出清单信息**  
  
1.  在 Access 元数据资源管理器中，展开 " **access-元数据库**"。  
  
2.  选中 " **数据库**" 旁边的复选框。  
  
    若要省略单个数据库或数据库对象，请展开 " **数据库** " 文件夹，然后清除数据库或数据库对象旁边的复选框。  
  
3.  右键单击 " **数据库** "，然后选择 " **导出架构**"。  
  
4.  在 " **选择导出的架构** " 对话框中，选择导出的元数据的目标架构，然后单击 **"确定"**。  
  
每次导出元数据时，SSMA 会将数据追加到库存。 清单中的现有数据不会更新或删除。  
  
## <a name="querying-the-exported-metadata"></a>查询导出的元数据  
导出有关 Access 数据库的元数据后，可以查询元数据。 下面的说明介绍了如何使用中的 "查询编辑器" 窗口 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 来运行查询。  
  
**查询元数据**  
  
1.  从 " **开始** " 菜单，依次指向 " **所有程序**" **、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Microsoft 2005** 或 **microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008** 或 **microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012**，然后单击 " **SQL Server Management Studio**"。  
  
2.  在 " **连接到服务器** " 对话框中，验证设置，然后单击 " **连接**"。  
  
3.  在 Management Studio 工具栏上，单击 " **新建查询** " 以打开查询编辑器。  
  
4.  在查询编辑器窗口中，输入一个查询。 以下部分演示了一些示例。  
  
5.  按 F5 键以运行查询。  
  
## <a name="query-examples"></a>查询示例  
在运行以下任何查询之前，您应该运行 *database_name* 查询，以确保查询是针对包含导出的元数据的数据库运行的。 例如，如果将元数据导出到名为 MyAccessMetadata 的数据库，则需要在代码的开头添加以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 内容：  
  
```  
USE MyAccessMetadata;  
GO  
```  
以下示例都使用 **dbo** 架构。 如果将元数据导出到另一个架构，请确保在运行这些查询时更改架构。  
  
### <a name="what-tables-and-columns-are-in-these-databases"></a>这些数据库中的表和列是什么？  
下面的查询联接包含列、表和数据库元数据的表，然后返回按列名称排序的所有数据库、表和列的名称：  
  
```  
SELECT DatabaseName, TableName, ColumnName   
FROM dbo.SSMA_Access_InventoryColumns C  
JOIN dbo.SSMA_Access_InventoryTables T  
ON C.TableId = T.TableId  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON T.DatabaseId = D.DatabaseId  
ORDER BY ColumnName;  
```  
  
### <a name="what-are-the-largest-databases"></a>最大数据库是什么？  
下面的查询返回每个 Access 数据库中的数据库名称、文件大小和表的数目，按文件大小排序：  
  
```  
SELECT DatabaseName, FileSize, TablesCount  
FROM dbo.SSMA_Access_InventoryDatabases  
ORDER BY FileSize DESC;  
```  
  
### <a name="who-is-the-owner-of-most-of-the-databases"></a>大多数数据库的所有者是谁？  
下面的查询返回每个 Access 数据库的数据库名称和所有者，并按所有者进行排序。  
  
```  
SELECT DatabaseName, FileOwner  
FROM dbo.SSMA_Access_InventoryDatabases  
ORDER BY FileOwner;  
```  
  
### <a name="which-databases-contain-the-same-tables"></a>哪些数据库包含相同的表？  
下面的查询使用子查询查找在表列表中出现多次的所有表名，然后使用此表列表来获取数据库名称。 结果以数据库名称和表名称的形式返回，并按表名进行排序。  
  
```  
SELECT DatabaseName, TableName   
FROM dbo.SSMA_Access_InventoryTables T  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON D.DatabaseId = T.DatabaseId  
WHERE TableName IN (  
SELECT TableName   
FROM dbo.SSMA_Access_InventoryTables  
GROUP BY TableName   
HAVING count(*)>1  
)  
ORDER BY TableName;  
```  
  
### <a name="which-databases-were-not-modified-in-the-last-six-months"></a>最近六个月内未修改哪些数据库？  
下面的查询获取当前日期，获取六个月之前的月份值，然后返回修改日期超过六个月之前的数据库的列表。  
  
```  
SELECT DatabaseName, DateModified  
FROM dbo.SSMA_Access_InventoryDatabases  
WHERE DATEDIFF(month, DateModified, GETDATE()) > 6  
ORDER BY DateModified;  
```  
  
### <a name="which-databases-contain-private-information"></a>哪些数据库包含私密信息？  
Access 数据库可能包含敏感信息或个人信息。 你可能希望将这些数据库移到， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以利用其安全功能。 如果知道包含敏感数据的列具有特定名称或包含特定字符，则可以使用查询来查找包含该信息的所有列。 例如，可以查找包含字符串 "salary" 的所有列。  然后，该查询将返回数据库名称、表名称和列名称。  
  
```  
SELECT DatabaseName, TableName, ColumnName   
FROM dbo.SSMA_Access_InventoryColumns C  
JOIN dbo.SSMA_Access_InventoryTables T  
ON C.TableId = T.TableId  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON T.DatabaseId = D. DatabaseId  
WHERE ColumnName LIKE '%salary%';  
```  
如果您不知道列名称，则可以编写查询以返回所有列。 为此，请从前面的查询中删除 WHERE 子句。  
  
## <a name="see-also"></a>另请参阅  
[准备要迁移的 Access 数据库](preparing-access-databases-for-migration-accesstosql.md)  
  
