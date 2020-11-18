---
title: " (AccessToSQL) 连接到 Azure SQL 数据库 |Microsoft Docs"
description: 了解如何连接到 Azure SQL 数据库的目标实例以迁移 Access 数据库。 SSMA 获取有关 Azure SQL 数据库中的数据库的元数据。
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- instance of SQL Azure
- metadata, refreshing
- refreshing metadata
- SQL Azure
- SQL Azure, connecting
- SQL Azure, connecting to
- SQL Azure, reconnecting
- SQL Azure, synchronizing metadata
ms.assetid: 1ba0d113-dc05-4431-8689-e14a8821bafd
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f910e9c07bf4318419714b97e4f4db742c913753
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94869435"
---
# <a name="connecting-to-azure-sql-database-accesstosql"></a>连接到 Azure SQL 数据库 (AccessToSQL) 

若要将 Access 数据库迁移到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ，你必须连接到的目标实例 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。 当你连接时，SSMA 将获取实例中所有数据库的元数据， [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 并在 **Azure SQL 数据库元数据资源管理器** 中显示数据库元数据。 SSMA 存储有关您连接到的实例的信息 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ，但不存储密码。

你的连接将 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 保持活动状态，直到你关闭项目。 重新打开项目时， [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 如果想要与服务器建立活动连接，则必须重新连接到。 在将数据库对象加载到和迁移数据之前，可以脱机工作 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。

有关实例的元数据 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 不会自动同步。 相反，若要更新 **AZURE SQL 数据库元数据资源管理器** 中的元数据，必须手动更新 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 元数据。 有关详细信息，请参阅本主题后面的 "同步 Azure SQL 数据库元数据" 一节。

## <a name="required-azure-sql-database-permissions"></a>必需的 Azure SQL 数据库权限

用于连接到的帐户 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 需要不同的权限，具体取决于帐户执行的操作：

- 若要将 Access 对象转换为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法、更新中的元数据 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 或将转换后的语法保存到脚本中，该帐户必须有权登录到的实例 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。

- 若要将数据库对象加载到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 中，该帐户必须是 **db_ddladmin** 数据库角色的成员。

- 若要将数据迁移到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ，该帐户必须是 **db_owner** 数据库角色的成员。

## <a name="establishing-an-azure-sql-database-connection"></a>建立 Azure SQL Database 连接

在将 Access 数据库对象转换为 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 语法之前，必须与 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 要将 access 数据库迁移到的实例建立连接。

定义连接属性时，还可以指定要将对象和数据迁移到的数据库。 在连接到之后，可以在访问架构级别自定义此映射 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。 有关详细信息，请参阅 [将 Access 数据库映射到 SQL Server 架构](mapping-source-and-target-databases-accesstosql.md)。
  
> [!IMPORTANT]
> 尝试连接到之前，请 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 确保你的 IP 地址是通过防火墙允许的 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。
  
若要连接到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)]，请执行以下操作：

1. 在 " **文件** " 菜单上，选择 " **连接到 SQL Azure** (在创建项目) 后启用此选项。
   如果以前连接到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ，则命令名称将 **重新连接到 SQL Azure**。

2. 在 "连接" 对话框中，输入或选择的服务器名称 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。

3. 输入、选择或 **浏览** 数据库名称。

4. 输入或选择 " **用户名**"。

5. 输入 **密码**。

6. SSMA 建议将加密连接到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。

7. 单击“连接”。
  
如果中没有数据库 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ，可以使用单击 "**浏览**" 按钮上显示的 "**创建 Azure 数据库**" 选项来创建第一个数据库。

## <a name="synchronizing-azure-sql-database-metadata"></a>同步 Azure SQL 数据库元数据

中数据库的元数据 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 不会自动更新。 **AZURE SQL 数据库元数据资源管理器** 中的元数据是首次连接到时 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 或上次手动更新元数据时的元数据的快照。 您可以为所有数据库或任何单个数据库或数据库对象手动更新元数据。 同步元数据：

1. 确保你已连接到 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。

2. 在 **AZURE SQL 数据库 "元数据资源管理器**" 中，选中要更新的数据库或数据库架构旁边的复选框。
   例如，若要更新所有数据库的元数据，请选中 " **数据库**" 旁边的复选框。

3. 右键单击 " **数据库**"、"数据库" 或 "数据库架构"，然后选择 " **与数据库同步**"。

## <a name="refreshing-azure-sql-database-metadata"></a>刷新 Azure SQL 数据库元数据

如果在 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 连接后架构发生更改，则可以刷新服务器的元数据。

刷新 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 元数据：

- 在 **AZURE SQL 数据库元数据资源管理器** 中，右键单击 " **数据库**"，然后选择 " **从数据库刷新**"。

## <a name="reconnecting-to-azure-sql-database"></a>重新连接到 Azure SQL 数据库

你的连接将 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 保持活动状态，直到你关闭项目。 重新打开项目时， [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 如果想要与服务器建立活动连接，则必须重新连接到。 在将数据库对象加载到和迁移数据之前，可以脱机工作 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。

重新连接到的过程与 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 建立连接的过程相同。

## <a name="next-steps"></a>后续步骤

迁移的下一步取决于你的项目需求：

- 若要自定义访问架构与 Azure SQL 数据库之间的映射，请参阅 [将访问数据库映射到 SQL Server 架构](mapping-source-and-target-databases-accesstosql.md)。
- 若要自定义项目的配置选项，请参阅 [设置项目选项](setting-conversion-and-migration-options-accesstosql.md)。
- 若要自定义源和目标数据类型的映射，请参阅 [映射源和目标数据类型](mapping-source-and-target-data-types-accesstosql.md)。
- 如果不需要执行这些任务中的任何一种，可以将 Access 数据库对象定义转换为 SQL Azure 对象定义。 有关详细信息，请参阅 [转换 Access 数据库](converting-access-database-objects-accesstosql.md)。

## <a name="see-also"></a>另请参阅

[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)
