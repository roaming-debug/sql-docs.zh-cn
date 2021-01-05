---
title: 快速入门：连接并查询 PostgreSQL
description: 学习本快速入门，了解如何使用 Azure Data Studio 连接到 PostgreSQL，然后使用 SQL 语句创建并查询数据库。
ms.custom: seodec18
ms.date: 09/18/2019
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.reviewer: alayu, maghan, sstein
ms.topic: quickstart
author: sunilagarwal
ms.author: sunila
ms.openlocfilehash: 389c8b745a3768f63e2f702a7f48e8c110262fb7
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637704"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-postgresql"></a>快速入门：使用 Azure Data Studio 连接并查询 PostgreSQL

本快速入门介绍如何使用 Azure Data Studio 连接到 PostgreSQL，然后使用 SQL 语句创建数据库 tutorialdb 并对其进行查询。

## <a name="prerequisites"></a>先决条件

若要完成本快速入门，需要 Azure Data Studio、Azure Data Studio 的 PostgreSQL 扩展以及对 PostgreSQL 服务器的访问权限。

- [安装 Azure Data Studio](./download-azure-data-studio.md)。
- [安装 Azure Data Studio 的 PostgreSQL 扩展](./extensions/postgres-extension.md)。
- [安装 PostgreSQL](https://www.postgresql.org/download/)。 （或者，可以使用 [az postgres up](/azure/postgresql/quickstart-create-server-up-azure-cli) 在云中创建 Postgres 数据库）。 

## <a name="connect-to-postgresql"></a>连接到 PostgreSQL

1. 启动“Azure Data Studio”。

2. 第一次启动 Azure Data Studio 时，将打开“连接”对话框。 如果未打开“连接”对话框，请单击“服务器”页中的“新建连接”图标  ：

   ![“新建连接”图标](media/quickstart-postgresql/new-connection-icon.png)

3. 在弹出的窗体中，转到“连接类型”，然后从下拉列表中选择“PostgreSQL” 。


4. 使用 PostgreSQL 服务器的服务器名称、用户名和密码填写其余字段。 

   ![新建连接屏幕](media/quickstart-postgresql/new-connection-screen.png)  

   | 设置       | 示例值 | 说明 |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **服务器名称** | localhost | 完全限定的服务器名称 |
   | **用户名** | postgres | 要用来登录的用户名。 |
   | **密码(SQL 登录名)** | password | 用于登录的帐户的密码。 |
   | **密码** | *检查* | 如果不想每次连接时都输入密码，请选中此框。 |
   | **数据库名称** | \<Default\> | 如果希望连接指定数据库，请填写此项。 |
   | **服务器组** | \<Default\> | 使用此选项可以将此连接分配给你创建的特定服务器组。 | 
   | **名称(可选)** | *留空* | 使用此选项可以为服务器指定一个易记名称。 | 

5. 选择“连接”。 

成功连接后，你的服务器将在“服务器”侧栏中打开。


## <a name="create-a-database"></a>创建数据库

以下步骤会创建一个名为“tutorialdb”的数据库：

1. 右键单击“服务器”侧栏中的 PostgreSQL 服务器，选择“新建查询” 。

2. 将此 SQL 语句粘贴到打开的查询编辑器中。

   ```sql
   CREATE DATABASE tutorialdb;
   ```

3. 从工具栏中选择“运行”以执行查询。 通知显示在“消息”窗格中，以显示查询进度。

>[!TIP]
> 可以使用键盘上的“F5”而不是使用“运行”来执行语句 。

查询完成后，右键单击“数据库”并选择“刷新”，可在“数据库”节点下的列表中看到“tutorialdb”   。


## <a name="create-a-table"></a>创建表

 以下步骤会在“tutorialdb”中创建一个表：

1. 使用查询编辑器中的下拉列表将连接上下文更改为“tutorialdb”。 

   ![更改上下文](media/quickstart-postgresql/change-context.png)

2. 将以下 SQL 语句粘贴到查询编辑器中，并单击“运行”。 

   > [!NOTE]
   > 可以在编辑器中追加此语句或覆盖现有查询。 单击“运行”将仅执行突出显示的查询。 如果未突出显示任何内容，则单击“运行”将执行编辑器中的所有查询。

   ```sql
   -- Drop the table if it already exists
   DROP TABLE IF EXISTS customers;
   -- Create a new table called 'customers'
   CREATE TABLE customers(
       customer_id SERIAL PRIMARY KEY,
       name VARCHAR (50) NOT NULL,
       location VARCHAR (50) NOT NULL,
       email VARCHAR (50) NOT NULL
   );
   ```

## <a name="insert-rows"></a>插入行

将以下代码片段粘贴到查询窗口，并单击“运行”：

   ```sql
   -- Insert rows into table 'customers'
   INSERT INTO customers
       (customer_id, name, location, email)
    VALUES
      ( 1, 'Orlando', 'Australia', ''),
      ( 2, 'Keith', 'India', 'keith0@adventure-works.com'),
      ( 3, 'Donna', 'Germany', 'donna0@adventure-works.com'),
      ( 4, 'Janet', 'United States','janet1@adventure-works.com');
   ```

## <a name="query-the-data"></a>查询数据

1. 将以下代码片段粘贴到查询编辑器中，并单击“运行”：
   
   ```sql
   -- Select rows from table 'customers'
   SELECT * FROM customers; 
   ```

2. 将显示查询结果：

   ![查看结果](media/quickstart-postgresql/view-results.png)

## <a name="next-steps"></a>后续步骤

了解 [Azure Data Studio 中可用于 Postgres 的方案](./extensions/postgres-extension.md)。