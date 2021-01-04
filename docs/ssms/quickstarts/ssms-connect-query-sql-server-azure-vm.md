---
title: 使用 SQL Server Management Studio (SSMS) 连接和查询 Azure VM 上的 SQL Server 实例
description: 使用 SSMS 连接到 Azure VM 上的 SQL Server 实例。 通过在 SSMS 中运行基本 T-SQL 查询，创建和查询 Azure VM 上的 SQL Server 实例。
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: 29c39caf6885ee974c62ed153df982b435c72c95
ms.sourcegitcommit: 8a8c89b0ff6d6dfb8554b92187aca1bf0f8bcc07
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97619060"
---
# <a name="quickstart-connect-and-query-a-sql-server-instance-on-an-azure-virtual-machine-using-sql-server-management-studio-ssms"></a>快速入门：使用 SQL Server Management Studio (SSMS) 连接和查询 Azure 虚拟机上的 SQL Server 实例

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

开始使用 SQL Server Management Studio (SSMS) 连接到 Azure 虚拟机上的 SQL Server 实例，并运行一些 Transact-SQL (T-SQL) 命令。

> [!div class="checklist"]
> - 连接到 SQL Server 实例
> - 创建数据库
> - 在新数据库中创建表
> - 在新表中插入行
> - 查询新表并查看结果
> - 使用查询窗口表验证连接属性
## <a name="prerequisites"></a>先决条件

若要完成本文，需要 SQL Server Management Studio 以及对数据源的访问权限。

- 安装 [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md)。
- [Azure VM 上的 SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyP3g3mY45X8dbqEtIr8HASwSLUYRBrwwciZptYt0vUU4K7TuWnXTbxoCoPQQAvD_BwE)。

## <a name="connect-to-sql-virtual-machines"></a>连接到 SQL 虚拟机

以下步骤演示如何为 Azure VM 创建可选 DNS 标签，然后与 SQL Server Management Studio (SSMS) 进行连接。

### <a name="configure-a-dns-label-for-the-public-ip-address"></a>配置用于公共 IP 地址的 DNS 标签

若要从 Internet 连接到 SQL Server 数据库引擎，请考虑创建用于公共 IP 地址的 DNS 标签。 可以通过 IP 地址进行联接，但使用 DNS 标签可以创建更容易标识的 A 记录，并可抽象基础性公共 IP 地址。

> [!NOTE]
> 如果打算只连接到同一虚拟网络中的 SQL Server 实例，或者只进行本地连接，则 DNS 标签不是必需的。

1. 通过在门户中选择“虚拟机”来创建 DNS 标签。 选择要显示其属性的 SQL Server VM。

2. 在虚拟机概览中，选择“公共 IP 地址”。

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-ip-address-.png" alt-text="公共 IP 地址":::

3. 在公共 IP 地址的属性中，展开“配置” 。

4. 输入 DNS 标签名称。 此名称是一种可通过名称而非 IP 地址直接连接到 SQL Server VM 的 A 记录。

5. 选择“保存”按钮。

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/azure-sql-vm-dns-label.png" alt-text="DNS 标签":::

### <a name="connect"></a>连接

1. 启动 SQL Server Management Studio。 首次运行 SSMS 时，系统将打开“连接到服务器”窗口。 如未打开，可以选择“对象资源管理器” > “连接” > “数据库引擎”，将其手动打开。

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-object-explorer.png" alt-text="对象资源管理器中的“连接”链接":::

