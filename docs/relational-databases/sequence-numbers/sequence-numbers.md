---
description: 序列号
title: 序列号 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- sequence number object, overview
- sequence [Database Engine]
- autonumbers, sequences
- sequence numbers [SQL Server]
- sequence number object
ms.assetid: c900e30d-2fd3-4d5f-98ee-7832f37e79d1
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d29cae171cad8f0da3c53a708112167a0a17f749
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463058"
---
# <a name="sequence-numbers"></a>序列号
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  序列是一种用户定义的架构绑定对象，它根据创建该序列时采用的规范生成一组数值。 这组数值以定义的间隔按升序或降序生成，并且可根据要求循环（重复）。 序列不与表相关联，这一点与标识列不同。 应用程序将引用某一序列对象以便接收其下一个值。 序列与表之间的关系由应用程序控制。 用户应用程序可以引用某一序列对象并且跨多行和表协调值键。  
  
 序列是通过使用 **CREATE SEQUENCE** 语句独立于表来创建的。 其选项使您可以控制增量、最大值和最小值、起始点、自动重新开始功能和缓存以便改进性能。 有关这些选项的信息，请参阅 [CREATE SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md)。  
  
 与在插入行时生成的标识列值不同，应用程序可以通过调用 [NEXT VALUE FOR](../../t-sql/functions/next-value-for-transact-sql.md) 函数在插入行之前获取下一序列号。 在调用 NEXT VALUE FOR 时分配该序列号，即使在该序列号永远也不插入某个表中时也是如此。 此 NEXT VALUE FOR 函数可用作表定义中某个列的默认值。 使用 [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md) 可一次获取某个范围的多个序列号。  
  
 序列可定义为任何整数数据类型。 如果未指定数据类型，则序列将默认为 **bigint**。  
  
## <a name="using-sequences"></a>使用序列  
 在以下情况下将使用序列，而非标识列：  
  
-   应用程序要求在插入到表中之前有一个数值。  
  
-   应用程序要求在多个表之间或者某个表内的多个列之间共享单个数值系列。  
  
-   在达到指定的数值时，应用程序必须重新开始该数值系列。 例如，在分配值 1 到 10 后，应用程序再次开始分配值 1 到 10。  
  
-   应用程序要求序列值按其他字段排序。 NEXT VALUE FOR 函数可以将 OVER 子句应用于该函数调用。 OVER 子句确保返回的值按照 OVER 子句的 ORDER BY 子句的顺序生成。  
  
-   应用程序要求同时分配多个数值。 例如，应用程序需要保留五个序号。 如果正在同时向其他进程发出数值，则请求标识值可能会导致在系列中出现间断。 调用 sp_sequence_get_range 可以一次检索该序列中的若干数值。  
  
-   您需要更改序列的规范，例如增量值。  
  
## <a name="limitations"></a>限制  
 与不能更改其值的标识列不同，在插入到表后不自动保护序列值。 若要防止更改序列值，请对表使用更新触发器以便回滚更改。  
  
 对于序列值不自动强制唯一性。 按照设计能够重复使用序列值。 如果某个表中的序列值要求唯一，则对列创建唯一索引。 如果要求表中的序列值在一组表之间唯一，则创建触发器以免更新语句或序列号循环导致的重复项。  
  
 序列对象根据其定义生成数值，但序列对象不控制生成数值的方式。 在回滚事务时、在某个序列对象由多个表共享时或者在分配序列号且不在多个表中使用它们时，插入到表中的序列号可能具有间断。 当使用 CACHE 选项创建时，意外关机（如电源故障）可能导致缓存中的序列号丢失。  
  
 如果在单个 **NEXT VALUE FOR** 语句中有多个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数的实例指定同一序列生成器，则所有这些实例返回该 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句独立于表来创建的。 此行为与 ANSI 标准保持一致。  
 
 序列号在当前事务的作用域之外生成。 无论提交还是回滚使用序列号的事务，都会占用序列号。 只有在记录被完全填充后，才会发生重复验证。 在某些情况下，这可能会导致在创建过程中将相同数字用于多个记录，但随后被识别为重复。 如果发生这种情况，且其他自动编号值已应用于后续记录，这可能会导致自动编号值之间存在差距。
  
## <a name="typical-use"></a>典型用法  
 若要创建从 -2,147,483,648 到 2,147,483,647 且增量为 1 的整数序列号，请使用以下语句。  
  
```  
CREATE SEQUENCE Schema.SequenceName  
    AS int  
    INCREMENT BY 1 ;  
```  
  
 若要创建类似于从 1 到 2,147,483,647 且增量为 1 的标识列的整数序列号，请使用以下语句。  
  
