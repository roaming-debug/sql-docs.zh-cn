---
description: T-SQL 教程：创建和查询数据库对象
title: T-SQL 教程：创建和查询数据库对象 |Microsoft Docs
ms.custom: ''
ms.date: 07/30/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
ms.assetid: 9fb8656b-0e4e-4ada-b404-4db4d3eea995
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8d5649928b9e937fef37c7c67bc85c5f560dac20
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460870"
---
# <a name="lesson-1-create-and-query-database-objects"></a>第 1 课：创建和查询数据库对象
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]

本课将介绍如何创建数据库，在数据库中创建表，然后访问表中的数据并对其进行更改。 由于本课是对使用 [!INCLUDE[tsql](../includes/tsql-md.md)]的简介，因此它未使用或说明为这些语句提供的许多选项。  
  
[!INCLUDE[tsql](../includes/tsql-md.md)] 语句可以使用下列方法进行编写并提交到 [!INCLUDE[ssDE](../includes/ssde-md.md)] ：  
  
-   通过使用 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]。 本教程假定使用的是 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]，但是也可以使用 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] Express（可以从 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=14630)免费下载）。  
  
-   通过使用 [sqlcmd 实用工具](../tools/sqlcmd-utility.md)。  
  
-   通过从您创建的应用程序进行连接。  
  
代码以相同方式和相同权限在 [!INCLUDE[ssDE](../includes/ssde-md.md)] 上执行，而不管您如何提交代码语句。  
  
若要在 [!INCLUDE[tsql](../includes/tsql-md.md)] 中运行 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]语句，请打开 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 并连接到 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]的实例。  

## <a name="prerequisites"></a>先决条件
若要完成本教程，需要 SQL Server Management Studio 以及针对 SQL Server 实例的访问权限。 

- 安装 [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)。

如果没有 SQL Server 实例，请创建一个。 若要创建一个 SQL Server 实例，请从以下链接选择平台。 如果选择 SQL 身份验证，请使用 SQL Server 登录凭据。
- **Windows**：[下载 SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads)。
- **macOS**：[在 Docker 上下载 SQL Server 2017](../linux/quickstart-install-connect-docker.md)。

## <a name="create-a-database"></a>创建数据库
与许多 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句一样，[`CREATE DATABASE`](statements/create-database-transact-sql.md) 语句具有一个必需参数：数据库的名称。` CREATE DATABASE` 还具有许多可选参数，如希望放置数据库文件的磁盘位置。 当你执行不带可选参数的 `CREATE DATABASE` 时，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 使用其中许多参数的默认值。

1.  在查询编辑器窗口中，键入以下代码，但不要执行它：  
  
    ```sql  
    CREATE DATABASE TestData  
    GO  
    ```  
  
2.  使用指针选择词语 `CREATE DATABASE`，再按 **F1**。 应该会打开 SQL Server 联机丛书中的 `CREATE DATABASE` 主题。 你可以使用此方法查找 `CREATE DATABASE` 以及在本教程中使用的其他语句的完整语法。  
  
3.  在查询编辑器中，按 **F5** 以执行语句并创建名为 `TestData`的数据库。  
  
创建数据库时， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 制作 **model** 数据库的副本，并将该副本重命名为数据库名称。 除非您将初始大小很大的数据库指定为可选参数，否则此操作应该只需要几秒钟。  
  
> [!NOTE]  
> 在单个批处理中提交多条语句时，可以用关键字 GO 分隔各语句。 当批处理只包含一条语句时，GO 是可选的。  

## <a name="create-a-table"></a>创建表

[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]

若要创建表，您必须提供该表的名称以及该表中每个列的名称和数据类型。 指出每个列中是否允许空值，也是一种很好的做法。 若要创建表时，必须具有 `CREATE TABLE` 权限，以及对包含该表的架构的 `ALTER SCHEMA` 权限。 [`db_ddladmin`](../relational-databases/security/authentication-access/database-level-roles.md) 固定数据库角色具有这些权限。  
  
