---
title: 使用 Azure Synapse Analytics 进行连接和查询
description: 本快速入门演示如何使用 Azure Data Studio 连接到 Azure Synapse Analytics 中的专用 SQL 池。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.reviewer: maghan, jrasnick
ms.custom: seodec18; seo-lt-2019
ms.date: 10/15/2020
ms.openlocfilehash: 8215e84c4c61935b3f5831ca771e2868e27be428
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091720"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-data-using-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>快速入门：使用 Azure Data Studio，在 Azure Synapse Analytics 中借助专用 SQL 池连接和查询数据

本快速入门演示如何使用 Azure Data Studio 连接到 Azure Synapse Analytics 中的专用 SQL 池。

## <a name="prerequisites"></a>先决条件
若要完成此快速入门，需要 Azure Data Studio 和 Azure Synapse Analytics 中的专用 SQL 池。

- [安装 Azure Data Studio](./download-azure-data-studio.md)。

如果还没有专用 SQL 池，请参阅[创建专用 SQL 池](/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision)。

请记下服务器名称和登录凭据！


## <a name="connect-to-your-dedicated-sql-pool"></a>连接到专用 SQL 池

使用 Azure Data Studio 建立与 Azure Synapse Analytics 服务器的连接。

1. 首次运行 Azure SQL Data，应会打开“连接”页。 如果无法看到“连接”页，请选择“SERVERS”边栏中的“添加连接”或“新建连接”图标：
   
   ![“连接”页面的屏幕截图，其中突出显示了“新建连接”图标。](media/quickstart-sql-dw/new-connection-icon.png)

2. 本文使用 SQL 登录名，但也支持 Windows 身份验证 。 使用 Azure SQL 服务器的服务器名称、用户名和密码，按如下所示填写字段：

   |   设置    | 建议的值 | 说明 |
   |--------------|-----------------|-------------| 
   | **服务器名称** | 完全限定的服务器名称 | 例如，名称应类似于：sqlpoolservername.database.windows.net。 |
   | **身份验证** | SQL 登录名| 在本教程中使用 SQL 身份验证。 |
   | **用户名** | 服务器管理员帐户 | 这是在创建服务器时指定的帐户。 |
   | **密码(SQL 登录名)** | 服务器管理员帐户的密码 | 这是在创建服务器时指定的密码。 |
   | **是否保存密码？** | 是或否 | 如果不想每次都输入密码，请选择“是”。 |
   | **数据库名称** | *留空* | 要连接到的数据库的名称。 |
   | **服务器组** | 选择 <Default> | 如果已创建服务器组，则可以设置为特定服务器组。 | 

3. 如果服务器没有可允许 Azure Data Studio 连接的防火墙规则，则会打开“新建防火墙规则”窗体。 填写窗体以新建防火墙规则。 有关详细信息，请参阅[防火墙规则](/azure/sql-database/sql-database-firewall-configure)。

4. 成功连接后，你的服务器将在“服务器”边栏中打开。

## <a name="create-a-database-in-your-dedicated-sql-pool"></a>在专用 SQL 池中创建数据库

1. 在对象资源管理器中右键单击服务器，选择“新建查询”。

2. 将以下代码片段粘贴到查询编辑器中，并选择“运行”：

   ```sql
    IF NOT EXISTS (
       SELECT name
       FROM sys.databases
       WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB] (EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
    GO  
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
   ```

## <a name="create-a-table"></a>创建表

查询编辑器仍连接到 master 数据库，但需要在 TutorialDB 数据库中创建一个表 。 

1. 将连接上下文更改为 TutorialDB：

2. 将以下代码片段粘贴到查询编辑器中，并选择“运行”：

   > [!NOTE]
   > 可在编辑器中将此片段追加到或覆盖之前的查询。 请注意，选择“运行”仅执行选定的查询。 如果未选择任何内容，选择“运行”将执行编辑器中所有的查询。

   ```sql
   -- Create a new table called 'Customers' in schema 'dbo'
   -- Drop the table if it already exists
   IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
   DROP TABLE dbo.Customers
   GO
   -- Create the table in the specified schema
   CREATE TABLE dbo.Customers
   (
      CustomerId        INT     NOT NULL,
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```

    :::image type="content" source="media/quickstart-sql-dw/create-table.png" alt-text="在 TutorialDB 数据库中创建表":::


## <a name="insert-rows"></a>插入行

1. 将以下代码片段粘贴到查询编辑器中，并选择“运行”：

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
      SELECT 1, N'Orlando',N'Australia', N'' UNION ALL
      SELECT 2, N'Keith', N'India', N'keith0@adventure-works.com' UNION ALL
      SELECT 3, N'Donna', N'Germany', N'donna0@adventure-works.com' UNION ALL
      SELECT 4, N'Janet', N'United States', N'janet1@adventure-works.com'
   ```

    :::image type="content" source="media/quickstart-sql-dw/create-rows.png" alt-text="在表中创建行":::

## <a name="view-the-result"></a>查看结果

1. 将以下代码片段粘贴到查询编辑器中，并选择“运行”：

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

2. 将显示查询结果：

    :::image type="content" source="media/quickstart-sql-dw/view-results.png" alt-text="查看结果":::


## <a name="clean-up-resources"></a>清理资源

如果不打算继续使用本文中创建的示例数据库，则[删除资源组](/azure/synapse-analytics/sql-data-warehouse/create-data-warehouse-portal#clean-up-resources)。

## <a name="next-steps"></a>后续步骤
有关详细信息，请访问[使用 Azure Data Studio连接到 Synapse SQL](/azure/synapse-analytics/sql/get-started-azure-data-studio)。

成功连接到 Azure Synapse Analytics 并运行查询后，请试用[代码编辑器教程](tutorial-sql-editor.md)。