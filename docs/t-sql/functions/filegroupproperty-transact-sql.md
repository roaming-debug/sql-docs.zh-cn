---
description: FILEGROUPPROPERTY (Transact-SQL)
title: FILEGROUPPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FILEGROUPPROPERTY_TSQL
- FILEGROUPPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- filegroups [SQL Server], property values
- FILEGROUPPROPERTY function
- viewing filegroup properties
- displaying filegroup properties
ms.assetid: b3a930e6-df05-4034-929c-f681f5f6fc6e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 369a9e1b4914fe4c3de1d7fc86596067d67867ef
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128485"
---
# <a name="filegroupproperty-transact-sql"></a>FILEGROUPPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

此函数返回指定名称的文件组属性值和文件组值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
FILEGROUPPROPERTY ( filegroup_name, property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 filegroup_name  
类型为 sysname 的表达式，它表示要为 `FILEGROUPPROPERTY` 返回命名的属性信息的文件组名称。  
  
 *property*  
类型为 varchar(128) 的表达式，它返回文件组属性的名称。 Property 可以返回下列值之一：  
  
|值|说明|返回的值|  
|-----------|-----------------|--------------------|  
|**IsReadOnly**|文件组是只读的。|1 = TRUE<br /><br /> 0 = FALSE<br /><br /> NULL = 输入无效。|  
|**IsUserDefinedFG**|文件组是用户定义文件组。|1 = TRUE<br /><br /> 0 = FALSE<br /><br /> NULL = 输入无效。|  
|**IsDefault**|文件组是默认的文件组。|1 = TRUE<br /><br /> 0 = FALSE<br /><br /> NULL = 输入无效。|  
  
## <a name="return-types"></a>返回类型  
**int**  
  
## <a name="remarks"></a>备注  
filegroup_name 与 sys.filegroups 目录视图中的 name 列相对应。  
  
## <a name="examples"></a>示例  
此示例返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中主文件组的 `IsDefault` 属性设置。  
  
```sql  
SELECT FILEGROUPPROPERTY('PRIMARY', 'IsDefault') AS 'Default Filegroup';  
GO  
```  

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]   
```  
Default Filegroup   
---------------------   
1  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
 [FILEGROUP_ID (Transact-SQL)](../../t-sql/functions/filegroup-id-transact-sql.md)   
 [FILEGROUP_NAME (Transact-SQL)](../../t-sql/functions/filegroup-name-transact-sql.md)   
 [元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [sys.filegroups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)  
  
  
