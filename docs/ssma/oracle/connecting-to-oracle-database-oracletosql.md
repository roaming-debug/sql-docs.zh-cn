---
title: 正在连接到 Oracle Database (OracleToSQL) |Microsoft Docs
description: 了解如何连接到 Oracle 数据库以将 Oracle 数据库迁移到 SQL Server。 SSMA 获取并显示有关所有 Oracle 架构的元数据。
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 06/04/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Refreshing Oracle Metadata
ms.assetid: e276cdbf-3ebc-4ba8-b40d-a7a42befa2b6
ms.author: alexiva
ms.openlocfilehash: 4354a4e2726151177c48c27ce37a74e5b4ac335e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058782"
---
# <a name="connecting-to-oracle-database-oracletosql"></a>连接到 Oracle Database (OracleToSQL)

若要将 Oracle 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，你必须连接到要迁移的 oracle 数据库。 当你连接时，SSMA 将获取有关所有 Oracle 架构的元数据，然后在 "Oracle 元数据资源管理器" 窗格中显示它。 SSMA 存储有关数据库服务器的信息，但不存储密码。

在关闭项目之前，与数据库的连接保持活动状态。 重新打开项目时，如果要连接到数据库，则必须重新连接。

有关 Oracle 数据库的元数据不会自动更新。 相反，如果要更新 Oracle 元数据资源管理器中的元数据，则必须手动对其进行更新。 有关详细信息，请参阅本主题后面的 "刷新 Oracle 元数据" 一节。

## <a name="required-oracle-permissions"></a>必需的 Oracle 权限

用于连接到 Oracle 数据库的帐户至少必须具有以下权限：

- `CONNECT`  
  连接 (需要为数据库创建会话) 。

- `SELECT ANY DICTIONARY`  
  需要查询系统字典表 (例如，) 以便 `SYS.MLOG$` 发现所有对象。

这将允许 SSMA 加载连接用户所拥有的架构中的所有对象。 在大多数实际情况下，存储过程和 SSMA 之间存在跨架构引用，需要能够发现所有被引用的对象才能成功转换。 若要获取在其他架构中定义的对象的元数据，该帐户必须具有以下附加权限：

- `SELECT ANY TABLE`  
  需要发现其他架构中的表、视图、具体化视图和同义词。

- `SELECT ANY SEQUENCE`  
  需要在其他架构中发现序列。

- `CREATE ANY PROCEDURE`  
  需要在其他架构中发现过程、函数和包的 PL/SQL。

- `CREATE ANY TRIGGER`  
  需要在其他架构中发现触发器定义。

- `CREATE ANY TYPE`  
  需要发现在其他架构中定义的类型。

某些 SSMA 功能需要额外的权限。 例如，如果想要使用 [测试人员](testing-migrated-database-objects-oracletosql.md) 和 [备份管理](managing-backups-oracletosql.md) 功能，则需要为连接用户授予以下权限：

- `EXECUTE ANY PROCEDURE`  
  需要运行要在所有架构中测试的过程和函数。

- `CREATE ANY TABLE` 和 `ALTER ANY TABLE`  
  需要为更改跟踪和备份创建和修改临时表。

- `INSERT ANY TABLE` 和 `UPDATE ANY TABLE`  
  需要将更改跟踪和备份数据插入临时表中。

- `DROP ANY TABLE`  
  需要删除用于更改跟踪和备份的临时表。

- `CREATE ANY INDEX` 和 `ALTER ANY INDEX`  
  需要在用于更改跟踪和备份的临时表上创建和修改索引。

- `DROP ANY INDEX`  
  需要删除用于更改跟踪和备份的临时表的索引。

- `CREATE ANY TRIGGER` 和 `ALTER ANY TRIGGER`  
  需要创建和修改用于更改跟踪的临时触发器。

- `DROP ANY TRIGGER`  
  需要删除用于更改跟踪的临时触发器。

> [!NOTE]
> 这是 SSMA 正常运行所需的一般权限集。 如果要将迁移范围缩小到部分架构，可以通过向有限的对象集（而不是）授予上述权限来执行此操作 `ALL` 。 尽管可能，正确地识别所有依赖项可能很困难，从而使 SSMA 无法正常运行。 强烈建议使用以上定义的通用集来消除迁移过程中的任何潜在权限问题。

## <a name="establishing-a-connection-to-oracle"></a>建立与 Oracle 的连接

