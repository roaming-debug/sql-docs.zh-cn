---
description: '&#x40;&#x40;TOTAL_ERRORS (Transact-SQL)'
title: '@@TOTAL_ERRORS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@TOTAL_ERRORS'
- '@@TOTAL_ERRORS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@TOTAL_ERRORS function'
- total errors [SQL Server]
- errors [SQL Server], read/write
- number of disk read/write errors
- disks [SQL Server], errors
- write errors [SQL Server]
- read/write errors
ms.assetid: 09e62428-ee0e-4ef5-b969-da9d255f1199
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 851909caa64e8391275d5908f49e652bf1d993ec
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128357"
---
# <a name="x40x40total_errors-transact-sql"></a>&#x40;&#x40;TOTAL_ERRORS (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回自上次启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所遇到的磁盘写入错误数。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
@@TOTAL_ERRORS  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 **integer**  
  
## <a name="remarks"></a>备注  
 并非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所遇到的所有写入错误都由该函数进行处理。 偶尔发生的非致命写入错误由服务器本身进行处理，并不将其视为错误。 若要显示包含几个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 统计信息（包括错误总数信息）的报表，请运行 sp_monitor。  
  
## <a name="examples"></a>示例  
 以下示例显示了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 到当前日期和时间为止所遇到的错误数。  
  
```sql
SELECT @@TOTAL_ERRORS AS 'Errors', GETDATE() AS 'As of';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Errors      As of                   
----------- ----------------------  
0           3/28/2003 12:32:11 PM   
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_monitor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)   
 [系统统计函数 (Transact-SQL)](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
  
  
