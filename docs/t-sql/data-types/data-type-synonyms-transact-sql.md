---
description: 数据类型同义词 (Transact-SQL)
title: 数据类型同义词 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], synonyms
- alternate names [SQL Server]
- synonyms [SQL Server], data types
ms.assetid: 390eef67-1a49-4185-a971-e07765be9717
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: de20f321f030d0ddfe7e7eb274577f5b4c3d3a70
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202309"
---
# <a name="data-type-synonyms-transact-sql"></a>数据类型同义词 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中包含数据类型同义词以便实现 ISO 兼容性。 下表列出了这些同义词以及它们映射到的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型。
  
|同义词|SQL Server 系统数据类型|  
|---|---|
|**Binary varying**|**varbinary**|  
|**char varying**|**varchar**|  
|**character**|**char**|  
|**character**|**char(1)**|  
|**character(n****)**|**char(n)**|  
|**character varying(n****)**|**varchar(n)**|  
|**Dec**|**decimal**|  
|**Double precision**|**float**|  
|**float**[ **(** n **)** ] for n = 1-7|**real**|  
|**float**[ **(** n **)** ] for n = 8-15|**float**|  
|**integer**|**int**|  
|**national character(** n **)**|**nchar(n)**|  
|**national char(** n **)**|**nchar(n)**|  
|**national character varying(** n **)**|**nvarchar(n)**|  
|**national char varying(** n **)**|**nvarchar(n)**|  
|**national text**|**ntext**|  
|**timestamp**|rowversion|  
  
在数据定义语言 (DDL) 语句中，数据类型同义词可用来代替相应的基本数据类型名。 这些语句包括 CREATE TABLE、CREATE PROCEDURE 和 DECLARE \@变量。 但是，创建对象后看不到同义词。 创建对象后，为对象分配与同义词相关联的基本数据类型。 没有记录显示在创建对象的语句中指定了同义词。
  
将为从原始对象派生的对象（如结果集列或表达式）分配基本数据类型。 使用原始对象或任何派生对象的任何元数据函数都将报告基本数据类型，而不是同义词，其中包括：

* 元数据操作，如 sp_help 和其他系统存储过程、
* 信息架构视图和
* 数据访问 API 元数据操作，用于报告表或结果集列的数据类型。
  
例如，您可以通过指定 `national character varying` 创建表：
  
```sql
CREATE TABLE ExampleTable (PriKey int PRIMARY KEY, VarCharCol national character varying(10))  
```  
  
为 `VarCharCol` 分配了 nvarchar(10) 数据类型，所有后续元数据函数将该列报告为 nvarchar(10) 列。 元数据函数永远不会将它们报告为 national character varying(10) 列。
  
## <a name="see-also"></a>另请参阅
[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)
  
  