```  
CREATE SEQUENCE Schema.SequenceName  
    AS int  
    START WITH 1  
    INCREMENT BY 1 ;  
  
```  
  
## <a name="managing-sequences"></a>管理序列  
 有关序列的信息，请查询 [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md)。  
  
## <a name="examples"></a>示例  
 请在 [CREATE SEQUENCE (Transact-SQL)](../../t-sql/statements/create-sequence-transact-sql.md)、[NEXT VALUE FOR (Transact-SQL)](../../t-sql/functions/next-value-for-transact-sql.md) 和 [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md) 主题查看其他示例。  
  
### <a name="a-using-a-sequence-number-in-a-single-table"></a>A. 在单个表中使用序列号  
 下面的示例创建一个名为 Test 的架构、一个名为 Orders 的表以及一个名为 CountBy1 的序列，然后使用 NEXT VALUE FOR 函数将行插入到该表中。  
  
```  
--Create the Test schema  
CREATE SCHEMA Test ;  
GO  
  
-- Create a table  
CREATE TABLE Test.Orders  
    (OrderID int PRIMARY KEY,  
    Name varchar(20) NOT NULL,  
    Qty int NOT NULL);  
GO  
  
-- Create a sequence  
CREATE SEQUENCE Test.CountBy1  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
  
-- Insert three records  
INSERT Test.Orders (OrderID, Name, Qty)  
    VALUES (NEXT VALUE FOR Test.CountBy1, 'Tire', 2) ;  
INSERT test.Orders (OrderID, Name, Qty)  
    VALUES (NEXT VALUE FOR Test.CountBy1, 'Seat', 1) ;  
INSERT test.Orders (OrderID, Name, Qty)  
    VALUES (NEXT VALUE FOR Test.CountBy1, 'Brake', 1) ;  
GO  
  
-- View the table  
SELECT * FROM Test.Orders ;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `OrderID  Name    Qty`  
  
 `1        Tire    2`  
  
 `2        Seat    1`  
  
 `3        Brake   1`  
  
### <a name="b-calling-next-value-for-before-inserting-a-row"></a>B. 在插入某一行之前调用 NEXT VALUE FOR  
 下面的示例通过使用在示例 A 中创建的 `Orders` 表，声明一个名为 `@nextID`的变量，然后使用 NEXT VALUE FOR 函数将该变量设置为下一个可用的序列号。 假定应用程序对订单执行某种处理，例如向客户提供其潜在订单的 `OrderID` 号，然后验证该订单。 无论这一处理时间有多长，或者在这个处理过程中添加了多少其他订单，原始编号都保留供此连接使用。 最后， `INSERT` 语句将该订单添加到 `Orders` 表。  
  
```  
DECLARE @NextID int ;  
SET @NextID = NEXT VALUE FOR Test.CountBy1;  
-- Some work happens  
INSERT Test.Orders (OrderID, Name, Qty)  
    VALUES (@NextID, 'Rim', 2) ;  
GO  
  
```  
  
### <a name="c-using-a-sequence-number-in-multiple-tables"></a>C. 在多个表中使用序列号  
 此示例假定一个生产线监视进程接收在车间中发生的事件的通知。 每个事件都接收一个唯一且单调递增的 `EventID` 号。 所有事件都使用相同的 `EventID` 序列号，因此，汇总了所有事件的报表可唯一标识各事件。 但是，事件数据根据事件的类型存储于三个不同的表中。 该代码示例创建一个名为 `Audit`的架构、一个名为 `EventCounter`的序列以及三个表，这三个表都使用 `EventCounter` 序列作为默认值。 然后，该示例向这三个表添加行并且查询结果。  
  
```  
CREATE SCHEMA Audit ;  
GO  
CREATE SEQUENCE Audit.EventCounter  
    AS int  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
  
CREATE TABLE Audit.ProcessEvents  
(  
    EventID int PRIMARY KEY CLUSTERED   
        DEFAULT (NEXT VALUE FOR Audit.EventCounter),  
    EventTime datetime NOT NULL DEFAULT (getdate()),  
    EventCode nvarchar(5) NOT NULL,  
    Description nvarchar(300) NULL  
) ;  
GO  
  
CREATE TABLE Audit.ErrorEvents  
(  
    EventID int PRIMARY KEY CLUSTERED  
        DEFAULT (NEXT VALUE FOR Audit.EventCounter),  
    EventTime datetime NOT NULL DEFAULT (getdate()),  
    EquipmentID int NULL,  
    ErrorNumber int NOT NULL,  
    EventDesc nvarchar(256) NULL  
) ;  
GO  
  
