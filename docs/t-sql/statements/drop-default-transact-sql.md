---
description: DROP DEFAULT (Transact-SQL)
title: DROP DEFAULT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_DEFAULT_TSQL
- DROP DEFAULT
dev_langs:
- TSQL
helpviewer_keywords:
- DROP DEFAULT statement
- defaults [SQL Server], removing
ms.assetid: d2d3af25-8877-46ba-95d9-1844961d97ee
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2762305952b7c2e189a9c399f614974e1fc32e11
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688766"
---
# <a name="drop-default-transact-sql"></a>DROP DEFAULT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从当前数据库中删除一个或多个用户定义的默认值。  
  
> [!IMPORTANT]
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的下一版本中将删除 DROP DEFAULT。 请不要在新的开发工作中使用 DROP DEFAULT，并准备修改当前使用它的应用程序。 请改用通过使用 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 或 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 的 DEFAULT 关键字创建的默认定义。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DROP DEFAULT [ IF EXISTS ] { [ schema_name . ] default_name } [ ,...n ] [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 IF EXISTS  
 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 到[当前版本](../../sql-server/what-s-new-in-sql-server-2016.md)）。  
  
 只有在默认值已存在时才对其进行有条件地删除。  
  
 *schema_name*  
 默认值所属架构的名称。  
  
 default_name  
 现有默认值的名称。 若要查看现有默认值的列表，请执行 **sp_help**。 默认值必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。 可以选择是否指定默认架构名称。  
  
## <a name="remarks"></a>备注  
 删除默认值之前，如果默认值当前绑定到某个列或别名数据类型，请通过执行 **sp_unbindefault** 解除默认值的绑定。  
  
 从允许空值的列中删除默认值后，当添加行且没有显式提供值时，将在该位置插入 NULL。 从 NOT NULL 列中删除默认值后，当添加行且没有显式提供值时，将返回错误消息。 这些行以后作为典型 INSERT 语句行为的一部分添加。  
  
## <a name="permissions"></a>权限  
 若要执行 DROP DEFAULT，用户至少应对默认值所属的架构具有 ALTER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-dropping-a-default"></a>A. 删除默认值  
 如果默认值没有绑定到列或别名数据类型，只需使用 DROP DEFAULT 即可将其删除。 以下示例删除用户创建的名为 `datedflt` 的默认值。  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.objects  
         WHERE name = 'datedflt'   
            AND type = 'D')  
   DROP DEFAULT datedflt;  
GO  
```  
  
 从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始，可以使用以下语法。  
  
```sql  
DROP DEFAULT IF EXISTS datedflt;  
GO  
```  
  
### <a name="b-dropping-a-default-that-has-been-bound-to-a-column"></a>B. 删除绑定到列的默认值  
 以下示例取消与 `EmergencyContactPhone` 表的 `Contact` 列关联的默认值的绑定，然后删除名为 `phonedflt` 的默认值。  
  
```sql  
USE AdventureWorks2012;  
GO  
   BEGIN   
      EXEC sp_unbindefault 'Person.Contact.Phone'  
      DROP DEFAULT phonedflt  
   END;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE DEFAULT (Transact-SQL)](../../t-sql/statements/create-default-transact-sql.md)   
 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_unbindefault (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)  
