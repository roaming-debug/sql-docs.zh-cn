---
description: SET LANGUAGE (Transact-SQL)
title: SET LANGUAGE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/05/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_LANGUAGE_TSQL
- SET LANGUAGE
dev_langs:
- TSQL
helpviewer_keywords:
- LANGUAGE option
- languages [SQL Server], setting language
- SET LANGUAGE statement
- options [SQL Server], date
- default languages
ms.assetid: 0ec0e5cf-e115-4be9-a0db-e65837d6fa45
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 892aa2ba74c93d3ee38bca4b3831fb2de2f56e4b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161837"
---
# <a name="set-language-transact-sql"></a>SET LANGUAGE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  指定会话的语言环境。 会话语言确定 **datetime** 格式和系统消息。  
  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
SET LANGUAGE { [ N ] 'language' | @language_var }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 [**N**] **'** _language_ **'**  |  **@** _language\_var_  
 存储在 [sys.syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) 中的语言的名称。 此参数可以是 Unicode，或者是转换为 Unicode 的 DBCS。 若要指定使用 Unicode 的语言，请使用 **N'** _language_ **'** 。 如果将语言指定为变量，则变量的数据类型必须为 **sysname**。  
  
## <a name="remarks"></a>注解  
 SET LANGUAGE 的设置是在执行或运行时设置，而不是在分析时设置。  
  
 SET LANGUAGE 隐式设置 [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) 的设置。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="examples"></a>示例  
 以下示例将默认语言设置为 `Italian`，显示月份，然后切换回 `us_english` 并再次显示月份名。  
  
```sql
DECLARE @Today DATETIME;  
SET @Today = '12/5/2007';  
  
SET LANGUAGE Italian;  
SELECT DATENAME(month, @Today) AS 'Month Name';  
  
SET LANGUAGE us_english;  
SELECT DATENAME(month, @Today) AS 'Month Name' ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md)   
 [sp_helplanguage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helplanguage-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
