---
description: CURRENT_TIMEZONE_ID (Transact-SQL)
title: CURRENT_TIMEZONE_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/18/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CURRENT_TIMEZONE)ID
- CURRENT_TIMEZONE_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- current time zone id [SQL Server]
- current timezoneid [SQL Server]
- system time zone id[SQL Server]
- system timezone id[SQL Server]
- functions [SQL Server], time zone id
- functions [SQL Server], timezoneid
- timezoneid [SQL Server], functions
- time zone id [SQL Server], functions
- CURRENT_TIMEZONE_ID function [SQL Server]
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 70f4fbb5e683aa7bc6436b27a727f2c3952b6592
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184049"
---
# <a name="current_timezone_id-transact-sql"></a>CURRENT_TIMEZONE_ID (Transact-SQL)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

此函数返回由服务器或实例观察到的时区的 ID。 对于 Azure SQL 托管实例，返回值基于实例创建期间它本身分配到的时区，而不是基于基础操作系统的时区。
  
> [!NOTE]  
> 对于 SQL 数据库，时区始终设置为 UTC，且 `CURRENT_TIMEZONE_ID` 返回 UTC 时区的 ID。
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CURRENT_TIMEZONE_ID ( )  
```
  
## <a name="arguments"></a>参数

此函数没有参数。
  
## <a name="return-type"></a>返回类型  

**varchar**
  
## <a name="remarks"></a>备注  

`CURRENT_TIMEZONE_ID` 是非确定性函数。 引用该列的视图和表达式无法进行索引。
  
## <a name="example"></a>示例

返回值反映服务器或实例的实际时区和语言设置。

```sql
SELECT CURRENT_TIMEZONE_ID();  
/* Returned:  
W. Europe Standard Time
*/
```  
  
## <a name="see-also"></a>另请参阅

[SQL 托管实例时区](/azure/sql-database/sql-database-managed-instance-timezone)

[CURRENT_TIMEZONE()](./current-timezone-transact-sql.md)