大多数表有一个主键，主键由表的一列或多列组成。 主键始终是唯一的。 [!INCLUDE[ssDE](../includes/ssde-md.md)] 将强制实施以下限制：表中的任何主键值都不能重复。  
  
有关数据类型的列表以及每种数据类型的说明链接，请参阅[数据类型 (Transact-SQL)](../t-sql/data-types/data-types-transact-sql.md)。  
  
> [!NOTE]  
> [!INCLUDE[ssDE](../includes/ssde-md.md)] 可安装为区分大小写或不区分大小写。 如果 [!INCLUDE[ssDE](../includes/ssde-md.md)] 区分大小写进行安装，则对象名必须始终具有相同的大小写。 例如，名为 OrderData 的表与名为 ORDERDATA 的表是不同的表。 如果 [!INCLUDE[ssDE](../includes/ssde-md.md)] 按不区分大小写进行安装，则这两个表名被视为同一个表，而且该名称只能使用一次。  
  
  
### <a name="switch-the-query-editor-connection-to-the-testdata-database"></a>将查询编辑器连接切换到 TestData 数据库  

在查询编辑器窗口中，键入以下代码，并执行它以更改与 `TestData` 数据库的连接。  
  
  ```sql  
  USE TestData  
  GO  
  ```  
  
### <a name="create-the-table"></a>创建表

在查询编辑器窗口中，键入以下代码，并执行它以创建一个名为 `Products` 的表。 该表中各列的名称为 `ProductID`、 `ProductName`、 `Price`和 `ProductDescription`。 `ProductID` 列是表的主键。 `int``varchar(25)`、 `money`和 `varchar(max)` 都是数据类型。 当插入或更改行时，只有 `Price` 和 `ProductionDescription` 列可以不包含数据。 此语句包含称为架构的可选元素 (`dbo.`)。 架构是拥有表的数据库对象。 如果您是管理员，则 `dbo` 是默认架构。 `dbo` 代表数据库所有者。  
  
  ```sql  
  CREATE TABLE dbo.Products  
     (ProductID int PRIMARY KEY NOT NULL,  
     ProductName varchar(25) NOT NULL,  
     Price money NULL,  
     ProductDescription varchar(max) NULL)  
  GO  
 ```  

## <a name="insert-and-update-data-in-a-table"></a>插入和更新表中的数据
现在已经创建了 **Products** 表，可以通过使用 INSERT 语句将数据插入到表中。 插入数据后，将通过使用 UPDATE 语句更改行的内容。 您将使用 UPDATE 语句的 WHERE 子句，以限制对单个行的更新。 这四条语句将输入以下数据。  
  
|ProductID|ProductName|价格|ProductDescription|  
|-------------|---------------|---------|----------------------|  
|1|Clamp|12.48|Workbench clamp|  
|50|Screwdriver|3.17|Flat head|  
|75|Tire Bar||Tool for changing tires.|  
|3000|3 mm Bracket|0.52||  
  
基本语法如下：INSERT、表名、列的列表、VALUES，然后是要插入的值的列表。 如果某行的前面有两个连字符，则指示该行为注释，编译器将忽略其文本。 在这种情况下，注释说明允许的语法变体。  
  
### <a name="insert-data-into-a-table"></a>将数据插入到表  
  
1.  执行以下语句，将一行插入到在上一个任务中创建的 `Products` 表中。
  
   ```sql 
   -- Standard syntax  
   INSERT dbo.Products (ProductID, ProductName, Price, ProductDescription)  
       VALUES (1, 'Clamp', 12.48, 'Workbench clamp')  
   GO   
   ```  

   > [!NOTE]
   > 如果插入成功，则继续执行下一步。
   >
   > 如果插入操作失败，则可能是因为 `Product` 表中已存在具有该产品 ID 的行。 若要继续，请删除表中的所有行并重复上一步。 [`TRUNCATE TABLE`](statements/truncate-table-transact-sql.md) 删除表中的所有行。 
   >
   > 运行以下命令来删除表中的所有行：
   > 
   > ```sql
   >TRUNCATE TABLE TestData.dbo.Products;
   > GO
   >```
   >
   > 截断表后，在此步骤中重复 `INSERT` 命令。

