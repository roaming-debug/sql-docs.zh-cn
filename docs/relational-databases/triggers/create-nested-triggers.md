---
description: 创建嵌套触发器
title: 创建嵌套触发器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- recursive DML triggers [SQL Server]
- DML triggers, nested
- triggers [SQL Server], nested
- direct recursion [SQL Server]
- triggers [SQL Server], recursive
- DML triggers, recursive
- RECURSIVE_TRIGGERS option
- indirect recursion [SQL Server]
- nested DML triggers
ms.assetid: cd522dda-b4ab-41b8-82b0-02445bdba7af
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2709846194c14dc08653efa761edd6660620872a
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128649"
---
# <a name="create-nested-triggers"></a>创建嵌套触发器
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  当触发器执行启动其他触发器的操作时，DML 和 DDL 触发器都是嵌套触发器。 这些操作都可以启动其他触发器等。 DML 触发器和 DDL 触发器最多可以嵌套 32 层。 可以通过 **nested triggers** 服务器配置选项来控制是否可以嵌套 AFTER 触发器。 但不管此设置是什么，都可以嵌套 INSTEAD OF 触发器（只有 DML 触发器可以为 INSTEAD OF 触发器）。  
  
> [!NOTE]  
>  [!INCLUDE[tsql](../../includes/tsql-md.md)] 触发器中对托管代码的任何引用均计为 32 层嵌套限制中的一层。 从托管代码内部调用的方法不根据此限制进行计数。  
  
 如果允许使用嵌套触发器，且链中的一个触发器启动了一个无限循环，则将超出嵌套层限制，且触发器将终止。  
  
 可使用嵌套触发器执行一些有用的日常工作，如保存前一个触发器所影响行的一个备份副本。 例如，可以在 `PurchaseOrderDetail` 上创建一个触发器，以保存由 `PurchaseOrderDetail` 触发器所删除的 `delcascadetrig` 行的备份副本。 `delcascadetrig` 触发器有效时，从 `PurchaseOrderID` 中删除 `PurchaseOrderHeader` 1965 将删除 `PurchaseOrderDetail`中对应的行。 为了保存数据，可在 `PurchaseOrderDetail` 上创建一个 DELETE 触发器，该触发器可将删除的数据保存到另一个单独创建的表 ( `del_save`) 中。 例如：  
  
```  
CREATE TRIGGER Purchasing.savedel  
   ON Purchasing.PurchaseOrderDetail  
FOR DELETE  
AS  
   INSERT del_save;  
   SELECT * FROM deleted;  
```  
  
 建议不要按与顺序相关的序列使用嵌套触发器。 应使用单独的触发器级联数据修改。  
  
> [!NOTE]  
>  由于触发器在事务中执行，如果在一组嵌套触发器的任意层中发生错误，则整个事务都将取消，且所有的数据修改都将回滚。 在触发器中包含 PRINT 语句可以确定错误的发生位置。  
  
## <a name="recursive-triggers"></a>递归触发器  
 AFTER 触发器不会以递归方式自行调用，除非设置了 RECURSIVE_TRIGGERS 数据库选项。  
  
 有两种不同的递归方式：  
  
-   直接递归  
  
     在触发器触发并执行一个导致同一个触发器再次触发的操作时，将发生此递归。 例如，应用程序更新了表 **T3**，从而触发了触发器 **Trig3** 。 **Trig3** 再次更新表 **T3** ，从而再次触发了触发器 **Trig3** 。  
  
     调用其他类型的触发器（AFTER 或 INSTEAD OF）之后再次调用同一个触发器时，也会发生直接递归。 换言之，当同一个 INSTEAD OF 触发器被第二次调用时，即使在这两次调用之间调用了一个或多个 AFTER 触发器，也会发生 INSTEAD OF 触发器的直接递归。 同样，当同一个 AFTER 触发器被第二次调用时，即使在这两次调用之间调用了一个或多个 INSTEAD OF 触发器，也会发生 AFTER 触发器的直接递归。 例如，一个应用程序对表 **T4** 进行更新。 此更新将导致触发 INSTEAD OF 触发器 **Trig4** 。 **Trig4** 对表 **T5** 进行更新。 此更新将导致触发 AFTER 触发器 **Trig5** 。 **Trig5** 更新表 **T4**，此更新将导致再次触发 INSTEAD OF 触发器 **Trig4** 。 此事件链即被认为是 **Trig4** 的直接递归。  
  
-   间接递归  
  
     当触发器触发并执行导致触发相同类型的其他触发器（AFTER 或 INSTEAD OF）的操作时，会发生此类递归。 第二个触发器执行一个再次触发第一个触发器的操作。 换言之，当在这两次调用之间调用其他 INSTEAD OF 触发器之前第二次调用 INSTEAD OF 触发器，便会发生间接递归。 同样，当在这两次调用之间调用其他 AFTER 触发器之前第二次调用 AFTER 触发器，也会发生间接递归。 例如，一个应用程序对表 **T1** 进行更新。 此更新将导致触发 AFTER 触发器 **Trig1** 。 **Trig1** 更新表 **T2**，此更新将导致触发 AFTER 触发器 **Trig2** 。 **Trig2** 反过来更新表 **T1** ，从而导致再次触发 AFTER 触发器 **Trig1** 。  
  
 当 RECURSIVE_TRIGGERS 数据库选项设置为 OFF 时，仅阻止 AFTER 触发器的直接递归。 若要禁用 AFTER 触发器的间接递归，还必须将 **nested triggers** 服务器选项设置为 **0**。  
  
