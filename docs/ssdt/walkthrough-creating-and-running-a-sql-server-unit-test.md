---
title: 创建和运行 SQL Server 单元测试
description: 了解如何创建 SQL Server 单元测试。 演练设置检测存储过程中错误的测试的步骤。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 992c1d8e-3729-438b-9ef4-cd103e28f145
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6b0eaf98f4d10c08548e898c261a489ad71f0ff4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016727"
---
# <a name="walkthrough-creating-and-running-a-sql-server-unit-test"></a>演练：创建和运行 SQL Server 单元测试

在本演练中，将创建一个 SQL Server 单元测试，该测试验证多个存储过程的行为。 创建 SQL Server 单元测试可帮助确定可能会引发不正确的应用程序行为的代码缺陷。 可以将 SQL Server 单元测试和应用程序测试作为一组自动执行的测试的一部分来运行。  
  
在本演练中，您将执行下列任务：  
  
-   [创建包含数据库架构的脚本](#CreateScript)  
  
-   [创建数据库项目并导入架构](#CreateProjectAndImport)  
  
-   [将数据库项目部署到独立开发环境中](#DeployDBProj)  
  
-   [创建 SQL Server 单元测试](#CreateDBUnitTests)  
  
-   [定义测试逻辑](#DefineTestLogic)  
  
-   [运行 SQL Server 单元测试](#RunTests)  
  
-   [添加负面单元测试](#NegativeTest)  
  
当某个单元测试在存储过程中检测到错误后，您需要更正该错误，然后重新运行测试。  
  
## <a name="prerequisites"></a>先决条件  
若要完成本演练，您必须能够连接到有权在其上创建和部署数据库的数据库服务器（或 LocalDB 数据库）。 有关详细信息，请参阅 [执行 Visual Studio 的数据库功能所需的权限](/previous-versions/visualstudio/visual-studio-2010/aa833413(v=vs.100))。  
  
## <a name="create-a-script-that-contains-a-database-schema"></a><a name="CreateScript"></a>创建包含数据库架构的脚本  
  
#### <a name="to-create-a-script-from-which-you-can-import-a-schema"></a>创建可从中导入架构的脚本  
  
1.  在 **“文件”** 菜单上，指向 **“新建”** ，然后单击 **“文件”** 。  
  
    此时将显示 **“新建文件”** 对话框。  
  
2.  在 **“类别”** 列表中，单击 **“常规”** （如果它尚未突出显示）。  
  
3.  在 **“模板”** 列表中，单击 **“Sql 文件”** ，然后单击 **“打开”** 。  
  
    将打开 Transact\-SQL 编辑器。  
  
4.  复制以下 Transact\-SQL 代码，并将其粘贴到 Transact\-SQL 编辑器中。  
  
    ```  
    PRINT N'Creating Sales...';  
    GO  
    CREATE SCHEMA [Sales]  
        AUTHORIZATION [dbo];  
    GO  
    PRINT N'Creating Sales.Customer...';  
    GO  
    CREATE TABLE [Sales].[Customer] (  
        [CustomerID]   INT           IDENTITY (1, 1) NOT NULL,  
        [CustomerName] NVARCHAR (40) NOT NULL,  
        [YTDOrders]    INT           NOT NULL,  
        [YTDSales]     INT           NOT NULL  
    );  
    GO  
    PRINT N'Creating Sales.Orders...';  
    GO  
    CREATE TABLE [Sales].[Orders] (  
        [CustomerID] INT      NOT NULL,  
        [OrderID]    INT      IDENTITY (1, 1) NOT NULL,  
        [OrderDate]  DATETIME NOT NULL,  
        [FilledDate] DATETIME NULL,  
        [Status]     CHAR (1) NOT NULL,  
        [Amount]     INT      NOT NULL  
    );  
    GO  
    PRINT N'Creating Sales.Def_Customer_YTDOrders...';  
    GO  
    ALTER TABLE [Sales].[Customer]  
        ADD CONSTRAINT [Def_Customer_YTDOrders] DEFAULT 0 FOR [YTDOrders];  
    GO  
    PRINT N'Creating Sales.Def_Customer_YTDSales...';  
    GO  
    ALTER TABLE [Sales].[Customer]  
        ADD CONSTRAINT [Def_Customer_YTDSales] DEFAULT 0 FOR [YTDSales];  
    GO  
    PRINT N'Creating Sales.Def_Orders_OrderDate...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [Def_Orders_OrderDate] DEFAULT GetDate() FOR [OrderDate];  
    GO  
    PRINT N'Creating Sales.Def_Orders_Status...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [Def_Orders_Status] DEFAULT 'O' FOR [Status];  
    GO  
    PRINT N'Creating Sales.PK_Customer_CustID...';  
    GO  
    ALTER TABLE [Sales].[Customer]  
        ADD CONSTRAINT [PK_Customer_CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) WITH (ALLOW_PAGE_LOCKS = ON, ALLOW_ROW_LOCKS = ON, PAD_INDEX = OFF, IGNORE_DUP_KEY = OFF, STATISTICS_NORECOMPUTE = OFF);  
    GO  
    PRINT N'Creating Sales.PK_Orders_OrderID...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [PK_Orders_OrderID] PRIMARY KEY CLUSTERED ([OrderID] ASC) WITH (ALLOW_PAGE_LOCKS = ON, ALLOW_ROW_LOCKS = ON, PAD_INDEX = OFF, IGNORE_DUP_KEY = OFF, STATISTICS_NORECOMPUTE = OFF);  
    GO  
    PRINT N'Creating Sales.FK_Orders_Customer_CustID...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [FK_Orders_Customer_CustID] FOREIGN KEY ([CustomerID]) REFERENCES [Sales].[Customer] ([CustomerID]) ON DELETE NO ACTION ON UPDATE NO ACTION;  
    GO  
    PRINT N'Creating Sales.CK_Orders_FilledDate...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [CK_Orders_FilledDate] CHECK ((FilledDate >= OrderDate) AND (FilledDate < '01/01/2030'));  
    GO  
    PRINT N'Creating Sales.CK_Orders_OrderDate...';  
    GO  
    ALTER TABLE [Sales].[Orders]  
        ADD CONSTRAINT [CK_Orders_OrderDate] CHECK ((OrderDate > '01/01/2005') and (OrderDate < '01/01/2030'));  
    GO  
    PRINT N'Creating Sales.uspCancelOrder...';  
    GO  
    CREATE PROCEDURE [Sales].[uspCancelOrder]  
    @OrderID INT  
    AS  
    BEGIN  
    DECLARE @Delta INT, @CustomerID INT  
    BEGIN TRANSACTION  
        SELECT @Delta = [Amount], @CustomerID = [CustomerID]  
         FROM [Sales].[Orders] WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Orders]  
       SET [Status] = 'X'  
    WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Customer]  
       SET  
       YTDOrders = YTDOrders - @Delta  
        WHERE [CustomerID] = @CustomerID  
    COMMIT TRANSACTION  
    END  
    GO  
    PRINT N'Creating Sales.uspFillOrder...';  
    GO  
    CREATE PROCEDURE [Sales].[uspFillOrder]  
    @OrderID INT, @FilledDate DATETIME  
    AS  
    BEGIN  
    DECLARE @Delta INT, @CustomerID INT  
    BEGIN TRANSACTION  
        SELECT @Delta = [Amount], @CustomerID = [CustomerID]  
         FROM [Sales].[Orders] WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Orders]  
       SET [Status] = 'F',  
           [FilledDate] = @FilledDate  
    WHERE [OrderID] = @OrderID;  
  
    UPDATE [Sales].[Customer]  
       SET  
       YTDSales = YTDSales - @Delta  
        WHERE [CustomerID] = @CustomerID  
    COMMIT TRANSACTION  
    END  
    GO  
    PRINT N'Creating Sales.uspNewCustomer...';  
    GO  
    CREATE PROCEDURE [Sales].[uspNewCustomer]  
    @CustomerName NVARCHAR (40)  
    AS  
    BEGIN  
    INSERT INTO [Sales].[Customer] (CustomerName) VALUES (@CustomerName);  
    RETURN SCOPE_IDENTITY()  
    END  
    GO  
    PRINT N'Creating Sales.uspPlaceNewOrder...';  
    GO  
    CREATE PROCEDURE [Sales].[uspPlaceNewOrder]  
    @CustomerID INT, @Amount INT, @OrderDate DATETIME, @Status CHAR (1)='O'  
    AS  
    BEGIN  
    DECLARE @RC INT  
    BEGIN TRANSACTION  
    INSERT INTO [Sales].[Orders] (CustomerID, OrderDate, FilledDate, Status, Amount)   
         VALUES (@CustomerID, @OrderDate, NULL, @Status, @Amount)  
    SELECT @RC = SCOPE_IDENTITY();  
    UPDATE [Sales].[Customer]  
       SET  
       YTDOrders = YTDOrders + @Amount  
        WHERE [CustomerID] = @CustomerID  
    COMMIT TRANSACTION  
    RETURN @RC  
    END  
    GO  
    CREATE PROCEDURE [Sales].[uspShowOrderDetails]  
    @CustomerID INT=0  
    AS  
    BEGIN  
    SELECT [C].[CustomerName], CONVERT(date, [O].[OrderDate]), CONVERT(date, [O].[FilledDate]), [O].[Status], [O].[Amount]  
      FROM [Sales].[Customer] AS C  
      INNER JOIN [Sales].[Orders] AS O  
         ON [O].[CustomerID] = [C].[CustomerID]  
      WHERE [C].[CustomerID] = @CustomerID  
    END  
    GO  
    ```  
  
5.  保存文件。 记下该位置，因为在下一个过程中必须使用此脚本。  
  
6.  在 **“文件”** 菜单上，单击 **“关闭解决方案”** 。  
  
    接下来，创建一个数据库项目并从已创建的脚本导入架构。  
  
## <a name="create-a-database-project-and-import-a-schema"></a><a name="CreateProjectAndImport"></a>创建数据库项目并导入架构  
  
#### <a name="to-create-a-database-project"></a>创建数据库项目  
  
1.  在 **“文件”** 菜单中，指向 **“新建”** ，然后单击 **“项目”** 。  
  
    将显示“新建项目”对话框。  
  
2.  在“已安装的模板”下，选择“SQL Server”节点，然后选择“SQL Server 数据库项目”。  
  
3.  在 **“名称”** 中，键入 **SimpleUnitTestDB**。  
  
4.  如果尚未选中 **“创建解决方案的目录”** 复选框，请选中此复选框。  
  
5.  如果尚未清除 **“添加到源代码管理”** 复选框，请清除此复选框，然后单击 **“确定”** 。  
  
    将创建数据库项目，并且它将显示在 **“解决方案资源管理器”** 中。 接下来，从脚本导入数据库架构。  
  
#### <a name="to-import-a-database-schema-from-a-script"></a>从脚本导入数据库架构  
  
1.  在“项目”菜单上，单击“导入”，然后单击“脚本(\*.sql)”。  
  
2.  在阅读“欢迎使用”页后，单击 **“下一步”** 。  
  
3.  单击 **“浏览”** ，转到您保存 .sql 文件的目录。  
  
4.  双击 .sql 文件，然后单击“完成”。  
  
    将导入脚本，并且该脚本中定义的对象将添加到您的数据库项目中。  
  
5.  查看摘要，然后单击 **“完成”** 以完成操作。  
  
    > [!NOTE]  
    > Sales.uspFillOrder 过程包含一个故意生成的编码错误，您将发现该错误并在此过程稍后的步骤中更正它。  
  
#### <a name="to-examine-the-resulting-project"></a>查看生成的项目  
  
1.  在 **“解决方案资源管理器”** 中，检查已导入到项目中的脚本文件。  
  
2.  在“SQL Server 对象资源管理器”中，在“项目”节点中查看该数据库。  
  
## <a name="deploying-to-localdb"></a><a name="DeployDBProj"></a>部署到 LocalDB  
默认情况下，在您按 F5 时，会将数据库部署（或发布）到 LocalDB 数据库。 您可以通过转到项目属性页的“调试”选项卡并更改连接字符串，更改数据库位置。  
  
## <a name="create-sql-server-unit-tests"></a><a name="CreateDBUnitTests"></a>创建 SQL Server 单元测试  
  
#### <a name="to-create-a-sql-server-unit-test-for-the-stored-procedures"></a>创建针对存储过程的 SQL Server 单元测试  
  
1.  在“SQL Server 对象资源管理器”中，展开“SimpleUnitTestDB”的项目节点，然后展开“可编程性”和“存储过程”节点。  
  
2.  右键单击存储过程之一，然后单击“创建单元测试”以便显示“创建单元测试”对话框。  
  
3.  选中所有五个存储过程对应的复选框：“Sales.uspCancelOrder” 、“Sales.uspFillOrder” 、“Sales.uspNewCustomer” 、“Sales.uspPlaceNewOrder” 和“Sales.uspShowOrderDetails” 。  
  
4.  在“项目”下拉列表中，选择“创建新的 Visual C# 测试项目”。  
  
5.  接受项目名称和类名称的默认名称，然后单击 **“确定”** 。  
  
6.  在测试配置对话框的 **“使用以下数据连接执行单元测试”** 中，指定与您在本演练前面部署的数据库的连接。 例如，如果使用了默认部署位置（即 LocalDB），应单击“新建连接”来指定 (LocalDB)\Projects。 然后，选择该数据库的名称。 再单击“确定”以关闭 **“连接属性”** 对话框。  
  
    > [!NOTE]  
    > 如果必须测试具有有限权限的视图或存储过程，则通常会在此步骤中指定该连接。 然后，指定具有更广泛权限的辅助连接来验证测试。 如果您具有辅助连接，则应该将该用户添加到数据库项目中，并在预先部署脚本中为该用户创建登录名。  
  
7.  在测试配置对话框的 **“部署”** 部分中，选中 **“运行单元测试前，自动部署该数据库项目”** 复选框。  
  
8.  在 **“数据库项目”** 中，单击 **SimpleUnitTestDB.sqlproj**。  
  
9. 在 **“部署配置”** 中，单击 **“调试”** 。  
  
    在 SQL Server 单元测试过程中，可能还需要生成测试数据。 对于本演练，您将跳过该步骤，因为测试将创建自己的数据。  
  
10. 单击“确定”。  
  
    将生成测试项目，并且将显示 SQL Server 单元测试设计器。 接下来，需要在单元测试的 Transact\-SQL 脚本中更新测试逻辑。  
  
## <a name="define-test-logic"></a><a name="DefineTestLogic"></a>定义测试逻辑  
此非常简单的数据库具有两个表 Customer 和 Order。 可以使用以下存储过程更新数据库：  
  
-   uspNewCustomer - 此存储过程向 Customer 表中添加记录，以将客户的“YTDOrders”和“YTDSales”列设置为零。  
  
-   uspPlaceNewOrder - 此存储过程向指定客户的 Orders 表中添加记录，并更新 Customer 表中相应记录中的 YTDOrders 值。  
  
-   uspFillOrder - 此存储过程通过将状态从“O”更改为“F”来更新 Orders 表中的记录，并增加了 Customer 表中相应记录中的 YTDSales 金额。  
  
-   uspCancelOrder - 此存储过程通过将状态从“O”更改为“X”来更新 Orders 表中的记录，并减少了 Customer 表中相应记录中的 YTDOrders 金额。  
  
-   uspShowOrderDetails - 此存储过程将 Orders 表与 Custom 表相联接并显示特定客户的记录。  
  
> [!NOTE]  
> 此示例说明如何创建简单的 SQL Server 单元测试。 在实际数据库中，您可以对特定客户的状态为“O”或“F”的所有订单的总额进行合计。 本演练中的过程也没有包含错误处理。 例如，它们不会阻止您对已填充的订单调用 uspFillOrder。  
  
测试假定数据库以干净状态启动。 您将创建验证以下条件的测试：  
  
-   uspNewCustomer - 验证在您运行存储过程后 Customer 表是否包含一行。  
  
-   uspPlaceNewOrder - 对于 CustomerID 为 1 的客户，生成一个金额为 $100 的订单。 验证该客户的 YTDOrders 金额是否为 100 以及 YTDSales 金额是否为零。  
  
-   uspFillOrder - 对于 CustomerID 为 1 的客户，生成一个金额为 $50 的订单。 填充该订单。 验证 YTDOrders 和 YTDSales 金额是否都为 50。  
  
-   uspShowOrderDetails - 对于 CustomerID 为 1 的客户，生成金额为 $100、$50 和 $5 的订单。 验证 uspShowOrderDetails 是否返回正确的列数以及结果集是否具有预期校验和。  
  
> [!NOTE]  
> 为获得一组完整的 SQL Server 单元测试，通常需要验证其他列的设置是否正确。 为了将本演练的大小控制在可管理的范围，本演练不描述如何验证 uspCancelOrder 的行为。  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspnewcustomer"></a>为 uspNewCustomer 编写 SQL Server 单元测试  
  
1.  在 SQL Server 单元测试设计器的导航栏中，单击 Sales_uspNewCustomerTest，并确保在相邻列表中突出显示“测试”。  
  
    在执行上一步后，可以为单元测试中的测试操作创建测试脚本。  
  
2.  在 Transact\-SQL 编辑器中更新 Transact\-SQL 语句，使其与以下语句相匹配：  
  
    ```  
    -- ssNoVersion unit test for Sales.uspNewCustomer  
    DECLARE @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @CustomerName = 'Fictitious Customer';  
  
    EXECUTE @RC = [Sales].[uspNewCustomer] @CustomerName;  
  
    SELECT * FROM [Sales].[Customer];  
    ```  
  
3.  在“测试条件”窗格中，单击“无结论”测试条件，然后单击“删除测试条件”图标（红色的 X）。  
  
4.  在“测试条件”窗格中，单击列表中的“行计数”，然后单击“添加测试条件”图标（绿色的 +）。  
  
5.  打开“属性”窗口（选择测试条件并且按下 F4），然后将“行计数”属性设置为 1。  
  
6.  在“文件”  菜单上，单击“全部保存” 。  
  
    接下来，为 uspPlaceNewOrder 定义单元测试逻辑。  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspplaceneworder"></a>为 uspPlaceNewOrder 编写 SQL Server 单元测试  
  
1.  在 SQL Server 单元测试设计器的导航栏中，单击 Sales_uspPlaceNewOrderTest，并确保在相邻列表中突出显示“测试”。  
  
    在执行此步骤后，可以为单元测试中的测试操作创建测试脚本。  
  
2.  在 Transact\-SQL 编辑器中更新 Transact\-SQL 语句，使其与以下语句相匹配：  
  
    ```  
    -- ssNoVersion unit test for Sales.uspPlaceNewOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- place an order for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, @Amount, @OrderDate, @Status;  
  
    -- verify that the YTDOrders value is correct.  
    SELECT @RC = [YTDOrders] FROM [Sales].[Customer] WHERE [CustomerID] = @CustomerID  
  
    SELECT @RC AS RC  
    ```  
  
3.  在 **“测试条件”** 窗格中，单击“无结论”测试条件，然后单击 **“删除测试条件”** 。  
  
4.  在 **“测试条件”** 窗格中，单击列表中的 **“标量值”** ，然后单击 **“添加测试条件”** 。  
  
5.  在 **“属性”** 窗口中，将 **“所需的值”** 属性设置为 100。  
  
6.  在 SQL Server 单元测试设计器的导航栏中，单击 Sales_uspPlaceNewOrderTest，并确保在相邻列表中突出显示“预先测试”。  
  
    在执行此步骤后，可以指定一些语句，以便将数据置于执行测试所需的状态。 对于此示例，您必须先创建 Customer 记录，然后才能下订单。  
  
7.  单击“单击此处以创建”以创建预先测试脚本。  
  
8.  在 Transact\-SQL 编辑器中更新 Transact\-SQL 语句，使其与以下语句相匹配：  
  
    ```  
    /*  
    Add Transact-SQL statements here that you want to run before  
    the test script is run.  
    */  
    -- Add a customer for this test with the name 'Fictitious Customer'  
    DECLARE @NewCustomerID AS INT, @CustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
       @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
    ```  
  
9. 在“文件”  菜单上，单击“全部保存” 。  
  
    接下来，为 uspFillOrder 创建单元测试。  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspfillorder"></a>为 uspFillOrder 编写 SQL Server 单元测试  
  
1.  在 SQL Server 单元测试设计器的导航栏中，单击 Sales_uspFillOrderTest，并确保在相邻列表中突出显示“测试”。  
  
    在执行此步骤后，可以为单元测试中的测试操作创建测试脚本。  
  
2.  在 Transact\-SQL 编辑器中更新 Transact\-SQL 语句，使其与以下语句相匹配：  
  
    ```  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @FilledDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
    -- Get the most recently added order.  
    SELECT @OrderID = MAX([OrderID]) FROM [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
  
    -- fill an order for that customer  
    EXECUTE @RC = [Sales].[uspFillOrder] @OrderID, @FilledDate;  
  
    -- verify that the YTDOrders value is correct.  
    SELECT @RC = [YTDSales] FROM [Sales].[Customer] WHERE [CustomerID] = @CustomerID  
  
    SELECT @RC AS RC;  
    ```  
  
3.  在 **“测试条件”** 窗格中，单击“无结论”测试条件，然后单击 **“删除测试条件”** 。  
  
4.  在 **“测试条件”** 窗格中，单击列表中的 **“标量值”** ，然后单击 **“添加测试条件”** 。  
  
5.  在 **“属性”** 窗口中，将 **“所需的值”** 属性设置为 100。  
  
6.  在 SQL Server 单元测试设计器的导航栏中，单击 Sales_uspFillOrderTest，并确保在相邻列表中突出显示“预先测试”。 在执行此步骤后，可以指定一些语句，以便将数据置于执行测试所需的状态。 对于此示例，您必须先创建 Customer 记录，然后才能下订单。  
  
7.  单击“单击此处以创建”以创建预先测试脚本。  
  
8.  在 Transact\-SQL 编辑器中更新 Transact\-SQL 语句，使其与以下语句相匹配：  
  
    ```  
    /*  
    Add Transact-SQL statements here that you want to run before  
    the test script is run.  
    */  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'CustomerB'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place an order for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, @Amount, @OrderDate, @Status;  
  
    COMMIT TRANSACTION  
    ```  
  
9. 在“文件”  菜单上，单击“全部保存” 。  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspshoworderdetails"></a>为 uspShowOrderDetails 编写 SQL Server 单元测试  
  
1.  在 SQL Server 单元测试设计器的导航栏中，单击 Sales_uspShowOrderDetailsTest，并确保在相邻列表中突出显示“测试”。  
  
    在执行此步骤后，可以为单元测试中的测试操作创建测试脚本。  
  
2.  在 Transact\-SQL 编辑器中更新 Transact\-SQL 语句，使其与以下语句相匹配：  
  
    ```  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @FilledDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- fill an order for that customer  
    EXECUTE @RC = [Sales].[uspShowOrderDetails] @CustomerID;  
  
    SELECT @RC AS RC;  
    ```  
  
3.  在 **“测试条件”** 窗格中，单击“无结论”测试条件，然后单击 **“删除测试条件”** 。  
  
4.  在 **“测试条件”** 窗格中，单击列表中的 **“所需的架构”** ，然后单击 **“添加测试条件”** 。  
  
5.  在“属性”窗口中的“配置”属性中，单击浏览按钮（“…”）  。  
  
6.  在 **“expectedSchemaCondition1 的配置”** 对话框中，指定与您的数据库的连接。 例如，如果使用了默认部署位置（即 LocalDB），应单击“新建连接”来指定 (LocalDB)\Projects。 然后，选择该数据库的名称。  
  
7.  单击“检索” 。 （如有必要，单击“检索”直到看到数据。）  
  
    将执行单元测试的 Transact\-SQL 主体，并且生成的架构将显示在对话框中。 因为没有执行预先测试代码，所有不返回任何数据。 因为您只验证架构，不验证数据，因此不返回数据是允许的。  
  
8.  单击“确定”。  
  
    所需的架构与测试条件存储在一起。  
  
9. 在 SQL Server 单元测试设计器的导航栏中，单击 Sales_uspShowOrderDetailsTest，并确保在相邻列表中突出显示“预先测试”。 在执行此步骤后，可以指定一些语句，以便将数据置于执行测试所需的状态。 对于此示例，您必须先创建 Customer 记录，然后才能下订单。  
  
10. 单击“单击此处以创建”以创建预先测试脚本。  
  
11. 在 Transact\-SQL 编辑器中更新 Transact\-SQL 语句，使其与以下语句相匹配：  
  
    ```  
    /*  
    Add Transact-SQL statements here to run before the test script is run.  
    */  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'FictitiousCustomer'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place 3 orders for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 100, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 50, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 5, @OrderDate, @Status;  
  
    COMMIT TRANSACTION  
    ```  
  
12. 在 SQL Server 单元测试设计器的导航栏中，单击 Sales_uspShowOrderDetailsTest，然后单击相邻列表中的“测试”。  
  
    您必须执行此操作，因为您要将校验和条件应用于测试，而不是预先测试。  
  
13. 在 **“测试条件”** 窗格中，单击列表中的 **“数据校验和”** ，然后单击 **“添加测试条件”** 。  
  
14. 在“属性”窗口中的“配置”属性中，单击浏览按钮（“…”）  。  
  
15. 在 **“checksumCondition1 的配置”** 对话框中，指定与您的数据库的连接。  
  
16. 使用以下代码替换对话框（在“编辑连接”按钮下）中的 Transact\-SQL：  
  
    ```  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'CustomerB'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @Status AS CHAR (1);  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @OrderDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place 3 orders for that customer  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 100, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 50, @OrderDate, @Status;  
    EXECUTE @RC = [Sales].[uspPlaceNewOrder] @CustomerID, 5, @OrderDate, @Status;  
  
    COMMIT TRANSACTION  
  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @FilledDate AS DATETIME;  
    DECLARE @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- fill an order for that customer  
    EXECUTE @RC = [Sales].[uspShowOrderDetails] @CustomerID;  
  
    SELECT @RC AS RC;  
    ```  
  
    此代码将预先测试中的 Transact\-SQL 代码与测试本身中的 Transact\-SQL 结合在一起。 您同时需要这两种代码，以便返回与运行测试时返回结果相同的结果。  
  
17. 单击“检索” 。 （如有必要，单击“检索”直到看到数据。）  
  
    将执行指定的 Transact\-SQL ，并且将针对返回的数据计算校验和。  
  
18. 单击“确定”。  
  
    计算出的校验和与测试条件存储在一起。 所需的校验和将显示在“数据校验和”测试条件的“值”列中。  
  
19. 在“文件”  菜单上，单击“全部保存” 。  
  
    此时，您已准备好运行测试。  
  
## <a name="run-sql-server-unit-tests"></a><a name="RunTests"></a>运行 SQL Server 单元测试  
  
#### <a name="to-run-the-sql-server-unit-tests"></a>运行 SQL Server 单元测试  
  
1.  在“测试”菜单上，指向“窗口”，然后单击“测试视图”（在 Visual Studio 2010 中）或“测试资源管理器”（在 Visual Studio 2012 中）。  
  
2.  在“测试视图”窗口 (Visual Studio 2010) 中，单击工具栏上的“刷新”以更新测试列表。 若要在“测试资源管理器”(Visual Studio 2012) 中查看测试列表，请生成解决方案。  
  
    “测试视图”或“测试资源管理器”窗口中列出了在本演练前面创建的、并在其中添加了 Transact\-SQL 语句和测试条件的测试。 名为 TestMethod1 的测试是空的，在本演练中将不使用它。  
  
3.  右键单击“Sales_uspNewCustomerTest”，然后单击“运行选定内容”。  
  
    Visual Studio 使用指定的特权上下文连接到数据库并应用数据生成计划。 Visual Studio 先切换到执行上下文，再在测试中运行 Transact\-SQL 脚本。 最后，Visual Studio 将针对在测试条件中指定的内容评估 Transact\-SQL 脚本的结果，并在“测试结果”窗口中显示结果（通过或失败）。  
  
4.  在 **“测试结果”** 窗口中查看结果。  
  
    测试通过，这意味着 **SELECT** 语句将在运行时返回一行。  
  
5.  为 Sales_uspPlaceNewOrderTest、Sales_uspFillOrderTest 和 Sales_uspShowOrderDetailsTest 测试重复步骤 3。 结果应如下所示：  
  
    |测试|预期结果|  
    |--------|-------------------|  
    |Sales_uspPlaceNewOrderTest|通过|  
    |Sales_uspShowOrderDetailsTest|通过|  
    |Sales_uspFillOrderTest|失败并出现以下错误：“ScalarValueCondition Condition (scalarValueCondition2) Failed:ResultSet 1 Row 1 Column 1: values do not match, actual '-100' expected '100'.”出现此错误的原因是存储过程的定义包含微小错误。|  
  
    接下来，您将更正该错误并重新运行测试。  
  
#### <a name="to-correct-the-error-in-salesuspfillorder"></a>更正 Sales.uspFillOrder 中的错误  
  
1.  在数据库的“SQL Server 对象资源管理器”项目节点中，双击 uspFillOrder 存储过程以在 Transact\-SQL 编辑器中打开其定义。  
  
2.  在定义中，找到以下 Transact\-SQL 语句：  
  
    ```  
    UPDATE [Sales].[Customer]  
       SET  
       YTDSales = YTDSales - @Delta  
        WHERE [CustomerID] = @CustomerID  
    ```  
  
3.  更改此语句中的 SET 子句，使其与以下语句相匹配：  
  
    ```  
    UPDATE [Sales].[Customer]  
       SET  
       YTDSales = YTDSales + @Delta  
        WHERE [CustomerID] = @CustomerID  
    ```  
  
4.  在 **“文件”** 菜单上，单击 **“保存 uspFillOrder.sql”** 。  
  
5.  在“测试视图”中，右键单击“Sales_uspFillOrderTest”，然后单击“运行选定内容”。  
  
    测试通过。  
  
## <a name="add-a-negative-unit-test"></a><a name="NegativeTest"></a>添加负面单元测试  
您可以创建负面测试来验证测试在应失败时是否失败。 例如，如果您尝试取消已填充的订单，则该测试应失败。 在本演练的此部分，您将为 Sales.uspCancelOrder 存储过程创建一个负面单元测试。  
  
若要创建和验证负面测试，您必须执行以下任务：  
  
-   更新存储过程以测试失败条件  
  
-   定义新单元测试  
  
-   修改单元测试的代码以指示它应失败  
  
-   运行单元测试  
  
#### <a name="to-update-the-stored-procedure"></a>更新存储过程  
  
1.  在 SimpleUnitTestDB 数据库的“SQL Server 对象资源管理器”项目节点中，展开“可编程性”节点，再展开“存储过程”节点，然后双击 uspCancelOrder。  
  
2.  在 Transact\-SQL 编辑器中，更新过程定义，使其与以下代码相匹配：  
  
    ```  
    CREATE PROCEDURE [Sales].[uspCancelOrder]  
    @OrderID INT  
    AS  
    BEGIN  
        DECLARE @Delta INT, @CustomerID INT, @PriorStatus CHAR(1)  
        BEGIN TRANSACTION  
            BEGIN TRY  
                IF (NOT EXISTS(SELECT [CustomerID] from [Sales].[Orders] WHERE [OrderID] = @OrderID))  
                BEGIN  
                    -- Specify WITH LOG option so that the error is  
                    -- written to the application log.  
                    RAISERROR( 'That order does not exist.', -- Message text  
                               16, -- severity  
                                1 -- state  
                            ) WITH LOG;  
                END  
  
                SELECT @Delta = [Amount], @CustomerID = [CustomerID], @PriorStatus = [Status]  
                 FROM [Sales].[Orders] WHERE [OrderID] = @OrderID  
  
                IF @PriorStatus <> 'O'   
                BEGIN  
                    -- Specify WITH LOG option so that the error is  
                    -- written to the application log.  
                    RAISERROR ( 'You can only cancel open orders.', -- Message text  
                                16, -- Severity  
                                1 -- State  
                                ) WITH LOG;  
                END  
                ELSE  
                BEGIN  
                    -- If we make it to here, then we can cancel the order. Update the status to 'X' first...  
                    UPDATE [Sales].[Orders]  
                       SET [Status] = 'X'  
                    WHERE [OrderID] = @OrderID  
                    -- and then remove the amount from the YTDOrders for the customer  
                    UPDATE [Sales].[Customer]  
                           SET  
                               YTDOrders = YTDOrders - @Delta  
                    WHERE [CustomerID] = @CustomerID  
                    COMMIT TRANSACTION  
                    RETURN 1; -- indicate success  
                END  
            END TRY  
            BEGIN CATCH  
                DECLARE @ErrorMessage NVARCHAR(4000);  
                DECLARE @ErrorSeverity INT;  
                DECLARE @ErrorState INT;  
  
                SELECT @ErrorMessage = ERROR_MESSAGE(),  
                       @ErrorSeverity = ERROR_SEVERITY(),  
                       @ErrorState = ERROR_STATE();  
  
                ROLLBACK TRANSACTION  
                -- Use RAISERROR inside the CATCH block to return  
                -- error information about the original error that  
                -- caused execution to jump to the CATCH block.  
                RAISERROR (@ErrorMessage, -- Mesasge text  
                           @ErrorSeverity, -- Severity  
                           @ErrorState -- State  
                          );  
                RETURN 0; -- indicate failure  
            END CATCH;  
    END  
    ```  
  
3.  在 **“文件”** 菜单上，单击 **“保存 uspCancelOrder.sql”** 。  
  
4.  按 F5 部署 **SimpleUnitTestDB**。  
  
    将更新部署到 uspCancelOrder 存储过程。 您没有更改任何其他对象，因此仅更新了该存储过程。  
  
    接下来，为此过程定义相关单元测试。  
  
#### <a name="to-write-the-sql-server-unit-test-for-uspcancelorder"></a>为 uspCancelOrder 编写 SQL Server 单元测试  
  
1.  在 SQL Server 单元测试设计器的导航栏中，单击 Sales_uspCancelOrderTest，并确保在相邻列表中突出显示“测试”。  
  
    在执行此步骤后，可以为单元测试中的测试操作创建测试脚本。  
  
2.  在 Transact\-SQL 编辑器中更新 Transact\-SQL 语句，使其与以下语句相匹配：  
  
    ```  
    -- ssNoVersion unit test for Sales.uspFillOrder  
    DECLARE @RC AS INT, @CustomerID AS INT, @Amount AS INT, @FilledDate AS DATETIME, @Status AS CHAR (1);  
    DECLARE @CustomerName AS NVARCHAR(40), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
           @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
    -- Get the most recently added order.  
    SELECT @OrderID = MAX([OrderID]) FROM [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
  
    -- try to cancel an order for that customer that has already been filled  
    EXECUTE @RC = [Sales].[uspCancelOrder] @OrderID;  
  
    SELECT @RC AS RC;  
    ```  
  
3.  在 **“测试条件”** 窗格中，单击“无结论”测试条件，然后单击 **“删除测试条件”** 图标。  
  
4.  在 **“测试条件”** 窗格中，单击列表中的 **“标量值”** ，然后单击 **“添加测试条件”** 图标。  
  
5.  在“属性”  窗口中，将“预期值”  属性设为 0。  
  
6.  在 SQL Server 单元测试设计器的导航栏中，单击 Sales_uspCancelOrderTest，并确保在相邻列表中突出显示“预先测试”。 在执行此步骤后，可以指定一些语句，以便将数据置于执行测试所需的状态。 对于此示例，您必须先创建 Customer 记录，然后才能下订单。  
  
7.  单击“单击此处以创建”以创建预先测试脚本。  
  
8.  在 Transact\-SQL 编辑器中更新 Transact\-SQL 语句，使其与以下语句相匹配：  
  
    ```  
    /*  
    Add Transact-SQL statements here to run before the test script is run.  
    */  
    BEGIN TRANSACTION  
  
    -- Add a customer for this test with the name 'CustomerB'  
    DECLARE @NewCustomerID AS INT, @RC AS INT, @CustomerName AS NVARCHAR (40);  
  
    SELECT @RC = 0,  
           @NewCustomerID = 0,  
           @CustomerName = N'Fictitious Customer';  
  
    IF NOT EXISTS(SELECT * FROM [Sales].[Customer] WHERE CustomerName = @CustomerName)  
    BEGIN  
    EXECUTE @NewCustomerID = [Sales].[uspNewCustomer] @CustomerName;  
    END  
  
    DECLARE @CustomerID AS INT, @Amount AS INT, @OrderDate AS DATETIME, @FilledDate AS DATETIME, @Status AS CHAR (1), @OrderID AS INT;  
  
    SELECT @RC = 0,  
           @CustomerID = 0,  
       @OrderID = 0,  
           @CustomerName = N'Fictitious Customer',  
           @Amount = 100,  
           @OrderDate = getdate(),  
       @FilledDate = getdate(),  
           @Status = 'O';  
  
    -- NOTE: Assumes that you inserted a Customer record with CustomerName='Fictitious Customer' in the pre-test script.  
    SELECT @CustomerID = [CustomerID] FROM [Sales].[Customer] WHERE [CustomerName] = @CustomerName;  
  
    -- delete any old records in the Orders table and clear out the YTD Sales/Orders fields  
    DELETE from [Sales].[Orders] WHERE [CustomerID] = @CustomerID;  
    UPDATE [Sales].[Customer] SET YTDOrders = 0, YTDSales = 0 WHERE [CustomerID] = @CustomerID;  
  
    -- place an order for that customer  
    EXECUTE @OrderID = [Sales].[uspPlaceNewOrder] @CustomerID, @Amount, @OrderDate, @Status;  
  
    -- fill the order for that customer  
    EXECUTE @RC = [Sales].[uspFillOrder] @OrderID, @FilledDate;  
  
    COMMIT TRANSACTION  
    ```  
  
9. 在“文件”  菜单上，单击“全部保存” 。  
  
    此时，您已准备好运行测试。  
  
#### <a name="to-run-the-sql-server-unit-tests"></a>运行 SQL Server 单元测试  
  
1.  在“测试视图”中，右键单击“Sales_uspCancelOrderTest”，然后单击“运行选定内容”。  
  
2.  在 **“测试结果”** 窗口中查看结果。  
  
    测试失败并显示以下错误：  
  
    **测试方法 TestProject1.SqlServerUnitTests1.Sales_uspCancelOrderTest 引发了异常：System.Data.SqlClient.SqlException：只能取消未结订单。**  
  
    接下来，修改代码以指示应出现异常。  
  
#### <a name="to-modify-the-code-for-the-unit-test"></a>修改单元测试的代码  
  
1.  在“解决方案资源管理器”中，展开 TestProject1，右键单击 SqlServerUnitTests1.cs，然后单击“查看代码”。  
  
2.  在代码编辑器中，导航至 Sales_uspCancelOrderTest 方法。 修改此方法的属性，使其与以下代码相匹配：  
  
    ```  
    [TestMethod(), ExpectedSqlException(Severity=16, MatchFirstError=false, State=1)]  
    public void Sales_uspCancelOrderTest()  
    ```  
  
    您指定希望看到特定异常。 您可以选择指定特定错误编号。 如果不添加此属性，单元测试将失败，并且“测试结果”窗口中将显示一则消息  
  
    > [!IMPORTANT]  
    > 目前，Visual Studio 2012 不支持 ExpectedSqlException 属性。 有关对此进行演练的信息，请参阅 [无法运行“预期的失败”数据库单元测试](https://social.msdn.microsoft.com/Forums/en-US/e74e06ad-e3c9-4cb0-97ad-a6f235a52345/unable-to-run-quotexpected-failurequot-database-unit-test)。  
  
3.  在“文件”菜单上，单击“保存 SqlServerUnitTests1.cs”。  
  
    接下来，重新运行单元测试以验证它是否按预期失败。  
  
#### <a name="to-re-run-the-sql-server-unit-tests"></a>重新运行 SQL Server 单元测试  
  
1.  在“测试视图”中，右键单击“Sales_uspCancelOrderTest”，然后单击“运行选定内容”。  
  
2.  在 **“测试结果”** 窗口中查看结果。  
  
    测试通过，这意味着过程在应失败时失败。  
  
## <a name="next-steps"></a>后续步骤  
在典型项目中，可以定义其他单元测试以验证所有关键数据库对象能否正常运行。 在完成该组测试后，可以将这些测试签入版本控制中以与团队成员共享它们。  
  
在建立基准后，可以创建和修改数据库对象，然后创建相关测试以验证所做更改是否会中断预期行为。  
  
## <a name="see-also"></a>另请参阅  
[创建和定义 SQL Server 单元测试](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[使用 SQL Server 单元测试验证数据库代码](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
[如何：创建空的 SQL Server 单元测试](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)  
[如何：配置 SQL Server 单元测试执行](../ssdt/how-to-configure-sql-server-unit-test-execution.md)  