2.  以下语句显示如何通过在字段列表（在圆括号中）中和值列表中均切换 `ProductID` 和 `ProductName` 的位置，更改提供参数的顺序。  
  
   ```sql  
   -- Changing the order of the columns  
   INSERT dbo.Products (ProductName, ProductID, Price, ProductDescription)  
       VALUES ('Screwdriver', 50, 3.17, 'Flat head')  
   GO    
   ```  
  
3.  以下语句演示，只要值是按正确顺序列出的，列的名称就是可选的。 此语法很常见，但是建议不要使用它，因为其他人了解您的代码可能会更困难。 `NULL` 为 `Price` 列指定，因为还不知道此产品的价格。  
  
   ```sql  
   -- Skipping the column list, but keeping the values in order  
   INSERT dbo.Products  
       VALUES (75, 'Tire Bar', NULL, 'Tool for changing tires.')  
   GO  
  ```  
  
4.  只要在默认架构中访问和更改表，架构名称就是可选的。 由于 `ProductDescription` 列允许 Null 值，而且没有提供值，因此可以从语句中完全删除 `ProductDescription` 列的名称和值。  
  
   ```sql  
   -- Dropping the optional dbo and dropping the ProductDescription column  
   INSERT Products (ProductID, ProductName, Price)  
       VALUES (3000, '3 mm Bracket', 0.52)  
   GO  
   ```  
  
### <a name="update-the-products-table"></a>更新 products 表  
键入并执行以下 `UPDATE` 语句，将第二种产品的 `ProductName` 从 `Screwdriver`更改为 `Flat Head Screwdriver`。  
  
  ```sql  
  UPDATE dbo.Products  
      SET ProductName = 'Flat Head Screwdriver'  
      WHERE ProductID = 50  
  GO  
  ```  

## <a name="read-data-from-a-table"></a>从表中读取数据
使用 SELECT 语句可以读取表中的数据。 SELECT 语句是最重要的 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句之一，其语法有许多变体。 在本教程中，您将使用五个简单版本。  
  
### <a name="read-the-data-in-a-table"></a>读取表中的数据  
  
1.  键入并执行以下语句以读取 `Products` 表中的数据。  
  
  ```sql 
  -- The basic syntax for reading data from a single table  
  SELECT ProductID, ProductName, Price, ProductDescription  
      FROM dbo.Products  
  GO  
  ```  
  
2.  你可以使用星号 (`*`) 选择表中的所有列。 星号用于临时查询。 在永久代码中，请提供列列表，使语句返回预测列，即使稍后会将新列添加到表中，也是如此。  
  
  ```sql  
  -- Returns all columns in the table  
  -- Does not use the optional schema, dbo  
  SELECT * FROM Products  
  GO   
  ```  
  
3.  可以省略不希望返回的列。 列将按列出它们的顺序返回。  
  
  ```sql  
  -- Returns only two of the columns from the table  
  SELECT ProductName, Price  
      FROM dbo.Products  
  GO    
  ```  
  
4.  使用 `WHERE` 子句可以限制返回给用户的行。  
  
  ``` sql 
  -- Returns only two of the records in the table  
  SELECT ProductID, ProductName, Price, ProductDescription  
      FROM dbo.Products  
      WHERE ProductID < 60  
  GO    
  ```  
  