## <a name="examples"></a>示例  
 下面的示例中说明使用递归触发器来解决自引用关系（也称为传递闭包）。 例如，表 `emp_mgr` 定义了以下内容：  
  
-   一个公司中的雇员 (`emp`)。  
  
-   每个雇员的经理 (`mgr`)。  
  
-   组织树中向每个经理汇报的雇员总数 (`NoOfReports`)。  
  
 递归 UPDATE 触发器可以用于在插入新雇员记录时让 `NoOfReports` 列保持最新。 INSERT 触发器更新经理记录的 `NoOfReports` 列，而该操作递归更新管理层向上的其他记录的 `NoOfReports` 列。  
  
```  
USE AdventureWorks2012;  
GO  
-- Turn recursive triggers ON in the database.  
ALTER DATABASE AdventureWorks2012  
   SET RECURSIVE_TRIGGERS ON;  
GO  
CREATE TABLE dbo.emp_mgr (  
   emp char(30) PRIMARY KEY,  
    mgr char(30) NULL FOREIGN KEY REFERENCES emp_mgr(emp),  
    NoOfReports int DEFAULT 0  
);  
GO  
CREATE TRIGGER dbo.emp_mgrins ON dbo.emp_mgr  
FOR INSERT  
AS  
DECLARE @e char(30), @m char(30);  
DECLARE c1 CURSOR FOR  
   SELECT emp_mgr.emp  
   FROM   emp_mgr, inserted  
   WHERE emp_mgr.emp = inserted.mgr;  
  
OPEN c1;  
FETCH NEXT FROM c1 INTO @e;  
WHILE @@fetch_status = 0  
BEGIN  
   UPDATE dbo.emp_mgr  
   SET emp_mgr.NoOfReports = emp_mgr.NoOfReports + 1 -- Add 1 for newly  
   WHERE emp_mgr.emp = @e ;                           -- added employee.  
  
   FETCH NEXT FROM c1 INTO @e;  
END  
CLOSE c1;  
DEALLOCATE c1;  
GO  
-- This recursive UPDATE trigger works assuming:  
--   1. Only singleton updates on emp_mgr.  
--   2. No inserts in the middle of the org tree.  
CREATE TRIGGER dbo.emp_mgrupd ON dbo.emp_mgr FOR UPDATE  
AS  
IF UPDATE (mgr)  
BEGIN  
   UPDATE dbo.emp_mgr  
   SET emp_mgr.NoOfReports = emp_mgr.NoOfReports + 1 -- Increment mgr's  
   FROM inserted                            -- (no. of reports) by  
   WHERE emp_mgr.emp = inserted.mgr;         -- 1 for the new report.  
  
   UPDATE dbo.emp_mgr  
   SET emp_mgr.NoOfReports = emp_mgr.NoOfReports - 1 -- Decrement mgr's  
   FROM deleted                             -- (no. of reports) by 1  
   WHERE emp_mgr.emp = deleted.mgr;          -- for the new report.  
END  
GO  
-- Insert some test data rows.  
INSERT dbo.emp_mgr(emp, mgr) VALUES  
    ('Harry', NULL)  
    ,('Alice', 'Harry')  
    ,('Paul', 'Alice')  
    ,('Joe', 'Alice')  
    ,('Dave', 'Joe');  
GO  
SELECT emp,mgr,NoOfReports  
FROM dbo.emp_mgr;  
GO  
-- Change Dave's manager from Joe to Harry  
UPDATE dbo.emp_mgr SET mgr = 'Harry'  
WHERE emp = 'Dave';  
GO  
SELECT emp,mgr,NoOfReports FROM emp_mgr;  
  
GO  
```  
  
 以下是更新前的结果。  
  
```  
emp                            mgr                           NoOfReports  
------------------------------ ----------------------------- -----------  
Alice                          Harry                          2  
Dave                           Joe                            0  
Harry                          NULL                           1  
Joe                            Alice                          1  
Paul                           Alice                          0  
```  
  
 以下是更新后的结果。  
  
```  
emp                            mgr                           NoOfReports  
------------------------------ ----------------------------- -----------  
Alice                          Harry                          2  
Dave                           Harry                          0  
Harry                          NULL                           2  
Joe                            Alice                          0  
Paul                           Alice                          0  
```  
  
 **设置 nested triggers 选项**  
  
-   [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
 **设置 RECURSIVE_TRIGGERS 数据库选项**  
  
-   [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [配置 nested triggers 服务器配置选项](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md)  
  
  
