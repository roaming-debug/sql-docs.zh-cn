---
description: Version - Transact SQL 元数据函数
title: VERSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 95a79b33-98f2-4929-a1a5-93b522a9e152
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 2d9aa1c5d67ceabc7e1fcd78f56e1c501f02a169
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754467"
---
# <a name="version---transact-sql-metadata-functions"></a>Version - Transact SQL 元数据函数
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

 返回在设备上运行的 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 或 [!INCLUDE[ssPDW_md](../../includes/sspdw-md.md)] 的版本。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Azure Synapse Analytics and Parallel Data Warehouse  
VERSION ( )  
```  
  
## <a name="arguments"></a>参数  
  
## <a name="general-remarks"></a>一般备注  
必须在 [FROM](../../t-sql/queries/from-transact-sql.md) 子句中指定表名称，此函数才会返回结果。 对于该查询的结果集中的每一行，都会返回结果行，使用 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md) 可限制返回的行数。  
  
## <a name="examples"></a>示例  
下面的示例返回版本号。  
  
```sql
SELECT VERSION();  
```  
  
## <a name="see-also"></a>另请参阅 
[SESSION_ID (Transact-SQL)](../../t-sql/functions/session-id-transact-sql.md)  
[DB_NAME (Transact-SQL)](../../t-sql/functions/db-name-transact-sql.md)  
  
  
