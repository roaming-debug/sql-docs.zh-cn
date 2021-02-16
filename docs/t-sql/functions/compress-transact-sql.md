---
description: COMPRESS (Transact-SQL)
title: COMPRESS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/11/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COMPRESS
- COMPRESS_TSQL
helpviewer_keywords:
- COMPRESS function
ms.assetid: c2bfe9b8-57a4-48b4-b028-e1a3ed5ece88
author: cawrites
ms.author: chadam
ms.openlocfilehash: 63ae2752a8d94a5b24aa8ebc2d3033985548de67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352419"
---
# <a name="compress-transact-sql"></a>COMPRESS (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

此函数使用 GZIP 算法压缩输入表达式。 该函数返回类型 varbinary(max) 的字节数组。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
COMPRESS ( expression )  
```  
  
## <a name="arguments"></a>参数
*expression*  
A

* **binary(** _n_*_)_*
* **char(** _n_*_)_*
* **nchar(** _n_*_)_*
* **nvarchar(max)**
* **nvarchar(** _n_*_)_*
* **varbinary(max)**
* **varbinary(** _n_*_)_*
* **varchar(max)**

或

* **varchar(** _n_*_)_*

expression。 有关详细信息，请参阅[表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)。
  
## <a name="return-types"></a>返回类型
varbinary(max) 代表已压缩的输入内容。
  
## <a name="remarks"></a>注解  
压缩数据无法编入索引。
  
`COMPRESS` 函数压缩输入的表达式数据。 必须调用此函数，才能压缩每个部分的数据。 请参阅[数据压缩](../../relational-databases/data-compression/data-compression.md)，详细了解行或页级别存储过程中的自动数据压缩。
  
## <a name="examples"></a>示例  
  
### <a name="a-compress-data-during-the-table-insert"></a>A. 在插入表格期间压缩数据  
此示例演示如何压缩插入到表格中的数据：
  
```sql
INSERT INTO player (name, surname, info )  
VALUES (N'Ovidiu', N'Cracium',   
        COMPRESS(N'{"sport":"Tennis","age": 28,"rank":1,"points":15258, turn":17}'));  
  
INSERT INTO player (name, surname, info )  
VALUES (N'Michael', N'Raheem', compress(@info));  
```  
  
### <a name="b-archive-compressed-version-of-deleted-rows"></a>B. 将已删除行的压缩版本进行存档  
此语句先从 `player` 表中删除旧的播放器记录。 为节省空间，它会以压缩格式将记录存储在 `inactivePlayer` 表中。
  
```sql
DELETE FROM player  
OUTPUT deleted.id, deleted.name, deleted.surname, deleted.datemodifier, COMPRESS(deleted.info)   
INTO dbo.inactivePlayers
WHERE datemodified < @startOfYear; 
```  
  
## <a name="see-also"></a>另请参阅
[字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
[DECOMPRESS (Transact-SQL)](../../t-sql/functions/decompress-transact-sql.md)
  
  