5.  您可以在返回列中的值时使用它们。 以下示例对 `Price` 列执行数学运算。 除非通过使用 `AS` 关键字提供一个名称，否则以此方式更改的列将没有名称。  
  
  ```sql  
  -- Returns ProductName and the Price including a 7% tax  
  -- Provides the name CustomerPays for the calculated column  
  SELECT ProductName, Price * 1.07 AS CustomerPays  
      FROM dbo.Products  
  GO  
  ```  
  
### <a name="useful-functions-in-a-select-statement"></a>SELECT 语句中的有用函数  
有关可以在 SELECT 语句中用来处理数据的一些函数的信息，请参阅下列主题：  

:::row:::
    :::column:::
        [字符串函数 (Transact-SQL)](../t-sql/functions/string-functions-transact-sql.md)
    :::column-end:::
    :::column:::
        [日期和时间数据类型及函数 (Transact-SQL)](../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [数学函数 (Transact-SQL)](../t-sql/functions/mathematical-functions-transact-sql.md)
    :::column-end:::
    :::column:::
        [文本与图像函数 (Transact-SQL)](./functions/text-and-image-functions-textptr-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="create-views-and-stored-procedures"></a>创建视图和存储过程
视图是存储的 SELECT 语句，而存储过程是以批处理方式执行的一条或多条 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句。  
  
视图像表那样进行查询，但不接受参数。 存储过程比视图更复杂。 存储过程可以同时具有输入参数和输出参数，并可以包括控制代码流的语句，如 IF 和 WHILE 语句。 将存储过程用于数据库中的所有重复操作，是一个良好的编程做法。  
  
在此示例中，你将使用 CREATE VIEW 创建一个视图，该视图仅选择 **Products** 表中的两列。 然后，您将使用 CREATE PROCEDURE 创建一个存储过程，该存储过程接受价格参数，并仅返回价格小于指定参数值的那些产品。  
  
### <a name="create-a-view"></a>创建视图  
  
执行以下语句创建一个视图，该视图执行 Select 语句，并将产品的名称和价格返回给用户。  
  
  ```sql  
  CREATE VIEW vw_Names  
     AS  
     SELECT ProductName, Price FROM Products;  
  GO    
  ```  
  
### <a name="test-the-view"></a>测试视图  
  
视图的处理方式与表类似。 使用 `SELECT` 语句访问视图。  
  
  ```sql  
  SELECT * FROM vw_Names;  
  GO   
  ```  
  
### <a name="create-a-stored-procedure"></a>创建存储过程  
  
以下语句创建一个名为 `pr_Names`的存储过程，接受名为 `@VarPrice` 、数据类型为 `money`的输入参数。 该存储过程打印与输入参数（已从 `Products less than` 数据类型更改为 `money` 字符数据类型）串联的语句 `varchar(10)` 。 然后，该存储过程对视图执行 `SELECT` 语句，将输入参数作为 `WHERE` 子句的一部分进行传递。 这将返回价格小于输入参数值的所有产品。  
  
  ```sql  
  CREATE PROCEDURE pr_Names @VarPrice money  
     AS  
     BEGIN  
        -- The print statement returns text to the user  
        PRINT 'Products less than ' + CAST(@VarPrice AS varchar(10));  
        -- A second statement starts here  
        SELECT ProductName, Price FROM vw_Names  
              WHERE Price < @varPrice;  
     END  
  GO    
  ```  
  
### <a name="test-the-stored-procedure"></a>测试存储过程  
  
若要测试存储过程，请键入并执行以下语句。 存储过程应该返回在第 1 课中向 `Products` 表中输入的、其价格小于 `10.00`的两个产品的名称。  
  
  ```sql  
  EXECUTE pr_Names 10.00;  
  GO  
  ```  

## <a name="next-steps"></a>后续步骤
下一篇文章将介绍如何配置对数据库对象的权限。 第 2 课将使用第 1 课中创建的对象。 

转到下一篇文章，了解详细信息：
> [!div class="nextstepaction"]
> [后续步骤](../t-sql/lesson-2-configuring-permissions-on-database-objects.md)
  
  