2. 此时会显示“连接到服务器”对话框。 输入以下信息：

    |   设置   |   建议的值   |   说明   |
    |--------------|-----------------------|-----------------|
    | **服务器类型** | 数据库引擎 | 对于“服务器类型”，选择“数据库引擎”（通常的默认选项）。 |
    | **服务器名称** | 完全限定的服务器名称 | 对于“服务器名称”，输入 Azure SQL VM 的名称。 你还可以使用 Azure SQL VM IP 地址进行连接。 | 
    | **身份验证** | SQL Server 身份验证 | 使用 Azure SQL VM 的 SQL Server 身份验证进行连接。 此外，如果已设置 Azure Active Directory 环境，则可使用任何 Azure Active Directory 选项。 </br> </br> Azure SQL VM 不支持 Windows 身份验证方法。 有关详细信息，请参阅 [Azure SQL 身份验证](/azure/sql-database/sql-database-security-overview#access-management)。|
    | **登录名** | 服务器帐户用户 ID | 用于创建服务器的服务器帐户的用户 ID。 使用 SQL Server 身份验证时需要登录名。 |
    | **密码** | 服务器帐户密码 | 用于创建服务器的服务器帐户的密码。 使用 SQL Server 身份验证时需要密码。 |

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-to-azure-sql-vm-object-explorer.png" alt-text="SQL 虚拟机的“服务器名称”字段":::

3. 完成所有字段后，选择“连接”。

    也可以通过选择“选项”来修改其他连接选项。 连接选项的示例包括你要连接到的数据库、连接超时值和网络协议。 本文对所有选项使用默认值。

4. 若要验证 Azure VM 上的 SQL Server 是否成功，请展开并浏览“对象资源管理器”中显示服务器名称、SQL Server 版本和用户名的对象。 这些对象因服务器类型而异。

    :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connect-azure-sql-vm.png" alt-text="Azure SQL VM 连接":::

## <a name="troubleshoot-connectivity-issues"></a>解决连接问题

尽管门户提供有自动配置连接的选项，但了解如何手动配置连接也非常重要。 了解相关要求也有助于进行故障排除。

下表列出了连接到 Azure VM 上的 SQL Server 的要求。

| 要求 | 说明 |
|---|---|
| [启用 SQL Server 身份验证模式](/sql/database-engine/configure-windows/change-server-authentication-mode#use-ssms) | 除非已在虚拟网络上配置 Active Directory，否则需要进行 SQL Server 身份验证才能连接到远程 VM。 |
| [创建 SQL 登录名](/sql/relational-databases/security/authentication-access/create-a-login) | 如果使用 SQL 身份验证，则需要提供带有用户名和密码的 SQL 登录名，并且该登录名还有权访问目标数据库。 |
| 启用 TCP/IP 协议 | SQL Server 必须允许通过 TCP 连接。 |
| [启用 SQL Server 端口的防火墙规则](/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) | VM 上的防火墙必须允许 SQL Server 端口（默认为 1433）上的入站流量。 |
| [创建 TCP 1433 的网络安全组规则](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#create-a-security-rule) | 如果希望通过 Internet 连接，请允许 VM 接收 SQL Server 端口（默认为 1433）上的流量。 本地连接和仅虚拟网路连接对此无要求。 仅在 Azure 门户中需要此步骤。 |

> [!TIP]
> 在门户中配置连接时，已为你完成上表中的步骤。 只需使用这些步骤来确认配置或手动为 SQL Server 设置连接。

## <a name="create-a-database"></a>创建数据库

按照以下步骤，创建一个名为 TutorialDB 的数据库：

1. 在“对象资源管理器”中右键单击服务器实例，然后选择“新建查询”：

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-query.png" alt-text="“新建查询”链接":::

2. 将以下 T-SQL 代码片段粘贴到查询窗口：

    ```sql
    IF NOT EXISTS (
    SELECT name
    FROM sys.databases
    WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB]
    GO
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
    ```

3. 通过选择“执行”或选择键盘上的 F5 来执行查询。

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/execute.png" alt-text="“执行”命令":::
  
    查询完成后，新的 TutorialDB 数据库会显示在“对象资源管理器”内的数据库列表中。 如未显示，请右键单击“数据库”节点，然后选择“刷新”。

## <a name="create-a-table-in-the-new-database"></a>在新数据库中创建表

本部分中将在新创建的 TutorialDB 数据库中创建一个表。 由于查询编辑器仍处于 master 数据库的上下文中，因此请按以下步骤操作，将连接上下文切换到 TutorialDB 数据库：

1. 在数据库下拉列表中，选择所需数据库，如下所示：

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/change-db.png" alt-text="更改数据库":::

2. 将以下 T-SQL 代码片段粘贴到查询窗口：

    ```sql
    USE [TutorialDB]
    -- Create a new table called 'Customers' in schema 'dbo'
    -- Drop the table if it already exists
    IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
    DROP TABLE dbo.Customers
    GO
    -- Create the table in the specified schema
    CREATE TABLE dbo.Customers
    (
       CustomerId        INT    NOT NULL   PRIMARY KEY, -- primary key column
       Name      [NVARCHAR](50)  NOT NULL,
       Location  [NVARCHAR](50)  NOT NULL,
       Email     [NVARCHAR](50)  NOT NULL
    );
    GO
    ```

3. 通过选择“执行”或选择键盘上的 F5 来执行查询。

查询完成后，新的“客户”表会显示在对象资源管理器内的表列表中。 如果表未显示，请右键单击“对象资源管理器”中的“TutorialDB” > “表”节点，并选择“刷新”。

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/new-table.png" alt-text="新建表":::

## <a name="insert-rows-into-the-new-table"></a>将行插入新表

现在，让我们将一些行插入前面创建的 Customers 表。 将以下 T-SQL 代码片段粘贴到查询窗口并选择“执行”：

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
   VALUES
      ( 1, N'Orlando', N'Australia', N''),
      ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
      ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
      ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
   GO
   ```

## <a name="query-the-table-and-view-the-results"></a>查询表并查看结果

查询结果在查询文本窗口下可见。 要查询 Customers 表并查看插入的行，请按照以下步骤操作：

1. 将以下 T-SQL 代码片段粘贴到查询窗口并选择“执行”：

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

    查询结果显示在文本输入区域下。

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/query-results.png" alt-text="“结果”列表":::

    你还可以通过选择以下选项之一来修改结果的显示方式：

   ![用于显示查询结果的三个选项](media/ssms-connect-query-sql-server-azure-vm/results.png)

   - 第一个按钮将在“文本视图”中显示结果，如下一部分中的图像所示。
   - 中间的按钮采用“网格视图”显示结果，这是默认选项。
       - 此值设置为默认值
   - 第三个按钮可将结果保存为默认扩展名是 .rpt 的文件。

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>使用查询窗口表验证连接属性

在查询结果下，可以找到有关连接属性的信息。 在运行前一步骤中的上述查询后，查看查询窗口底部的连接属性。

- 可以确定连接到的服务器和数据库，以及使用的用户名。
- 此外，还可以查看查询持续时间和之前执行的查询所返回的行数。

   :::image type="content" source="media/ssms-connect-query-sql-server-azure-vm/connection-properties.png" alt-text="连接属性":::

## <a name="additional-tools"></a>其他工具

也可以使用 [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) 连接和查询 [SQL Server](../../azure-data-studio/quickstart-sql-server.md)、[Azure SQL 数据库](../../azure-data-studio/quickstart-sql-database.md)和 [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md)。

## <a name="next-steps"></a>后续步骤

熟悉 SSMS 的最好方式是进行实践演练。 这些文章可帮助你使用 SSMS 的各种功能。

- [SQL Server Management Studio (SSMS) 查询编辑器](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [脚本](../tutorials/scripting-ssms.md)
- [在 SSMS 中使用模板](../template/templates-ssms.md)
- [SSMS 配置](../tutorials/ssms-configuration.md)
- [使用 SSMS 的其他提示和技巧](../tutorials/ssms-tricks.md)