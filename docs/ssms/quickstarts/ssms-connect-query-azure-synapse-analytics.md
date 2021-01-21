---
title: 使用 SQL Server Management Studio (SSMS) 连接和查询 Azure Synapse Analytic 中的专用 SQL 池
description: 使用 SSMS 在 Azure Synapse Analytics 中连接专用 SQL 池。 通过在 SSMS 中运行基本 T-SQL 查询，在 Azure Synapse Analytics 中创建和查询专用 SQL 池。
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: e3584fe72ada1cff1d3b35324c5c5563bf97d050
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596853"
---
# <a name="quickstart-connect-and-query-a-dedicated-sql-pool-in-azure-synapse-analytics-with-sql-server-management-studio-ssms"></a>快速入门：使用 SQL Server Management Studio (SSMS) 连接和查询 Azure Synapse Analytic 中的专用 SQL 池

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

开始使用 SQL Server Management Studio (SSMS) 连接到 Azure Synapse Analytics 中的专用 SQL 池，并运行一些 Transact-SQL (T-SQL) 命令。

> [!div class="checklist"]
> - 在 Azure Synapse Analytics 中连接专用 SQL 池
> - 创建数据库
> - 在新数据库中创建表
> - 在新表中插入行
> - 查询新表并查看结果
> - 使用查询窗口表验证连接属性

## <a name="prerequisites"></a>先决条件

若要完成本文，需要 SQL Server Management Studio 以及对数据源的访问权限。