连接到数据库时，SSMA 将读取数据库元数据，然后将此元数据添加到项目文件。 当 SSMA 将对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语法以及将其迁移到时，将使用此元数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 您可以在 "Oracle 元数据资源管理器" 窗格中浏览此元数据，查看各个数据库对象的属性。

> [!IMPORTANT]
> 尝试连接之前，请确保数据库服务器正在运行，并且可以接受连接。

**连接到 Oracle**

1. 在 " **文件** " 菜单上，选择 " **连接到 Oracle**"。  
   如果以前已连接到 Oracle，则命令名称将 **重新连接到 oracle**。
  
2. 根据安装的提供程序，在 " **提供程序** " 框中选择 " **Oracle 客户端提供程序** " 或 " **OLE DB 提供程序**"。 默认值为 "Oracle 客户端"。

3. 在 " **模式** " 框中，选择 " **标准模式**"、" **TNSNAME 模式**" 或 " **连接字符串模式**"。  
   使用 "标准" 模式指定服务器名称和端口。 使用服务名称模式手动指定 Oracle 服务名称。 使用连接字符串模式提供完整的连接字符串。

4. 如果选择 " **标准" 模式**，请提供以下值：
   1. 在 " **服务器名称** " 框中，输入或选择数据库服务器的名称或 IP 地址。
   2. 如果数据库服务器未配置为接受默认端口 (1521) 上的连接，请在 " **服务器端口** " 框中输入用于 Oracle 连接的端口号。
   3. 在 " **ORACLE SID** " 框中，输入系统标识符。
   4. 在 " **用户名** " 框中，输入具有所需权限的 Oracle 帐户。
   5. 在 " **密码** " 框中，输入指定用户名的密码。

5. 如果选择 **TNSNAME 模式**，请提供以下值：
   1. 在 " **连接标识符** " 框中，输入数据库的连接标识符 (TNS 别名) 。
   2. 在 " **用户名** " 框中，输入具有所需权限的 Oracle 帐户。
   3. 在 " **密码** " 框中，输入指定用户名的密码。
  
6. 如果选择 " **连接字符串模式**"，请在 " **连接字符串** " 框中提供连接字符串。  
   下面的示例演示一个 OLE DB 连接字符串：

   `Provider=OraOLEDB.Oracle;Data Source=MyOracleDB;User Id=myUsername;Password=myPassword;`

   下面的示例演示了使用集成安全性的 Oracle 客户端连接字符串：

   `Data Source=MyOracleDB;Integrated Security=yes;`

   有关详细信息，请参阅 [连接到 Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md)。

## <a name="reconnecting-to-oracle"></a>重新连接到 Oracle

与数据库服务器的连接会一直保持活动状态，直到关闭该项目。 重新打开项目时，如果要连接到数据库，则必须重新连接。 你可以脱机工作，直至你要更新元数据、将数据库对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和迁移数据。

## <a name="refreshing-oracle-metadata"></a>刷新 Oracle 元数据

有关 Oracle 数据库的元数据不会自动刷新。 Oracle 元数据资源管理器中的元数据是首次连接时或上次手动刷新元数据时的元数据的快照。 可以手动更新所有架构、单个架构或单个数据库对象的元数据。

**刷新元数据**

1. 请确保已连接到数据库。

2. 在 Oracle 元数据资源管理器中，选中要更新的每个架构或数据库对象旁边的复选框。

3. 右键单击 " **架构**"、单个架构或数据库对象，然后选择 " **从数据库刷新**"。  
   如果没有活动连接，SSMA 将显示 " **连接到 Oracle** " 对话框，以便你可以连接。

4. 在 "从数据库刷新" 对话框中，指定要刷新的对象。

   - 若要刷新对象，请单击对象旁边的 **活动** 字段，直到出现箭头。
   - 若要防止对象被刷新，请单击对象旁边的 **活动** 字段，直到出现 **X** 。
   - 若要刷新或拒绝某个对象类别，请单击 "category" 文件夹旁边的 **活动** 字段。

   若要查看颜色编码的定义，请单击 " **图例** " 按钮。

5. [!INCLUDE[clickOK](../../includes/clickok-md.md)]

## <a name="next-steps"></a>后续步骤

迁移过程的下一步是 [连接到 SQL Server 的实例](connecting-to-sql-server-oracletosql.md)。

## <a name="see-also"></a>另请参阅

[将 Oracle 数据库迁移到 SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)
