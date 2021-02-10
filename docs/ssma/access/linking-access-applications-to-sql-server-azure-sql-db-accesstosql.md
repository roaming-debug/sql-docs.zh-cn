---
title: 将访问应用程序链接到 SQL Server-Azure SQL 数据库 |Microsoft Docs
description: 了解如何将 Access 表链接到已迁移的表，以便可以将现有的 Access 应用程序用于 SQL Server 或 Azure SQL 数据库。
ms.prod: sql
ms.custom: ''
ms.date: 08/17/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases, linking to SQL Azure
- Access databases, linking to SQL Server
- auto-increment columns
- data types, unsupported
- hyperlink columns
- linking tables
- migrating databases, post-migration issues
- post-migration issues
- reference, post-migration issues
- refreshing linked tables
- slow performance
- unlinking tables
ms.assetid: 82374ad2-7737-4164-a489-13261ba393d4
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 53fbbd5a865594f46eacb75c9799b9c956efb2da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059018"
---
# <a name="linking-access-applications-to-sql-server---azure-sql-database-accesstosql"></a>将访问应用程序链接到 SQL Server-Azure SQL 数据库 (AccessToSQL) 
如果要将现有的 Access 应用程序用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则可以将原始访问表链接到已迁移 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 的表。 链接会修改你的 Access 数据库，以便你的查询、窗体、报表和数据访问页使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库中的数据，而非 access 数据库中的数据。  
  
> [!NOTE]  
> 访问表将保留在 Access 中，但不会随 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 更新一起更新。 链接表并验证功能后，您可能需要删除访问表。  
  
## <a name="linking-access-and-sql-server-tables"></a>链接访问和 SQL Server 表  
当你将 Access 表链接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 表时，Jet 数据库引擎将存储连接信息和表元数据，但数据存储在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 中。 即使实际的表和数据位于或 SQL Azure，此链接也允许访问应用程序对访问表进行操作 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
> [!NOTE]  
> 如果使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证，你的密码将以明文形式存储在链接访问表上。 建议使用 Windows 身份验证。  
  
**链接表**  
  
1.  在 "Access 元数据资源管理器" 中，选择要链接的表。  
  
2.  右键单击 " **表**"，然后选择 " **链接**"。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 迁移助手 (SSMA) 的访问权限备份原始访问表并创建链接表。  
  
链接表后，SSMA 中的表将显示一个小链接图标。 在 Access 中，表以 "链接的" 图标显示，它是一个指向它的箭头。  
  
在 Access 中打开表时，将使用键集游标检索数据。 因此，对于大型表，不会一次检索所有数据。 但是，浏览表时，Access 会根据需要检索其他数据。  
  
