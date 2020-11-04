---
title: 使用 Transact-SQL 编辑器创建数据库对象
description: 请按照本教程的说明操作，了解如何使用 Transact-SQL 编辑器来执行核心数据库任务，包括创建和搜索数据库对象。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18; seo-lt-2019
ms.date: 09/24/2018
ms.openlocfilehash: e2c200bc57bc62a54a9850e85e13b9c9f15c49f0
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243376"
---
# <a name="tutorial-use-the-transact-sql-editor-to-create-database-objects---azure-data-studio"></a>教程：使用 Transact-SQL 编辑器创建数据库对象 - Azure Data Studio

创建和运行查询、存储过程、脚本等是数据库专业人员的核心任务。 本教程演示 T-SQL 编辑器中用于创建数据库对象的主要功能。

在本教程中，你将了解如何使用 [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] 执行以下操作：
> [!div class="checklist"]
> * 搜索数据库对象
> * 编辑表数据 
> * 使用片段快速编写 T-SQL
> * 使用“速览定义”和“转到定义” 查看数据库对象详细信息


## <a name="prerequisites"></a>先决条件

本教程需要使用 SQL Server 或 Azure SQL 数据库 TutorialDB。 若要创建 TutorialDB 数据库，请完成以下其中一项快速入门：

- [使用 [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] 连接并查询 SQL Server](quickstart-sql-server.md)
- [使用 [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] 连接并查询 Azure SQL 数据库](quickstart-sql-database.md)


## <a name="quickly-locate-a-database-object-and-perform-a-common-task"></a>快速查找数据库对象并执行常见任务

[!INCLUDE[name-sos-short](../includes/name-sos-short.md)] 提供搜索小组件以快速查找数据库对象。 结果列表为与所选对象相关的常见任务提供了上下文菜单，如表的“编辑数据”。

1. 打开“服务器”边栏 (Ctrl+G)，展开“数据库”，然后选择“TutorialDB” 。 

1. 右键单击“TutorialDB”，从上下文菜单中选择“管理”，打开“TutorialDB 仪表板” ：

   ![上下文菜单 - 管理](./media/tutorial-sql-editor/insight-open-dashboard.png)

1. 在仪表板上，右键单击“dbo.Customers”（在搜索小组件中），选择“编辑数据” 。
   
   > [!TIP]
   > 对于包含多个对象的数据库，使用搜索小组件可以快速找到要查找的表、视图等。

   ![快速搜索小组件](./media/tutorial-sql-editor/quick-search-widget.png)

1. 编辑第一行中的“Email”列，键入 orlando0\@adventure-works.com，然后按 Enter 保存更改。

   ![编辑数据](./media/tutorial-sql-editor/edit-data.png)

## <a name="use-t-sql-snippets-to-create-stored-procedures"></a>使用 T-SQL 片段创建存储过程

Azure Data Studio 提供了许多用于快速创建语句的内置 T-SQL 代码片段。


1. 按 Ctrl + N，打开新的查询编辑器。

2. 在编辑器中键入“sql”，使用向下键移动到“sqlCreateStoredProcedure”，然后按 Tab（或 Enter）以加载创建存储过程片段  。

   ![显示查询编辑器的屏幕截图，其中在编辑器中键入了 sql，且突出显示了 sqlCreateStoredProcedure 选项。](./media/tutorial-sql-editor/snippet-list.png)

3. 创建存储过程片段设置了两个字段（StoredProcedureName 和 SchemaName），以便快速进行编辑 。 选择“StoredProcedureName”，右键单击并选择“更改所有匹配项”。 接下来键入 getCustomer，所有 StoredProcedureName 条目都会更改为 getCustomer  。

   ![显示查询编辑器的屏幕截图，其中突出显示了“更改所有出现项”选项。](./media/tutorial-sql-editor/snippet.png)

5. 将所有出现的 SchemaName 更改为 dbo 。 
6. 该片段包含需要更新的占位符参数和正文文本。 EXECUTE 语句还包含占位符文本，因为它不知道该过程将具有多少参数。 对于本教程，请更新该片段，使其类似于以下代码：

    ```sql
    -- Create a new stored procedure called 'getCustomer' in schema 'dbo'
    -- Drop the stored procedure if it already exists
    IF EXISTS (
    SELECT *
    FROM INFORMATION_SCHEMA.ROUTINES
    WHERE SPECIFIC_SCHEMA = N'dbo'
    AND SPECIFIC_NAME = N'getCustomer'
    )
    DROP PROCEDURE dbo.getCustomer
    GO
    -- Create the stored procedure in the specified schema
    CREATE PROCEDURE dbo.getCustomer
    @ID int
    -- add more stored procedure parameters here
    AS
    -- body of the stored procedure
    SELECT  c.CustomerId, 
    c.Name, 
    c.Location, 
    c.Email
    FROM dbo.Customers c
    WHERE c.CustomerId = @ID
    FOR JSON PATH

    GO
    -- example to execute the stored procedure we just created
    EXECUTE dbo.getCustomer 1
    GO
    ```
    
5. 若要创建存储过程并为对其运行测试，请按 F5。

存储过程现已创建，“结果”窗格将显示 JSON 中返回的客户。 若要查看格式化的 JSON，请单击返回的记录。 


## <a name="use-peek-definition"></a>使用速览定义 

