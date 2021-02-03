---
description: COLUMNS_UPDATED (Transact-SQL)
title: COLUMNS_UPDATED (Transact-SQL)
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COLUMNS_UPDATED_TSQL
- COLUMNS_UPDATED
dev_langs:
- TSQL
helpviewer_keywords:
- COLUMNS_UPDATED function
- testing columns
- column testing [SQL Server]
- updated columns
ms.assetid: 765fde44-1f95-4015-80a4-45388f18a42c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2f2c9a71bab585bc2b4d6b03ff62b60cfc96bd2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209985"
---
# <a name="columns_updated-transact-sql"></a>COLUMNS_UPDATED (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函数将返回 varbinary 位模式，它指示表或视图中已插入或已更新的列。 可以在 [!INCLUDE[tsql](../../includes/tsql-md.md)] INSERT 或 UPDATE 触发器主体中的任意位置使用 `COLUMNS_UPDATED`，以测试触发器是否应执行某些操作。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
COLUMNS_UPDATED ( )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
**varbinary**
  
## <a name="remarks"></a>备注  
`COLUMNS_UPDATED` 是针对在多列上执行的 UPDATE 或 INSERT 操作进行检测。 若要对一列的 UPDATE 或 INSERT 尝试进行测试，请使用 [UPDATE()](../../t-sql/functions/update-trigger-functions-transact-sql.md)。
  
`COLUMNS_UPDATED` 将按从左到右的顺序返回一个或多个字节。 每个字节的最右侧位是最低有效位。 最左侧字节的最右侧位表示表中的第一表列；向左的下一位表示第二列，依此类推。 如果创建了触发器的表中包含八列以上，`COLUMNS_UPDATED` 则将返回多个字节，最低有效字节位于最左侧。 在 INSERT 操作中，`COLUMNS_UPDATED` 将为所有列返回 TRUE，因为这些列已插入了显式值或隐式 (NULL) 值。
  