CREATE TABLE Audit.StartStopEvents  
(  
    EventID int PRIMARY KEY CLUSTERED  
        DEFAULT (NEXT VALUE FOR Audit.EventCounter),  
    EventTime datetime NOT NULL DEFAULT (getdate()),  
    EquipmentID int NOT NULL,  
    StartOrStop bit NOT NULL  
) ;  
GO  
  
INSERT Audit.StartStopEvents (EquipmentID, StartOrStop)   
    VALUES (248, 0) ;  
INSERT Audit.StartStopEvents (EquipmentID, StartOrStop)   
    VALUES (72, 0) ;  
INSERT Audit.ProcessEvents (EventCode, Description)   
    VALUES (2735,   
    'Clean room temperature 18 degrees C.') ;  
INSERT Audit.ProcessEvents (EventCode, Description)   
    VALUES (18, 'Spin rate threashold exceeded.') ;  
INSERT Audit.ErrorEvents (EquipmentID, ErrorNumber, EventDesc)   
    VALUES (248, 82, 'Feeder jam') ;  
INSERT Audit.StartStopEvents (EquipmentID, StartOrStop)   
    VALUES (248, 1) ;  
INSERT Audit.ProcessEvents (EventCode, Description)   
    VALUES (1841, 'Central feed in bypass mode.') ;  
-- The following statement combines all events, though not all fields.  
SELECT EventID, EventTime, Description FROM Audit.ProcessEvents   
UNION SELECT EventID, EventTime, EventDesc FROM Audit.ErrorEvents   
UNION SELECT EventID, EventTime,   
CASE StartOrStop   
    WHEN 0 THEN 'Start'   
    ELSE 'Stop'  
END   
FROM Audit.StartStopEvents  
ORDER BY EventID ;  
GO  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `EventID  EventTime                Description`  
  
 `1        2009-11-02 15:00:51.157  Start`  
  
 `2        2009-11-02 15:00:51.160  Start`  
  
 `3        2009-11-02 15:00:51.167  Clean room temperature 18 degrees C.`  
  
 `4        2009-11-02 15:00:51.167  Spin rate threshold exceeded.`  
  
 `5        2009-11-02 15:00:51.173  Feeder jam`  
  
 `6        2009-11-02 15:00:51.177  Stop`  
  
 `7        2009-11-02 15:00:51.180  Central feed in bypass mode.`  
  
### <a name="d-generating-repeating-sequence-numbers-in-a-result-set"></a>D. 在结果集中生成重复序列号  
 下面的示例演示序列号的两个功能：循环以及在 select 语句中使用 `NEXT VALUE FOR` 。  
  
```  
CREATE SEQUENCE CountBy5  
   AS tinyint  
    START WITH 1  
    INCREMENT BY 1  
    MINVALUE 1  
    MAXVALUE 5  
    CYCLE ;  
GO  
  
SELECT NEXT VALUE FOR CountBy5 AS SurveyGroup, Name FROM sys.objects ;  
GO  
```  
  
### <a name="e-generating-sequence-numbers-for-a-result-set-by-using-the-over-clause"></a>E. 通过使用 OVER 子句为结果集生成序列号  
 下面的示例使用 `OVER` 子句在其添加序列号列之前按 `Name` 对结果集进行排序。  
  
```  
USE AdventureWorks2012 ;  
GO  
  
CREATE SCHEMA Samples ;  
GO  
  
CREATE SEQUENCE Samples.IDLabel  
    AS tinyint  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
  
SELECT NEXT VALUE FOR Samples.IDLabel OVER (ORDER BY Name) AS NutID, ProductID, Name, ProductNumber FROM Production.Product  
WHERE Name LIKE '%nut%' ;  
```  
  
### <a name="f-resetting-the-sequence-number"></a>F. 重置序列号  
 示例 E 使用了前 79 个 `Samples.IDLabel` 序列号。 （您的版本的 `AdventureWorks2012` 可能会返回不同数目的结果。）执行以下语句以便使用接下来的 79 个序列号（80 到 158）。  
  
```  
SELECT NEXT VALUE FOR Samples.IDLabel OVER (ORDER BY Name) AS NutID, ProductID, Name, ProductNumber FROM Production.Product  
WHERE Name LIKE '%nut%' ;  
```  
  
 执行以下语句以便重新开始 `Samples.IDLabel` 序列。  
  
```  
ALTER SEQUENCE Samples.IDLabel  
RESTART WITH 1 ;  
```  
  
 再次执行 select 语句以便确认 `Samples.IDLabel` 序列以数字 1 开头。  
  
