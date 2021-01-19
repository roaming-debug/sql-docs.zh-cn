---
description: DROP RULE (Transact-SQL)
title: DROP RULE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_RULE_TSQL
- DROP RULE
dev_langs:
- TSQL
helpviewer_keywords:
- rules [SQL Server], removing
- deleting roles
- DROP RULE statement
- removing roles
- dropping roles
ms.assetid: 8370b730-7fd5-43fe-a7f6-8300b3caa16d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 81dd188416fd57274dbfd2ba587ddd9fdde1f51c
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172599"
---
# <a name="drop-rule-transact-sql"></a>DROP RULE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从当前数据库中删除一个或多个用户定义规则。  
  
> [!IMPORTANT]
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的下一版本中将删除 DROP RULE。 请不要在新的开发工作中使用 DROP RULE，并准备修改当前使用它们的应用程序。 请改用可以通过 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 或 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 的 CHECK 关键字创建的 CHECK 约束。 有关详细信息，请参阅 [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DROP RULE [ IF EXISTS ] { [ schema_name . ] rule_name } [ ,...n ] [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 IF EXISTS  
 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 到[当前版本](https://go.microsoft.com/fwlink/p/?LinkId=299658)）。  
  
 只有在规则已存在时才对其进行有条件地删除。  
  
 *schema_name*  
 规则所属架构的名称。  
  
 *rule*  
 要删除的规则。 规则名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。 可以根据需要选择指定规则架构名称。  
  
## <a name="remarks"></a>注解  
 如果规则当前绑定到列或别名数据类型，则需先解除绑定才能删除该规则。 若要对规则解除绑定，请使用 **sp_unbindrule**。 如果尝试删除的规则是绑定的，则将显示错误消息，并取消 DROP RULE 语句。  
  
 规则被删除后，在以前受规则约束的列中输入的新数据时将不受规则的约束。 现有数据不受任何影响。  
  
 DROP RULE 语句不适用于 CHECK 约束。 有关删除 CHECK 约束的详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 若要在最小范围内执行 DROP RULE，用户必须对规则所属的架构具有 ALTER 权限。  
  
## <a name="examples"></a>示例  
 以下示例将解除绑定，然后删除名为 `VendorID_rule` 的规则。 
  
```sql  
sp_unbindrule 'Production.ProductVendor.VendorID'  
DROP RULE VendorID_rule  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE RULE (Transact-SQL)](../../t-sql/statements/create-rule-transact-sql.md)   
 [sp_bindrule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [sp_unbindrule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [USE (Transact-SQL)](../../t-sql/language-elements/use-transact-sql.md)  

