---
description: PERMISSIONS (Transact-SQL)
title: PERMISSIONS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PERMISSIONS_TSQL
- PERMISSIONS
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], verifying
- current permission status
- users [SQL Server], permissions status
- checking permission status
- security [SQL Server], permissions
- verifying permission status
- testing permissions
- PERMISSIONS function
ms.assetid: 81625a56-b160-4424-91c5-1ce8b259a8e6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2fa0effd0cd559ae6f52e422ce96684d6b9b9e0f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211448"
---
# <a name="permissions-transact-sql"></a>PERMISSIONS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回一个包含位图的值，该值指示当前用户的语句、对象或列权限。  
  
 **重要提示** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 改用 [fn_my_permissions](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md) 和 [Has_Perms_By_Name](../../t-sql/functions/has-perms-by-name-transact-sql.md)。 继续使用 PERMISSIONS 函数可能导致性能降低。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
PERMISSIONS ( [ objectid [ , 'column' ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *objectid*  
 安全对象的 ID。 如果未指定 objectid，则位图值包含当前用户的语句权限；否则，位图包含当前用户对该安全对象的权限。 指定的安全对象必须在当前数据库中。 使用 [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md) 函数确定 objectid 值。  
  
 **'** *column* **'**  
 返回其权限信息的列的可选名。 该列必须是 objectid 指定的表中的有效列名。  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="remarks"></a>备注  
 可以使用 PERMISSIONS 确定当前用户是否具有执行某个语句或向另一用户授予权限所需的权限。  
  
 所返回的权限信息是 32 位位图。  
  
 低 16 位反映授予用户的权限，以及应用于 Windows 组或当前用户属于其成员的固定服务器角色的权限。 例如，当没有指定 objectid 时，将返回值 66（十六进制值 0x42），指示当前用户具有执行 CREATE TABLE（十进制值 2）和 BACKUP DATABASE（十进制值 64）语句的权限。  
  
 高 16 位反映用户可以授予其他用户的权限。 除左移 16 位（与 65536 相乘）之外，高 16 位的解释方式与下表中所介绍的低 16 位的解释方式完全相同。 例如，位 0x8（十进制值 8）指示当指定 objectid 时的 INSERT 权限。 而 0x80000（十进制值 524288）指示 GRANT INSERT 权限的能力，这是因为 524288 = 8 x 65536。  
  
 由于角色中的成员身份，没有执行语句权限的用户仍然可以将该权限授予其他用户。  
  
 下表显示语句权限所使用的位（未指定 objectid）。  
  
|位（十进制）|位（十六进制）|语句权限|  
|-----------------|-----------------|--------------------------|  
|1|0x1|CREATE DATABASE（仅限于 master 数据库）|  
|2|0x2|CREATE TABLE|  
|4|0x4|CREATE PROCEDURE|  
|8|0x8|CREATE VIEW|  
|16|0x10|CREATE RULE|  
|32|0x20|CREATE DEFAULT|  
|64|0x40|BACKUP DATABASE|  
|128|0x80|BACKUP LOG|  
|256|0x100|保留|  
  
 下表显示当仅指定 objectid 时，返回的对象权限所使用的位。  
  
|位（十进制）|位（十六进制）|语句权限|  
|-----------------|-----------------|--------------------------|  
|1|0x1|SELECT ALL|  
|2|0x2|UPDATE ALL|  
|4|0x4|REFERENCES ALL|  
|8|0x8|INSERT|  
|16|0x10|DELETE|  
|32|0x20|EXECUTE（仅限于过程）|  
|4096|0x1000|SELECT ANY（至少一列）|  
|8192|0x2000|UPDATE ANY|  
|16384|0x4000|REFERENCES ANY|  
  
 下表显示当同时指定 objectid 和列时，返回的列级对象权限所使用的位。  
  
|位（十进制）|位（十六进制）|语句权限|  
|-----------------|-----------------|--------------------------|  
|1|0x1|SELECT|  
|2|0x2|UPDATE|  
|4|0x4|REFERENCES|  
  
 如果指定的参数为 NULL 或无效（例如，objectid 或列不存在），则返回 NULL。 没有定义不适用的权限所使用的位值（例如，表的 EXECUTE 权限、位 0x20）。  
  
 使用位与 (&) 运算符确定 PERMISSIONS 函数返回的位图中的每个位集。  
  
 还可使用 sp_helprotect 系统存储过程返回某用户在当前数据库中的权限列表。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-the-permissions-function-with-statement-permissions"></a>A. 对语句权限使用 PERMISSIONS 函数  
 以下示例确定当前用户能否执行 `CREATE TABLE` 语句。  
  
```sql  
IF PERMISSIONS()&2=2  
   CREATE TABLE test_table (col1 INT)  
ELSE  
   PRINT 'ERROR: The current user cannot create a table.';  
```  
  
### <a name="b-using-the-permissions-function-with-object-permissions"></a>B. 对对象权限使用 PERMISSIONS 函数  
 以下示例确定当前用户能否在 `Address` 数据库的 `AdventureWorks2012` 表中插入数据行。  
  
```sql  
IF PERMISSIONS(OBJECT_ID('AdventureWorks2012.Person.Address','U'))&8=8   
   PRINT 'The current user can insert data into Person.Address.'  
ELSE  
   PRINT 'ERROR: The current user cannot insert data into Person.Address.';  
```  
  
### <a name="c-using-the-permissions-function-with-grantable-permissions"></a>C. 对可授予的权限使用 PERMISSIONS 函数  
 以下示例确定当前用户能否将 `Address` 数据库的 `AdventureWorks2012` 表的 INSERT 权限授予另一用户。  
  
```sql  
IF PERMISSIONS(OBJECT_ID('AdventureWorks2012.Person.Address','U'))&0x80000=0x80000  
   PRINT 'INSERT on Person.Address is grantable.'  
ELSE  
   PRINT 'You may not GRANT INSERT permissions on Person.Address.';  
```  
  
## <a name="see-also"></a>另请参阅  
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [OBJECT_ID (Transact-SQL)](../../t-sql/functions/object-id-transact-sql.md)   
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [sp_helprotect (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [系统函数 (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
