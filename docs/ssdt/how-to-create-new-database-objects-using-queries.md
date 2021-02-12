---
title: 使用查询创建新的数据库对象
description: 熟悉 Transact-SQL 编辑器。 了解如何打开此编辑器，并查看演示如何使用它创建新表、函数或视图的示例。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: ac983ac7-f9c4-495d-8a99-e1ba370fb271
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: c7afdd34756511df443c472eb6b80280fea32d96
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018297"
---
# <a name="how-to-create-new-database-objects-using-queries"></a>如何：使用查询创建新的数据库对象

如果更喜欢使用脚本来创建或编辑视图、存储过程、函数、触发器或用户定义的类型，则可以使用 Transact\-SQL 编辑器。 Transact\-SQL 编辑器提供 IntelliSense 和其他语言支持。 有关详细信息，请参阅[使用 Transact-SQL 编辑器编辑和执行脚本](../ssdt/use-transact-sql-editor-to-edit-and-execute-scripts.md)。  
  
在使用“查看代码”上下文菜单在连接的数据库或项目中打开某一数据库实体时，将调用 Transact\-SQL 编辑器。 在从 SQL Server 对象资源管理器使用“新建查询”上下文菜单或者向数据库项目添加新的脚本对象时，该编辑器也自动打开。 如果没有连接到某一数据库，但想要对其执行查询，则也可以使用“新建查询连接”对话框，方法是：从“SQL”菜单中选择“Transact-SQL 编辑器”菜单以连接到某一数据库，然后启动 Transact\-SQL 编辑器。  
  
> [!WARNING]  
> 下面的过程利用在[连接的数据库开发](../ssdt/connected-database-development.md)一节的前面的过程中创建的实体。  
  
### <a name="to-create-a-new-table-using-a-transact-sql-query"></a>使用 Transact\-SQL 查询创建一个新表  
  
1.  右键单击 Trade 数据库节点并选择“新建查询”。  
  
2.  在脚本窗格中，粘贴以下代码：  
  
    ```  
  
    CREATE TABLE [dbo].[Fruits] (  
        [Id]         INT NOT NULL,  
        [Perishable] BIT DEFAULT ((1)) NULL,  
        PRIMARY KEY CLUSTERED ([Id] ASC),  
        FOREIGN KEY ([Id]) REFERENCES [dbo].[Products] ([Id])   
    );  
    ```  
  
3.  单击 Transact\-SQL 编辑器工具栏中的“执行查询”按钮以便运行此查询。  
  
4.  右键单击“SQL Server 对象资源管理器”中的“Trade”数据库，然后选择“刷新”。 请注意，新的 Fruits 表已添加到该数据库中。  
  
### <a name="to-create-a-new-function"></a>创建新的函数  
  
1.  使用以下代码替换当前 Transact\-SQL 编辑器中的代码：  
  
    ```  
  
    CREATE FUNCTION [dbo].GetProductsBySupplier  
    (  
    @SupplierId int  
    )  
    RETURNS @returntable TABLE   
    (  
    [Id] int NOT NULL,   
    [Name] NVARCHAR (128) NOT NULL,  
    [Shelflife] INT NOT NULL,  
    [SupplierId] INT NOT NULL,  
    [CustomerId] INT NOT NULL  
    )  
    AS  
    BEGIN  
    INSERT @returntable  
    SELECT *  from Products p  
    where p.SupplierId = @SupplierId  
    RETURN   
    END  
    ```  
  
    此函数将返回 `Products` 表中其 `SupplierId` 等于指定参数的所有行。 单击 Transact\-SQL 编辑器工具栏中的“执行查询”按钮以便运行此查询。  
  
2.  在 SQL Server 对象资源管理器中的 Trade 节点下，展开“可编程性”和“函数”节点。 可以在“表值函数”下找到刚创建的新函数。  
  
### <a name="to-create-a-new-view"></a>创建新的视图  
  
1.  使用以下代码替换当前 Transact\-SQL 编辑器中的代码。 然后单击编辑器上方的“执行查询”按钮以便运行此查询。  
  
    ```  
    CREATE VIEW [dbo].PerishableFruits   
    AS SELECT p.Id, p.Name FROM dbo.Products p  
    join dbo.Fruits f on f.Id = p.Id  
    where f.Perishable = 1  
    ```  
  
2.  在 SQL Server 对象资源管理器中的 Trade 节点下，展开“视图”节点以便找到刚创建的新视图。  
  
## <a name="see-also"></a>另请参阅  
[管理表、关系和修复错误](../ssdt/manage-tables-relationships-and-fix-errors.md)  
[使用 Transact-SQL 编辑器编辑和执行脚本](../ssdt/use-transact-sql-editor-to-edit-and-execute-scripts.md)  
  
