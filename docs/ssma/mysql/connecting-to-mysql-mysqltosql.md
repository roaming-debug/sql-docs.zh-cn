---
title: 正在连接到 MySQL (MySQLToSQL) |Microsoft Docs
description: 了解如何连接到目标 iMySQL 数据库以迁移 MySQL 数据库。 SSMA 获取有关 Azure SQL 数据库中的数据库的元数据。
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to MySQL, MySQL permission
- Connecting to MySQL,reconnecting
ms.assetid: 084c7020-f729-4f91-90e0-143f85fa68d1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d7f3f46a58fccafa68164d9335c66df10c5b7487
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068892"
---
# <a name="connecting-to-mysql-mysqltosql"></a>连接到 MySQL (MySQLToSQL)
若要将 MySQL 数据库迁移到 SQL Server 或 SQL Azure，你必须连接到要迁移的 MySQL 数据库。 在连接时，SSMA 将获取有关所有 MySQL 架构的元数据，然后在 "MySQL 元数据资源管理器" 窗格中显示该架构。 SSMA 存储有关数据库服务器的信息，但不存储密码。  
  
在关闭项目之前，与数据库的连接保持活动状态。 重新打开项目时，如果要连接到数据库，则必须重新连接。  
  
不会自动更新有关 MySQL 数据库的元数据。 相反，如果要更新 MySQL 元数据资源管理器中的元数据，则必须手动更新。 有关详细信息，请参阅本主题后面的 "刷新 MySQL 元数据" 一节。  
  
## <a name="required-mysql-permissions"></a>必需的 MySQL 权限  
用于连接到 MySQL 数据库的帐户至少必须具有 **connect** 权限。 这使 SSMA 可以从连接用户所拥有的架构中获取元数据。 若要获取其他架构中的对象的元数据，然后转换这些架构中的对象，该帐户必须具有以下权限：  
  
-   对数据库对象的 "显示" 权限  
  
-   "Information_schema" 上的 "选择" 特权  
  
-   针对 Udf 的 mysql (上的 "选择" 特权)   
  
## <a name="establishing-a-connection-to-mysql"></a>与 MySQL 建立连接  
连接到数据库时，SSMA 将读取数据库元数据，然后将此元数据添加到项目文件。 当 SSMA 将对象转换为 SQL Server 或 SQL Azure 语法，并在将数据迁移到 SQL Server 或 SQL Azure 时，此元数据由使用。 可以在 MySQL 元数据资源管理器窗格中浏览此元数据，并查看单个数据库对象的属性。  
  
> [!IMPORTANT]  
> 尝试连接之前，请确保数据库服务器正在运行，并且可以接受连接。  
  
**连接到 MySQL**  
  
1.  在 " **文件** " 菜单上，选择 " **连接到 MySQL** (在创建项目) 后将启用此选项。  
  
    如果以前已连接到 MySQL，则命令名称将 **重新连接到 mysql**。  
  
2.  在 " **提供程序** " 框中，选择 "MySQL ODBC 5.1 驱动程序 (信任的) "。 它是 "标准" 模式下的默认提供程序。  
  
3.  在 " **模式** " 框中，选择 " **标准模式**"。 它是默认模式。  
  
    使用 "标准" 模式指定服务器名称和端口。  
  
4.  在 " **标准" 模式** 下，提供下列值：  
  
    1.  在 " **服务器名称** " 框中，输入 MySQL 服务器名称。 在 " **服务器端口** " 框中，输入要为3306的端口号。 它是默认端口。  
  
    2.  在 " **用户名** " 框中，输入具有所需权限的 MySQL 帐户。  
  
    3.  在 " **密码** " 框中，输入指定用户名的密码。  
  
5.  **SSL：** 如果希望安全地连接到 MySQL，请通过选中 " **ssl** " 复选框， (ssl) 使用安全套接字层。  
  
6.  **配置：** 它提供了一个选项，用于通过安全套接字层 (SSL) 配置与 MySQL 的连接。  
  
    > [!NOTE]  
    > 若要启用 **配置**，SSL 必须设置为 **True**。  
  
    单击 "配置" 按钮时，会显示一个对话框。 若要在连接到 MySQL 数据库时使用加密，必须定义该对话框中提供的以下三个证书文件的路径 [隐私增强邮件证书 (PEM) ]：  
  
    -   **SSL 证书颁发机构：** 指定带有可信 SSL Ca 列表的文件的路径。  
  
    -   **SSL 证书：** 指定用于建立安全连接的 SSL 证书文件的名称。  
  
    -   **SSL 密钥：** 指定用于建立安全连接的 SSL 密钥文件的名称。  
  
    > [!NOTE]  
    > -   如果提供了所需的信息，则 " **确定** " 按钮处于启用状态。 如果任何文件路径无效，则 "确定" 按钮将保持禁用状态。  
    > -   " **取消** " 按钮关闭对话框，并从主连接窗体 **禁用** SSL 选项。  
  
7.  有关详细信息，请参阅 [连接到 MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connect-to-mysql-mysqltosql.md)  
  
## <a name="reconnecting-to-mysql"></a>重新连接到 MySQL  
与数据库服务器的连接会一直保持活动状态，直到关闭该项目。 重新打开项目时，如果要连接到数据库，则必须重新连接。 你可以脱机工作，直至你要更新元数据、将数据库对象加载到 SQL Server 或 SQL Azure，以及迁移数据。  
  
## <a name="refreshing-mysql-metadata"></a>刷新 MySQL 元数据  
不会自动刷新有关 MySQL 数据库的元数据。 MySQL 元数据资源管理器中的元数据是首次连接时或上次手动刷新元数据时的元数据的快照。 可以手动更新所有架构、单个架构或单个数据库对象的元数据。  
  
**刷新元数据**  
  
1.  请确保已连接到数据库。  
  
2.  在 MySQL 元数据资源管理器中，选中要更新的每个架构或数据库对象旁边的复选框。  
  
3.  右键单击 " **架构**"、单个架构或数据库对象，然后选择 " **从数据库刷新**"。  
  
    如果没有活动连接，SSMA 将显示 " **连接到 MySQL** " 对话框，以便你可以连接。  
  
4.  在 "从数据库刷新" 对话框中，指定要刷新的对象。  
  
    -   若要刷新对象，请单击对象旁边的 **活动** 字段，直到出现箭头。  
  
    -   若要防止对象被刷新，请单击对象旁边的 **活动** 字段，直到出现 **X** 。  
  
    -   若要刷新或拒绝某个对象类别，请单击 "category" 文件夹旁边的 **活动** 字段。  
  
    -   若要查看颜色编码的定义，请单击 " **图例** " 按钮。  
  
5.  单击“确定”。  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是 [连接到 SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
## <a name="see-also"></a>另请参阅  
[将 MySQL 数据库迁移到 SQL Server-Azure SQL 数据库 &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