> [!IMPORTANT]  
> 若要将 access 表链接到 Azure 数据库，需要 SQL Server Native Client (SNAC) 版本10.5 或更高版本。   
> 你可以从 [Microsoft® SQL Server® 2008 R2 功能包](https://www.microsoft.com/download/details.aspx?id=44272)中获取 SNAC 的最新版本。  
  
## <a name="unlinking-access-tables"></a>取消链接访问表  
在从或 SQL Azure 表中取消链接访问表时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，SSMA 将还原原始访问表及其数据。  
  
**取消表链接**  
  
1.  在 "Access 元数据资源管理器" 中，选择要取消链接的表。  
  
2.  右键单击 " **表**"，然后选择 " **取消链接**"。  
  
## <a name="linking-tables-to-a-different-server"></a>将表链接到不同的服务器  
如果已将 Access 表链接到一个 SQL Server 实例，并且稍后要更改到另一个实例的链接，则必须重新链接这些表。  
  
**将表链接到不同的服务器**  
  
1.  在 "Access 元数据资源管理器" 中，选择要取消链接的表。  
  
2.  右键单击 " **表** "，然后选择 " **取消链接**"。  
  
3.  单击 " **重新连接到 SQL Server** " 按钮。  
  
4.  连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 要将访问表链接到的实例或 SQL Azure。  
  
5.  在 "Access 元数据资源管理器" 中，选择要链接的表。  
  
6.  右键单击 " **表**"，然后选择 " **链接**"。  
  
## <a name="updating-linked-tables"></a>更新链接表  
如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 表定义发生更改，则可以使用本主题前面所述的过程取消链接，然后重新链接 SSMA 中的表。 还可以通过使用 Access 来更新表。  
  
**使用 Access 更新链接表**  
  
1.  打开 Access 数据库。  
  
2.  在 " **对象** " 列表中，单击 " **表**"。  
  
3.  右键单击链接表，然后选择 " **链接表管理器**"。  
  
4.  选中要更新的每个链接表旁边的复选框，然后单击 **"确定"**。  
  
## <a name="possible-post-migration-issues"></a>可能的迁移后问题  
以下各部分列出了在你将数据库从访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 之后，然后链接这些表以及原因和解决方法后，现有访问应用程序中可能会发生的问题。  
  
### <a name="slow-performance-with-linked-tables"></a>链接表性能较低  
**原因：** 由于以下原因，某些查询可能会在升迁后缓慢：  
  
-   应用程序依赖于或 SQL Azure 中不存在的函数 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，这会导致 Jet 在本地提取表以运行选择的查询。  
  
-   用于更新或删除多行的查询将由 Jet 作为每行的参数化查询发送。  
  
**解决方法：** 将运行速度慢的查询转换为传递查询、存储过程或视图。 转换为传递查询具有以下问题：  
  
-   不能修改传递查询。 修改查询结果或添加新记录必须以另一种方式完成，例如通过在窗体上使用显式的 " **修改** " 或 " **添加** " 按钮来绑定到查询。  
  
-   某些查询需要用户输入，但传递查询不支持用户输入。 用户输入可通过 Visual Basic for Applications 提示输入参数 (VBA) 代码，或通过用作输入控件的窗体来获取。 在这两种情况下，VBA 代码都会向服务器提交带有用户输入的查询。  
  
### <a name="auto-increment-columns-are-not-updated-until-the-record-is-updated"></a>只有在更新记录后，才能更新自动增量列  
**原因：** 调用 RecordSet 后，在记录更新之前，"自动递增" 列可用。 在或 SQL Azure 中，这种情况并不成立 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 标识列新值的新值仅在保存新记录之后才可用。  
  
**解决方法：** 在访问标识字段之前，请运行以下 Visual Basic for Applications (VBA) 代码：  
  
```  
Recordset.Update  
Recordset.Move 0,  
Recordset.LastModified  
```  
  
### <a name="new-records-are-not-available"></a>新记录不可用  
**原因：** 使用 VBA 将记录添加到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 表时，如果表的唯一索引字段具有默认值，并且您没有为该字段赋值，则在您重新打开或 SQL Azure 中的表之前，不会显示新记录 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果尝试从新记录中获取值，会收到以下错误消息：  
  
`Run-time error '3167' Record is deleted.`  
  
**解决方法：**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用 VBA 代码打开或 SQL Azure 表时，请包括 `dbSeeChanges` 选项，如以下示例中所示：  
  
`Set rs = db.OpenRecordset("TestTable", dbOpenDynaset, dbSeeChanges)`  
  
### <a name="after-migration-some-queries-will-not-allow-the-user-to-add-a-new-record"></a>迁移后，某些查询将不允许用户添加新记录  
**原因：** 如果查询不包含在唯一索引中包含的所有列，则您不能使用查询来添加新的值。  
  
**解决方法：** 确保至少有一个唯一索引中包含的所有列都是查询的一部分。  
  
### <a name="you-cannot-modify-a-linked-table-schema-with-access"></a>不能使用 Access 修改链接表架构  
**原因：** 迁移数据和链接表后，用户无法在 Access 中修改表的架构。  
  
**解决方法：** 使用修改表架构 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，然后在 "访问" 中更新链接。  
  
### <a name="hyperlink-functionality-is-lost-after-migrating-data"></a>在迁移数据后，超链接功能丢失  
**原因：** 迁移数据后，列中的超链接会丢失其功能，并成为简单的 **nvarchar (最大)** 列。  
  
**解决方法：** 无。  
  
### <a name="some-sql-server-data-types-are-not-supported-by-access"></a>Access 不支持某些 SQL Server 数据类型  
**原因：** 如果以后更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 表包含 access 不支持的数据类型，则不能在 access 中打开该表。  
  
**解决方法：** 您可以定义只返回那些具有受支持数据类型的行的访问查询。  
  
## <a name="see-also"></a>另请参阅  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