Azure Data Studio 提供了使用查看定义功能查看对象定义的功能。 本部分将创建另一个存储过程，并使用速览定义查看表中哪些列可快速创建存储过程的正文。

1. 按 Ctrl + N，打开新的编辑器。 

2. 在编辑器中键入“sql”，使用向下键移动到“sqlCreateStoredProcedure”，然后按 Tab（或 Enter）以加载创建存储过程片段   。
3. 对于 StoredProcedureName，键入“setCustomer”，对于 SchemaName，键入“dbo”

3. 将 @param 占位符替换为以下参数定义：

   ```sql
   @json_val nvarchar(max)
   ```

4. 将存储过程的正文替换为以下代码：
   ```sql
   INSERT INTO dbo.Customers
   ```

5. 在刚刚添加的 INSERT 行中，右键单击“dbo.Customers”并选择“速览定义” 。

   ![速览定义](./media/tutorial-sql-editor/peek-definition.png)

6. 随即将显示表定义，方便你快速查看哪些列存在于表中。 请参阅“列”列表，轻松完成存储过程的语句。 完成先前添加的 INSERT 语句的创建，以完成存储过程的正文，并关闭速览定义窗口：

   ```sql
   INSERT INTO dbo.Customers (CustomerId, Name, Location, Email)
       SELECT CustomerId, Name, Location, Email
       FROM OPENJSON (@json_val)
       WITH(   CustomerId int, 
               Name nvarchar(50), 
               Location nvarchar(50), 
               Email nvarchar(50)
    )
   ```
7. 删除（或注释掉）查询底部的“EXECUTE”命令。
8. 整个语句应类似于以下代码：

   ```sql
   -- Create a new stored procedure called 'setCustomer' in schema 'dbo'
   -- Drop the stored procedure if it already exists
   IF EXISTS (
   SELECT *
       FROM INFORMATION_SCHEMA.ROUTINES
       WHERE SPECIFIC_SCHEMA = N'dbo'
       AND SPECIFIC_NAME = N'setCustomer'
   )
   DROP PROCEDURE dbo.setCustomer
   GO
   -- Create the stored procedure in the specified schema
   CREATE PROCEDURE dbo.setCustomer
       @json_val nvarchar(max) 
   AS
       -- body of the stored procedure
       INSERT INTO dbo.Customers (CustomerId, Name, Location, Email)
       SELECT CustomerId, Name, Location, Email
       FROM OPENJSON (@json_val)
       WITH(   CustomerId int, 
               Name nvarchar(50), 
               Location nvarchar(50), 
               Email nvarchar(50)
       )
   GO
   ```

8. 若要创建 setCustomer 存储过程，请按 F5。

## <a name="use-save-query-results-as-json-to-test-the-setcustomer-stored-procedure"></a>使用“将查询结果保存为 JSON”来测试 setCustomer 存储过程

在上一部分中创建的 setCustomer 存储过程要求将 JSON 数据传递到 \@json_val 参数中 。 本部分演示如何获取格式正确的 JSON 位，并将其传入参数，以便可以测试存储过程。

1. 在“服务器”边栏中，右键单击 “dbo.Customers”表，然后单击“选择前 1000 行”。

2. 在结果视图中选择第一行，确保选中整行（单击最左侧列中的数字 1）并选择“另存为 JSON”。  
3. 将文件夹更改到你能记住的位置，以便以后可以删除文件（例如桌面），然后单击“保存”。 打开 JSON 格式的文件。

   ![另存为 JSON](./media/tutorial-sql-editor/save-as-json.png)

4. 在编辑器中选择 JSON 数据并复制它。
5. 按 Ctrl + N，打开新的编辑器。
6. 前面的步骤演示了如何轻松地获取格式正确的数据，以完成对 setCustomer 过程的调用。 可以看到以下代码对新客户详细信息使用了相同的 JSON 格式，因此我们可以测试 setCustomer 过程。 语句包含用于声明参数并运行新的 get 和 set 过程的语法。 可以从上一节中粘贴已复制的数据，并对其进行编辑，使其与下面的示例相同，或只需将以下语句粘贴到查询编辑器中。

   ```sql
   -- example to execute the stored procedure we just created
   declare @json nvarchar(max) =
   N'[
       {
           "CustomerId": 5,
           "Name": "Lucy",
           "Location": "Canada",
           "Email": "lucy0@adventure-works.com"
       }
   ]'

   EXECUTE dbo.setCustomer @json_val = @json
   GO

   EXECUTE dbo.getCustomer @ID = 5
   ```

7. 按 F5 执行该脚本。 此脚本插入新客户，并以 JSON 格式返回新客户的信息。 单击该结果可打开格式化视图。

   ![测试结果](./media/tutorial-sql-editor/test-result.png)

## <a name="next-steps"></a>后续步骤
在本教程中，你了解了如何执行以下操作：
> [!div class="checklist"]
> * 快速搜索架构对象
> * 编辑表数据 
> * 使用片段编写 T-SQL 脚本
> * 使用速览定义和转到定义了解数据库对象详细信息


若要了解如何启用“5 个速度最慢的查询”小组件，请完成下一教程：

> [!div class="nextstepaction"]
> [启用慢查询示例见解小组件](tutorial-qds-sql-server.md)
