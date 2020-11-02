---
title: 连接并查询 Azure SQL 数据库
description: 学习本快速入门，了解如何使用 Azure Data Studio 连接到 Azure SQL 数据库服务器，然后创建和查询数据库。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.reviewer: alayu; maghan; sstein
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.custom: seodec18; sqlfreshmay19; seo-lt-2019
ms.date: 05/14/2019
ms.openlocfilehash: 7eb89be3b94565f7a8642dad893642176a22822b
ms.sourcegitcommit: fb8724fb99c46ecf3a6d7b02a743af9b590402f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92439291"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-azure-sql-database"></a>快速入门：使用 Azure Data Studio 连接和查询 Azure SQL 数据库

在本快速入门中，你将使用 Azure Data Studio 连接到 Azure SQL 数据库服务器。 随后，你将运行 Transact-SQL (T-SQL) 语句来创建和查询 TutorialDB 数据库，该数据库亦用于其他 Azure Data Studio 教程。

## <a name="prerequisites"></a>先决条件

若要完成此快速入门，需要 Azure Data Studio 和 Azure SQL 数据库服务器。

- [安装 Azure Data Studio](./download-azure-data-studio.md?view=sql-server-ver15)

如果你没有 Azure SQL Server，请完成以下 Azure SQL 数据库快速入门之一。 请记住完全限定服务器名称和登录凭据以用于后续步骤：

- [创建 DB - 门户](/azure/sql-database/sql-database-get-started-portal)
- [创建 DB - CLI](/azure/sql-database/sql-database-get-started-cli)
- [创建 DB - PowerShell](/azure/sql-database/sql-database-get-started-powershell)


## <a name="connect-to-your-azure-sql-database-server"></a>连接到 Azure SQL 数据库服务器

使用 Azure Data Studio 建立与 Azure SQL 数据库服务器的连接。

1. 首次运行 Azure Data Studio 时，应该会打开“欢迎”页。 如果没有看到“欢迎”页，请选择“帮助” > “欢迎”  。 选择“新建连接”以打开“连接”窗格 ：
   
   ![显示“欢迎使用 Azure Delta Studio”对话框的屏幕截图，其中突出显示了“下一个连接”选项。](media/quickstart-sql-database/new-connection-icon.png)

2. 本文使用 SQL 登录名，但也支持 Windows 身份验证。 使用 Azure SQL Server 的服务器名称、用户名和密码填写以下字段：

   | 设置       | 建议的值 | 说明 |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **服务器名称** | 完全限定的服务器名称 | 类似于 **servername.database.windows.net** 。 |
   | **身份验证** | SQL 登录名| 本教程使用 SQL 身份验证。 |
   | **用户名** | 服务器管理员帐户用户名 | 用于创建服务器的帐户的用户名。 |
   | **密码(SQL 登录名)** | 服务器管理员帐户密码 | 用于创建服务器的帐户的密码。 |
   | **是否保存密码？** | 是或否 | 如果不想每次都输入密码，请选择“是”。 |
   | **数据库名称** | *留空* | 仅连接到此处的服务器。 |
   | **服务器组** | 选择 <Default> | 可将此字段设置为创建的特定服务器组。 | 

   ![“Azure Data Studio - 连接”页面的屏幕截图。](media/quickstart-sql-database/new-connection-screen.png)  

3. 选择“连接”。

4. 如果服务器没有可允许 Azure Data Studio 连接的防火墙规则，则会打开“新建防火墙规则”窗体。 填写窗体以新建防火墙规则。 有关详细信息，请参阅[防火墙规则](/azure/sql-database/sql-database-firewall-configure)。

   ![新建防火墙规则](media/quickstart-sql-database/firewall.png)  

成功连接后，你的服务器将在“服务器”侧栏中打开。

## <a name="create-the-tutorial-database"></a>创建教程数据库

接下来的部分会创建在其他 Azure Data Studio 教程中使用的 TutorialDB 数据库。

1. 右键单击“服务器”侧栏中的 Azure SQL Server，然后选择“新建查询” 。

1. 将此 SQL 粘贴到查询编辑器中。

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

1. 在工具栏中选择“运行”。 通知在显示查询进度的“消息”窗格中显示。

## <a name="create-a-table"></a>创建表

查询编辑器连接到 **master** 数据库，但我们想要在 **TutorialDB** 数据库中创建一个表。 

1. 连接到 **TutorialDB** 数据库。

   ![更改上下文](media/quickstart-sql-database/change-context2.png)



1. 创建 `Customers` 表。 

   将查询编辑器中的上一个查询替换为此查询，然后选择“运行”。

   ```sql
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


## <a name="insert-rows-into-the-table"></a>在表中插入行

将上一个查询替换为此查询，然后选择“运行”。

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

## <a name="view-the-result"></a>查看结果

将上一个查询替换为此查询，然后选择“运行”。

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

查询结果显示：

   ![选择结果](media/quickstart-sql-database/select-results2.png)


## <a name="clean-up-resources"></a>清理资源

后续的快速入门文章以此处创建的资源为基础。 如果计划学习这些文章，请确保不要删除这些资源。 否则，在 Azure 门户中删除不再需要的资源。 有关详细信息，请参阅[清理资源](/azure/sql-database/sql-database-get-started-portal#clean-up-resources)。

## <a name="next-steps"></a>后续步骤

你现在已成功连接到 Azure SQL 数据库并运行查询，接下来请尝试[代码编辑器教程](tutorial-sql-editor.md)。