- 安装 [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md)。
- [Azure Synapse Analytics](https://azure.microsoft.com/services/synapse-analytics/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyDW39uBCUDMPmL693_KrmcNAV2uiMHZDeNJ-615AoxdIPBNqXwBDMhoCkL8QAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyDW39uBCUDMPmL693_KrmcNAV2uiMHZDeNJ-615AoxdIPBNqXwBDMhoCkL8QAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyDW39uBCUDMPmL693_KrmcNAV2uiMHZDeNJ-615AoxdIPBNqXwBDMhoCkL8QAvD_BwE)

## <a name="connect-to-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>在 Azure Synapse Analytics 中连接专用 SQL 池

[!INCLUDE[ssms-connect-azure-ad](../../includes/ssms-connect-azure-ad.md)]

1. 启动 SQL Server Management Studio。 首次运行 SSMS 时，系统将打开“连接到服务器”窗口。 如未打开，可以选择“对象资源管理器” > “连接” > “数据库引擎”，将其手动打开。

    :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connect-object-explorer.png" alt-text="对象资源管理器中的“连接”链接":::

2. 在“连接到服务器”窗口中，按照下表进行操作：

    |   设置   |   建议的值   |   说明   |
    |-------------|------------------------|-----------------|
    | **服务器类型** | 数据库引擎 | 对于“服务器类型”，选择“数据库引擎”（通常的默认选项）。 |
    | **服务器名称** | 完全限定的服务器名称 | 对于“服务器名称”，请输入 Azure Synapse Analytics 服务器名称。 |
    | **身份验证** | SQL Server 身份验证 | 使用 Azure Synapse Analytics 的 SQL Server 身份验证进行连接。 </br> </br> Azure SQL 不支持 Windows 身份验证方法。 有关详细信息，请参阅 [Azure SQL 身份验证](/azure/sql-database/sql-database-security-overview#access-management)。 |
    | **登录名** | 服务器帐户用户 ID | 用于创建服务器的服务器帐户的用户 ID。 |
    | **密码** | 服务器帐户密码 | 用于创建服务器的服务器帐户的密码。 |

    :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connect-to-azure-synapse-analytics-object-explorer.png" alt-text="Azure Synapse Analytics 的“服务器名称”字段":::

3. 完成所有字段后，选择“连接”。

    也可以通过选择“选项”来修改其他连接选项。 连接选项的示例包括你要连接到的数据库、连接超时值和网络协议。 本文对所有选项使用默认值。

    如果尚未设置防火墙设置，则将显示配置防火墙的提示。 登录后，填写 Azure 帐户登录信息并继续设置防火墙规则。 然后选择“确定”  。 此提示是一次性操作。 配置防火墙后，不应显示防火墙提示。

    :::image type="content" source="media/ssms-connect-query-azure-sql/azure-sql-firewall-sign-in-3.png" alt-text="Azure SQL 新建防火墙规则":::

4. 若要验证 Azure Synapse Analytics 连接是否成功，请展开并浏览“对象资源管理器”中显示服务器名称、SQL Server 版本和用户名的对象。 这些对象因服务器类型而异。

    :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connect-azure-synapse-analytics.png" alt-text="连接到 Azure Synapse Analytics 数据库":::

## <a name="troubleshoot-connectivity-issues"></a>解决连接问题

使用 Azure Synapse Analytics 时，可能会遇到连接问题。 有关排查连接问题的详细信息，请访问[排查连接问题](/azure/azure-sql/database/troubleshoot-common-errors-issues)。

你可以防止、排查、诊断和缓解在与 Azure SQL 数据库或 Azure SQL 托管实例交互时发生的连接错误和暂时性错误。 有关详细信息，请访问[排查暂时性连接错误](/azure/azure-sql/database/troubleshoot-common-connectivity-issues)。

## <a name="create-a-database"></a>创建数据库

现在，让我们按照以下步骤，创建一个名为 TutorialDB 的数据库：

1. 在“对象资源管理器”中右键单击服务器实例，然后选择“新建查询”：

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/new-query.png" alt-text="“新建查询”链接":::

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

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/execute.png" alt-text="“执行”命令":::
  
    查询完成后，新的 TutorialDB 数据库会显示在“对象资源管理器”内的数据库列表中。 如未显示，请右键单击“数据库”节点，然后选择“刷新”。

## <a name="create-a-table-in-the-new-database"></a>在新数据库中创建表

本部分中将在新创建的 TutorialDB 数据库中创建一个表。 由于查询编辑器仍处于 master 数据库的上下文中，因此请按以下步骤操作，将连接上下文切换到 TutorialDB 数据库：

1. 在数据库下拉列表中，选择所需数据库，如下所示：

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/change-db.png" alt-text="更改数据库":::

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

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/new-table.png" alt-text="新建表":::

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

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/query-results.png" alt-text="“结果”列表":::

    你还可以通过选择以下选项之一来修改结果的显示方式：

   ![用于显示查询结果的三个选项](media/ssms-connect-query-azure-synapse-analytics/results.png)

   - 第一个按钮将在“文本视图”中显示结果，如下一部分中的图像所示。
   - 中间的按钮采用“网格视图”显示结果，这是默认选项。
       - 此值设置为默认值
   - 第三个按钮可将结果保存为默认扩展名是 .rpt 的文件。

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>使用查询窗口表验证连接属性

在查询结果下，可以找到有关连接属性的信息。 在运行前一步骤中的上述查询后，查看查询窗口底部的连接属性。

- 可以确定连接到的服务器和数据库，以及使用的用户名。
- 此外，还可以查看查询持续时间和之前执行的查询所返回的行数。

   :::image type="content" source="media/ssms-connect-query-azure-synapse-analytics/connection-properties.png" alt-text="连接属性":::

## <a name="additional-tools"></a>其他工具

也可以使用 [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) 连接和查询 [SQL Server](../../azure-data-studio/quickstart-sql-server.md)、[Azure SQL 数据库](../../azure-data-studio/quickstart-sql-database.md)和 [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md)。

## <a name="next-steps"></a>后续步骤

熟悉 SSMS 的最好方式是进行实践演练。 这些文章可帮助你使用 SSMS 的各种功能。

- [SQL Server Management Studio (SSMS) 查询编辑器](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [脚本](../tutorials/scripting-ssms.md)
- [在 SSMS 中使用模板](../template/templates-ssms.md)
- [SSMS 配置](../tutorials/ssms-configuration.md)
- [使用 SSMS 的其他提示和技巧](../tutorials/ssms-tricks.md)