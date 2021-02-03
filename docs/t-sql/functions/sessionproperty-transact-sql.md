---
description: SESSIONPROPERTY (Transact-SQL)
title: SESSIONPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SESSIONPROPERTY
- SESSIONPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET statement, SESSIONPROPERTY function
- SESSIONPROPERTY function
- sessions [SQL Server], SET options settings
ms.assetid: 1f3730b4-1495-4d3a-af43-e57952812df9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 4ce8fb3ed5f419efdcde0eac6c22c61bca543c09
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212196"
---
# <a name="sessionproperty-transact-sql"></a>SESSIONPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回会话的 SET 选项设置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
SESSIONPROPERTY (option)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *option*  
 该会话的当前选项设置。 option 可以是下列任意值。  
  
|选项|说明|  
|------------|-----------------|  
|ANSI_NULLS|指定是否对 Null 值上的等号 (=) 和不等号 (<>) 应用 ISO 标准行为。<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|ANSI_PADDING|控制列存储小于定义的列大小的值的方式，以及列存储在字符串和 binary 数据中有尾随空格的值的方式。<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|ANSI_WARNINGS|指定是否对某些情况下（包括被零除和算术溢出）生成错误消息或警告应用 ISO 标准行为。<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|ARITHABORT|确定在执行查询过程中发生溢出或被零除的错误时是否终止查询。<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|CONCAT_NULL_YIELDS_ NULL|控制是将串联结果视为 Null 还是空字符串值。<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|NUMERIC_ROUNDABORT|指定当表达式中的舍入导致精度降低时是否生成错误消息和警告。<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|QUOTED_IDENTIFIER|指定是否遵从 ISO 关于如何使用引号来分隔标识符和文字字符串的规则。<br /><br /> 1 = ON<br /><br /> 0 = OFF|  
|\<Any other string>|NULL = 输入无效。|  
  
## <a name="return-types"></a>返回类型  
 **sql_variant**  
  
## <a name="remarks"></a>备注  
 通过组合服务器级、数据库级和用户指定的选项对 SET 选项进行配置。  
  
## <a name="examples"></a>示例  
 以下示例返回 `CONCAT_NULL_YIELDS_NULL` 选项的设置。  
  
```sql  
SELECT   SESSIONPROPERTY ('CONCAT_NULL_YIELDS_NULL')  
```  
  
## <a name="see-also"></a>另请参阅  
 [sql_variant (Transact-SQL)](../../t-sql/data-types/sql-variant-transact-sql.md)   
 [SET ANSI_NULLS (Transact-SQL)](../../t-sql/statements/set-ansi-nulls-transact-sql.md)   
 [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)   
 [SET ANSI_WARNINGS (Transact-SQL)](../../t-sql/statements/set-ansi-warnings-transact-sql.md)   
 [SET ARITHABORT (Transact-SQL)](../../t-sql/statements/set-arithabort-transact-sql.md)   
 [SET CONCAT_NULL_YIELDS_NULL (Transact-SQL)](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)   
 [SET NUMERIC_ROUNDABORT (Transact-SQL)](../../t-sql/statements/set-numeric-roundabort-transact-sql.md)   
 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)  
  
  