```  
SELECT NEXT VALUE FOR Samples.IDLabel OVER (ORDER BY Name) AS NutID, ProductID, Name, ProductNumber FROM Production.Product  
WHERE Name LIKE '%nut%' ;  
```  
  
### <a name="g-changing-a-table-from-identity-to-sequence"></a>G. 将表从标识更改为序列  
 下面的示例创建一个包含该示例的三行的架构和表。 然后，该示例添加一个新列并且删除旧列。  
  
```  
-- Create a schema  
CREATE SCHEMA Test ;  
GO  
  
-- Create a table  
CREATE TABLE Test.Department  
    (  
        DepartmentID smallint IDENTITY(1,1) NOT NULL,  
        Name nvarchar(100) NOT NULL,  
        GroupName nvarchar(100) NOT NULL  
    CONSTRAINT PK_Department_DepartmentID PRIMARY KEY CLUSTERED   
         (DepartmentID ASC)   
    ) ;  
GO  
  
-- Insert three rows into the table  
INSERT Test.Department(Name, GroupName)  
    VALUES ('Engineering', 'Research and Development');  
GO  
  
INSERT Test.Department(Name, GroupName)  
    VALUES ('Tool Design', 'Research and Development');  
GO  
  
INSERT Test.Department(Name, GroupName)  
    VALUES ('Sales', 'Sales and Marketing');  
GO  
  
-- View the table that will be changed  
SELECT * FROM Test.Department ;  
GO  
  
-- End of portion creating a sample table  
--------------------------------------------------------  
-- Add the new column that does not have the IDENTITY property  
ALTER TABLE Test.Department   
    ADD DepartmentIDNew smallint NULL  
GO  
  
-- Copy values from the old column to the new column  
UPDATE Test.Department  
    SET DepartmentIDNew = DepartmentID ;  
GO  
  
-- Drop the primary key constraint on the old column  
ALTER TABLE Test.Department  
    DROP CONSTRAINT [PK_Department_DepartmentID];  
-- Drop the old column  
ALTER TABLE Test.Department  
    DROP COLUMN DepartmentID ;  
GO  
  
-- Rename the new column to the old columns name  
EXEC sp_rename 'Test.Department.DepartmentIDNew',   
    'DepartmentID', 'COLUMN';  
GO  
  
-- Change the new column to NOT NULL  
ALTER TABLE Test.Department  
    ALTER COLUMN DepartmentID smallint NOT NULL ;  
-- Add the unique primary key constraint  
ALTER TABLE Test.Department  
    ADD CONSTRAINT PK_Department_DepartmentID PRIMARY KEY CLUSTERED   
         (DepartmentID ASC) ;  
-- Get the highest current value from the DepartmentID column   
-- and create a sequence to use with the column. (Returns 3.)  
SELECT MAX(DepartmentID) FROM Test.Department ;  
-- Use the next desired value (4) as the START WITH VALUE;  
CREATE SEQUENCE Test.DeptSeq  
    AS smallint  
    START WITH 4  
    INCREMENT BY 1 ;  
GO  
  
-- Add a default value for the DepartmentID column  
ALTER TABLE Test.Department  
    ADD CONSTRAINT DefSequence DEFAULT (NEXT VALUE FOR Test.DeptSeq)   
        FOR DepartmentID;  
GO  
  
-- View the result  
SELECT DepartmentID, Name, GroupName  
FROM Test.Department ;   
-- Test insert  
INSERT Test.Department (Name, GroupName)  
    VALUES ('Audit', 'Quality Assurance') ;  
GO  
  
-- View the result  
SELECT DepartmentID, Name, GroupName  
FROM Test.Department ;  
GO  
  
```  
  
 使用 `SELECT *` 的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句将这个新列作为最后一列接收，而非作为第一列接收。 如果这样做是不可接受的，则您必须创建全新的表，将数据移到该表中，然后针对这个新表重新创建权限。  
  
## <a name="related-content"></a>相关内容  
 [CREATE SEQUENCE (Transact-SQL)](../../t-sql/statements/create-sequence-transact-sql.md)  
  
 [ALTER SEQUENCE (Transact-SQL)](../../t-sql/statements/alter-sequence-transact-sql.md)  
  
 [DROP SEQUENCE (Transact-SQL)](../../t-sql/statements/drop-sequence-transact-sql.md)  
  
 [IDENTITY（属性）(Transact-SQL)](../../t-sql/statements/create-table-transact-sql-identity-property.md)  
  
  
