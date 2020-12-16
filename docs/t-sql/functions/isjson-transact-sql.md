---
description: ISJSON (Transact-SQL)
title: ISJSON (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ISJSON
- ISJSON_TSQL
helpviewer_keywords:
- ISJSON function
- JSON, validating
ms.assetid: c836f3d3-3e17-44ae-92bf-f341918896c3
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 49d1bcd94ed12e0b9bb22e63018b0867f680dd41
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464298"
---
# <a name="isjson-transact-sql"></a>ISJSON (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  测试字符串是否包含有效 JSON。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
ISJSON ( expression )  
```  
  
## <a name="arguments"></a>参数
 *expression*  
 要测试的字符串。  
  
## <a name="return-value"></a>返回值  
 如果字符串包含有效 JSON，则返回 1；否则，返回 0。 如果 expression 为 NULL，则返回 NULL。  
  
 不返回错误。  
  
## <a name="remarks"></a>备注  
 **ISJSON** 不检查在相同级别的键的唯一性。  
  
## <a name="examples"></a>示例  
  
### <a name="example-1"></a>示例 1  
如果参数值 `@param` 包含有效 JSON，则下面的示例有条件地运行语句块。  
  
```sql  
DECLARE @param <data type>
SET @param = <value>

IF (ISJSON(@param) > 0)  
BEGIN  
     -- Do something with the valid JSON value of @param.  
END
```  
  
### <a name="example-2"></a>示例 2  
下面的示例将返回其列 `json_col` 包含有效 JSON 的行。  
  
```sql  
SELECT id, json_col
FROM tab1
WHERE ISJSON(json_col) > 0 
```  
  
## <a name="see-also"></a>另请参阅  
 [JSON 数据 (SQL Server)](../../relational-databases/json/json-data-sql-server.md)  
  
  
