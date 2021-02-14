---
description: '连接到 SQL Server (DB2ToSQL) '
title: 正在连接到 SQL Server (DB2ToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: b59803cb-3cc6-41cc-8553-faf90851410e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1109bd083476bf9be2421b06637751c34fc6405c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081628"
---
# <a name="connecting-to-sql-server-db2tosql"></a>连接到 SQL Server (DB2ToSQL) 

若要将 DB2 数据库迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，你必须连接到的目标实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 在连接时，SSMA 将获取实例中所有数据库的元数据， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并在 **SQL Server 元数据资源管理器** 中显示数据库元数据。 SSMA 存储有关您连接到的实例的信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，但不存储密码。

你的连接将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 保持活动状态，直到你关闭项目。 重新打开项目时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如果想要与服务器建立活动连接，则必须重新连接到。 在将数据库对象加载到和迁移数据之前，可以脱机工作 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

有关实例的元数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会自动同步。 相反，若要更新 **SQL Server 元数据资源管理器** 中的元数据，必须手动更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 元数据。 有关详细信息，请参阅本主题后面的 "同步 SQL Server 元数据" 一节。

## <a name="required-sql-server-permissions"></a>必需的 SQL Server 权限

用于连接到的帐户 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 需要不同的权限，具体取决于帐户执行的操作：

- 若要将 DB2 对象转换为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法、更新中的元数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或将转换后的语法保存到脚本中，该帐户必须有权登录到的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

- 若要将数据库对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，该帐户必须是 **db_ddladmin** 服务器角色的成员。

- 若要将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，该帐户必须是 **db_owner** 数据库角色的成员。

- 若要运行 SSMA 生成的代码，该帐户必须 `EXECUTE` 对目标数据库的 **ssma_db2** 架构中的所有用户定义函数都具有权限。 这些函数提供 DB2 系统函数的等效功能，由转换后的对象使用。

## <a name="establishing-a-sql-server-connection"></a>建立 SQL Server 连接

在将 DB2 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语法之前，必须建立与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 要迁移 DB2 数据库的实例的连接。

定义连接属性时，还可以指定要将对象和数据迁移到的数据库。 在连接到之后，你可以在 DB2 架构级别自定义此映射 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关详细信息，请参阅 [将 DB2 架构映射到 SQL Server 架构 &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-schemas-to-sql-server-schemas-db2tosql.md)。

> [!IMPORTANT]
> 尝试连接到之前，请 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 确保的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 正在运行，并且可以接受连接。  
  
若要连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ：

1. 在 " **文件** " 菜单上，选择 " **连接到 SQL Server**"。
   如果以前连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则命令名称将 **重新连接到 SQL Server**。

2. 在 "连接" 对话框中，输入或选择实例的名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。
   - 如果要连接到本地计算机上的默认实例，则可以输入 `localhost` 或 `.`)  (。
   - 如果要连接到另一台计算机上的默认实例，请输入计算机的名称。
   - 如果要连接到另一台计算机上的命名实例，请输入计算机名称后跟反斜杠和实例名称，如 `MyServer\MyInstance` 。

3. 如果实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置为接受非默认端口上的连接，请 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 " **服务器端口** " 框中输入用于连接的端口号。 对于的默认实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，默认端口号为1433。 对于命名实例，SSMA 将尝试从 Browser 服务获取端口号 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

4. 在 " **数据库** " 框中，输入目标数据库的名称。
   当你重新连接到时，此选项不可用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

5. 在 " **身份验证** " 框中，选择要用于连接的身份验证类型。 若要使用当前的 Windows 帐户，请选择 " **Windows 身份验证**"。 若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，请选择 " **SQL Server 身份验证**"，然后提供登录名和密码。

6. 对于安全连接，将添加两个控件，即 " **加密连接** " 和 " **TrustServerCertificate** " 复选框。 只有在选中 " **加密连接** " 时，" **TrustServerCertificate** " 复选框才可见。 选中 " **加密连接** " (true) 并取消选中 " **TrustServerCertificate** " (false) ，它将验证 SQL Server SSL 证书。 验证服务器证书是 SSL 握手过程的一部分，这可确保服务器是要连接到的正确服务器。 为了确保这一点，证书必须安装在客户端和服务器端。

7. 单击“连接”  。

> [!IMPORTANT]
> 虽然你可以连接到的更高版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，但与创建迁移项目时选择的版本相比，数据库对象的转换由项目的目标版本而不是你连接到的的版本确定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

## <a name="synchronizing-sql-server-metadata"></a>同步 SQL Server 元数据

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]与数据库有关的元数据不会自动更新。 **SQL Server 元数据资源管理器** 中的元数据是首次连接到时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或上次手动更新元数据时的元数据的快照。 您可以为所有数据库或任何单个数据库或数据库对象手动更新元数据。 同步元数据：

1. 确保你已连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。
  
2. 在 **SQL Server 元数据资源管理器**"中，选中要更新的数据库或数据库架构旁边的复选框。
   例如，若要更新所有数据库的元数据，请选中 " **数据库**" 旁边的复选框。

3. 右键单击 " **数据库**"、"数据库" 或 "数据库架构"，然后选择 " **与数据库同步**"。

## <a name="next-step"></a>下一步

迁移的下一步取决于你的项目需求：

- 若要自定义 DB2 架构和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库与架构之间的映射，请参阅 [将 Db2 架构映射到 SQL Server 架构 &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-schemas-to-sql-server-schemas-db2tosql.md)。
- 若要自定义项目的配置选项，请参阅 DB2ToSQL&#41;和相关部分 [&#40;转换&#41; &#40;项目设置 ](../../ssma/db2/project-settings-conversion-db2tosql.md) 。
- 若要自定义源和目标数据类型的映射，请参阅 [映射 DB2 和 SQL Server 数据类型 &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md)。
- 如果不需要执行这些任务中的任何一种，可以将 DB2 数据库对象定义转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象定义。 有关详细信息，请参阅将 [DB2 架构转换 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md)。

## <a name="see-also"></a>另请参阅

[将 DB2 数据库迁移到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)