若要检测针对特定列的更新或插入操作，请使用按位运算符和已测试列的整数位掩码的语法。 例如，假设表 t1 包含列 C1、C2、C3、C4 和 C5。 若要验证列 C2、C3 和 C4 是否已全部成功更新（使用具有 UPDATE 触发器的表 t1），请使用 & 14 的语法。 若要测试是否只更新了列 C2，请指定 & 2。 有关实际示例，请参阅[示例 A](#a-using-columns_updated-to-test-the-first-eight-columns-of-a-table) 和[示例 B](#b-using-columns_updated-to-test-more-than-eight-columns)。
  
可以在 [!INCLUDE[tsql](../../includes/tsql-md.md)] INSERT 或 UPDATE 触发器内部的任意位置使用 `COLUMNS_UPDATED`。
  
INFORMATION_SCHEMA.COLUMNS 视图的 ORDINAL_POSITION 列与 `COLUMNS_UPDATED` 所返回列的位模式不兼容。 若要获取与 `COLUMNS_UPDATED` 兼容的位模式，请在查询 `INFORMATION_SCHEMA.COLUMNS` 视图时引用 `COLUMNPROPERTY` 系统函数的 `ColumnID` 属性，如以下示例所示。
  
```sql
SELECT TABLE_NAME, COLUMN_NAME,  
    COLUMNPROPERTY(OBJECT_ID(TABLE_SCHEMA + '.' + TABLE_NAME),  
    COLUMN_NAME, 'ColumnID') AS COLUMN_ID  
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS  
WHERE TABLE_NAME = 'Person';  
```  

如果将触发器应用于列，`COLUMNS_UPDATED` 将返回为 `true` 或 `1`，即使列值保持不变也是如此。 这是有意为之，并且触发器应实现确定是否允许插入/更新/删除操作的业务逻辑。 
  
## <a name="column-sets"></a>列集
对表定义了列集后，`COLUMNS_UPDATED` 函数的行为将如下所述：
-   显式更新列集的成员列后，该列的对应位将被设置为 1；该列集位也将被设置为 1。  
-   显式更新列集后，列集位将被设置为 1；该表中所有稀疏列的位也将被设置为 1。  
-   对于插入操作，所有位都将设置为 1。  
  
     由于对列集的更改将导致列集中所有列的位将被设置为 1，因此，列集中未更改的列将显示为已修改。 有关列集的详细信息，请参阅[使用列集](../../relational-databases/tables/use-column-sets.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-columns_updated-to-test-the-first-eight-columns-of-a-table"></a>A. 使用 COLUMNS_UPDATED 测试表的前八列  
此示例将创建两个表：`employeeData` 和 `auditEmployeeData`。 `employeeData` 表包含敏感的员工工资信息，人力资源部的成员可以对此表进行修改。 如果员工的社会保险号码 (SSN)、年薪或银行帐户号码发生了更改，则将生成一个审核记录并插入到 `auditEmployeeData` 审核表中。
  
通过 `COLUMNS_UPDATED()` 函数，我们可以快速对包含敏感员工信息的列所做的任何更改进行检测。 仅在尝试检测对表中前八列进行的更改时，以这种方式使用 `COLUMNS_UPDATED()` 才有效。
  
```sql
USE AdventureWorks2012;  
GO  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_NAME = 'employeeData')  
   DROP TABLE employeeData;  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_NAME = 'auditEmployeeData')  
   DROP TABLE auditEmployeeData;  
GO  
CREATE TABLE dbo.employeeData (  
   emp_id INT NOT NULL PRIMARY KEY,  
   emp_bankAccountNumber CHAR (10) NOT NULL,  
   emp_salary INT NOT NULL,  
   emp_SSN CHAR (11) NOT NULL,  
   emp_lname NCHAR (32) NOT NULL,  
   emp_fname NCHAR (32) NOT NULL,  
   emp_manager INT NOT NULL  
   );  
GO  
CREATE TABLE dbo.auditEmployeeData (  
   audit_log_id uniqueidentifier DEFAULT NEWID() PRIMARY KEY,  
   audit_log_type CHAR (3) NOT NULL,  
   audit_emp_id INT NOT NULL,  
   audit_emp_bankAccountNumber CHAR (10) NULL,  
   audit_emp_salary INT NULL,  
   audit_emp_SSN CHAR (11) NULL,  
   audit_user sysname DEFAULT SUSER_SNAME(),  
   audit_changed DATETIME DEFAULT GETDATE()  
   );  
GO  
CREATE TRIGGER dbo.updEmployeeData   
ON dbo.employeeData   
AFTER UPDATE AS  
/* Check whether columns 2, 3 or 4 have been updated. If any or all  
columns 2, 3 or 4 have been changed, create an audit record.
The bitmask is: power(2, (2-1)) + power(2, (3-1)) + power(2, (4-1)) = 14.
This bitmask translates into base_10 as: 2 + 4 + 8 = 14.
To test whether all columns 2, 3, and 4 are updated, use = 14 instead of > 0  
(below). */
  
   IF (COLUMNS_UPDATED() & 14) > 0  
/* Use IF (COLUMNS_UPDATED() & 14) = 14 to see whether all columns 2, 3,   
and 4 are updated. */  
      BEGIN  
-- Audit OLD record.  
      INSERT INTO dbo.auditEmployeeData  
         (audit_log_type,  
         audit_emp_id,  
         audit_emp_bankAccountNumber,  
         audit_emp_salary,  
         audit_emp_SSN)  
         SELECT 'OLD',   
            del.emp_id,  
            del.emp_bankAccountNumber,  
            del.emp_salary,  
            del.emp_SSN  
         FROM deleted del;  
  
-- Audit NEW record.  
      INSERT INTO dbo.auditEmployeeData  
         (audit_log_type,  
         audit_emp_id,  
         audit_emp_bankAccountNumber,  
         audit_emp_salary,  
         audit_emp_SSN)  
         SELECT 'NEW',  
            ins.emp_id,  
            ins.emp_bankAccountNumber,  
            ins.emp_salary,  
            ins.emp_SSN  
         FROM inserted ins;  
   END;  
GO  
  
/* Inserting a new employee does not cause the UPDATE trigger to fire. */  
INSERT INTO employeeData  
   VALUES ( 101, 'USA-987-01', 23000, 'R-M53550M', N'Mendel', N'Roland', 32);  
GO  
  
/* Updating the employee record for employee number 101 to change the   
salary to 51000 causes the UPDATE trigger to fire and an audit trail to   
be produced. */  
  
UPDATE dbo.employeeData  
   SET emp_salary = 51000  
   WHERE emp_id = 101;  
GO  
SELECT * FROM auditEmployeeData;  
GO  
  
/* Updating the employee record for employee number 101 to change both   
the bank account number and social security number (SSN) causes the   
UPDATE trigger to fire and an audit trail to be produced. */  
  
UPDATE dbo.employeeData  
   SET emp_bankAccountNumber = '133146A0', emp_SSN = 'R-M53550M'  
   WHERE emp_id = 101;  
GO  
SELECT * FROM dbo.auditEmployeeData;  
  
GO  
```  
  
### <a name="b-using-columns_updated-to-test-more-than-eight-columns"></a>B. 使用 COLUMNS_UPDATED 测试八个以上的列  
若要对影响表中前八列以外的列的更新进行检测，请使用 `SUBSTRING` 函数测试由 `COLUMNS_UPDATED` 返回的正确位。 此示例对影响 `AdventureWorks2012.Person.Person` 表中的列 `3`、`5` 和 `9` 的更新进行检测。
  
```sql
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Person.uContact2', N'TR') IS NOT NULL  
    DROP TRIGGER Person.uContact2;  
GO  
CREATE TRIGGER Person.uContact2 ON Person.Person  
AFTER UPDATE AS  
    IF ( (SUBSTRING(COLUMNS_UPDATED(), 1, 1) & 20 = 20)   
        AND (SUBSTRING(COLUMNS_UPDATED(), 2, 1) & 1 = 1) )   
    PRINT 'Columns 3, 5 and 9 updated';  
GO  
  
UPDATE Person.Person   
   SET NameStyle = NameStyle,  
      FirstName=FirstName,  
      EmailPromotion=EmailPromotion;  
GO  
```  
  
## <a name="see-also"></a>另请参阅
[位运算符 (Transact-SQL)](../../t-sql/language-elements/bitwise-operators-transact-sql.md)  
[CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)  
[UPDATE() (Transact-SQL)](../../t-sql/functions/update-trigger-functions-transact-sql.md)
  
  
