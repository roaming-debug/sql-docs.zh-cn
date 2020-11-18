---
title: 连接到 DB2 数据库 (DB2ToSQL) |Microsoft Docs
description: 了解如何连接到 DB2 数据库的目标实例以迁移 DB2 数据库。 SSMA 获取有关所有 DB2 架构的元数据。
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 5eb5801d-f0c3-4127-97c0-0b1ef49f4844
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d0ac703c8ea155f33ecb713b98a26f0c39b5a695
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870066"
---
# <a name="connecting-to-db2-database-db2tosql"></a>连接到 DB2 数据库 (DB2ToSQL) 

若要将 DB2 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，你必须连接到要迁移的 db2 数据库。 当你连接时，SSMA 将获取有关所有 DB2 架构的元数据，然后在 DB2 元数据资源管理器窗格中显示它。 SSMA 存储有关数据库服务器的信息，但不存储密码。

在关闭项目之前，与数据库的连接保持活动状态。 重新打开项目时，如果要连接到数据库，则必须重新连接。

不会自动更新有关 DB2 数据库的元数据。 相反，如果要更新 DB2 元数据资源管理器中的元数据，则必须手动对其进行更新。 有关详细信息，请参阅本主题后面的 "刷新 DB2 元数据" 一节。

## <a name="required-db2-permissions"></a>必需的 DB2 权限

"用户授权" 定义可供用户使用的命令和对象的列表。 此列表将控制用户操作。 在 DB2 中，有预先确定的特权组可以在实例级别和 DB2 数据库级别进行授权。 这使 SSMA 可以从连接用户所拥有的架构中获取元数据。 若要获取其他架构中的对象的元数据，然后转换这些架构中的对象，该帐户必须具有以下权限：

- 除非 CREATE 中使用了 RESTRICT 关键字，否则通常会将架构迁移的架构访问权限授予 PUBLIC
- 数据迁移的数据访问需要 DATAACCESS

## <a name="establishing-a-connection-to-db2"></a>建立与 DB2 的连接

连接到数据库时，SSMA 将读取数据库元数据，然后将此元数据添加到项目文件。 当 SSMA 将对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语法以及将其迁移到时，将使用此元数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 您可以在 DB2 元数据资源管理器窗格中浏览此元数据，并查看各个数据库对象的属性。  

> [!IMPORTANT]
> 尝试连接之前，请确保数据库服务器正在运行，并且可以接受连接。

**连接到 DB2**

1. 在 " **文件** " 菜单上，选择 " **连接到 DB2**"。

   如果以前连接到 DB2，则命令名称将 **重新连接到 db2**。

2. 在 " **提供程序** " 框中，你将看到当前仅为 DB2 客户端访问提供程序的 **OLE DB 提供程序** 。

3. 在 " **管理器** " 框中，可以选择 **db2 for zOs**、 **db2 for LUW** 或 **db2 for i**

4. 在 " **模式** " 框中，选择 " **标准模式**" 或 " **连接字符串模式**"。

   使用 "标准" 模式指定服务器名称和端口。 使用服务名称模式手动指定 DB2 服务名称。 使用连接字符串模式提供完整的连接字符串。

5. 如果选择 " **标准" 模式**，请提供以下值：

   - 在 " **服务器名称** " 框中，输入或选择数据库服务器的名称或 IP 地址。
   - 如果数据库服务器未配置为接受默认端口 (1521) 上的连接，请在 " **服务器端口** " 框中输入用于 DB2 连接的端口号。
   - 在 " **服务器端口** " 框中，输入 tcp/ip 端口号。
   - 在 " **初始目录** " 框中，输入数据库名称。
   - 在 " **用户名** " 框中，输入具有所需权限的 DB2 帐户。
   - 在 " **密码** " 框中，输入指定用户名的密码。

6. 如果选择 " **连接字符串模式**"，请在 " **连接字符串** " 框中提供连接字符串。

   下面的示例演示一个 OLE DB 连接字符串：

   `Provider=OraOLEDB.DB2;Data Source=MyDB2DB;User Id=myUsername;Password=myPassword;`

   以下示例显示了使用集成安全性的 DB2 客户端连接字符串：
  
   `Data Source=MyDB2DB;Integrated Security=yes;`

   有关详细信息，请参阅 [连接到 Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md)。
  
## <a name="reconnecting-to-db2"></a>重新连接到 DB2

与数据库服务器的连接会一直保持活动状态，直到关闭该项目。 重新打开项目时，如果要连接到数据库，则必须重新连接。 你可以脱机工作，直至你要更新元数据、将数据库对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和迁移数据。

## <a name="refreshing-db2-metadata"></a>刷新 DB2 元数据

不会自动刷新有关 DB2 数据库的元数据。 DB2 元数据资源管理器中的元数据是首次连接时或上次手动刷新元数据时的元数据的快照。 可以手动更新所有架构、单个架构或单个数据库对象的元数据。

**刷新元数据**

1. 请确保已连接到数据库。
2. 在 DB2 元数据资源管理器中，选中要更新的每个架构或数据库对象旁边的复选框。
3. 右键单击 " **架构**"、单个架构或数据库对象，然后选择 " **从数据库刷新**"。

   如果没有活动连接，SSMA 将显示 " **连接到 DB2** " 对话框，以便你可以连接。
  
4. 在 "从数据库刷新" 对话框中，指定要刷新的对象。
   - 若要刷新对象，请单击对象旁边的 **活动** 字段，直到出现箭头。
   - 若要防止对象被刷新，请单击对象旁边的 **活动** 字段，直到出现 **X** 。
   - 若要刷新或拒绝某个对象类别，请单击 "category" 文件夹旁边的 **活动** 字段。

     若要查看颜色编码的定义，请单击 " **图例** " 按钮。

5. [!INCLUDE[click OK](../../includes/clickok-md.md)]

## <a name="next-step"></a>下一步

- 迁移过程的下一步是 [连接到 SQL Server](./connecting-to-sql-server-db2tosql.md)。

## <a name="see-also"></a>另请参阅

- [将 DB2 数据库迁移到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)