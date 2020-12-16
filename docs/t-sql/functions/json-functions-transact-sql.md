---
description: JSON 函数 (Transact-SQL)
title: JSON 函数 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
helpviewer_keywords:
- JSON functions
ms.assetid: ec97d451-06af-44a3-8304-305d410cfc8e
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: c55ffdeec513c71c89cd5ed4bb8ef9f6e88e7330
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478608"
---
# <a name="json-functions-transact-sql"></a>JSON 函数 (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

使用本部分中的页面上介绍的函数可验证或更改 JSON 文本，或是提取简单或复杂值。  
  
|函数|描述|  
|--------------|-----------------|  
|[ISJSON](../../t-sql/functions/isjson-transact-sql.md)|测试字符串是否包含有效 JSON。|  
|[JSON_VALUE](../../t-sql/functions/json-value-transact-sql.md)|从 JSON 字符串中提取标量值。|  
|[JSON_QUERY](../../t-sql/functions/json-query-transact-sql.md)|从 JSON 字符串中提取对象或数组。|  
|[JSON_MODIFY](../../t-sql/functions/json-modify-transact-sql.md)|更新 JSON 字符串中属性的值，并返回已更新的 JSON 字符串。|

 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中对 JSON 的内置支持的详细信息，请参阅 [JSON 数据 (SQL Server)](../../relational-databases/json/json-data-sql-server.md)。  

## <a name="see-also"></a>另请参阅

 - [使用内置函数验证、查询和更改 JSON 数据 (SQL Server)](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md)
 - [JSON 路径表达式 (SQL Server)](../../relational-databases/json/json-path-expressions-sql-server.md)
 - [JSON 数据 (SQL Server)](../../relational-databases/json/json-data-sql-server.md)  
