---
description: sp_prepexec (Transact-SQL)
title: sp_prepexec (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepexec
- sp_cursor_prepexec_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_prepexec
ms.assetid: f9141850-a62b-43bf-8e46-b2f92b75ca56
author: markingmyname
ms.author: maghan
ms.openlocfilehash: df6a482aeca510fde3d89dd74ddafc0ebe713287
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196006"
---
# <a name="sp_prepexec-transact-sql"></a>sp_prepexec (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  准备并执行参数化的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 sp_prepexec 结合了 sp_prepare 和 sp_execute 的函数。 此操作由表格格式数据流中的 ID = 13 调用 (TDS) 包。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_prepexec handle OUTPUT, params , stmt  
    [ , bound param ] [ ,...n]]  
```  
  
## <a name="arguments"></a>参数  
 *柄*  
 是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成的 *句柄* 标识符。 *句柄* 是带有 **int** 返回值的必需参数。  
  
 *params*  
 标识参数化语句。 变量的参数定义将替换为语句 *中的参数* 标记。 *params* 是调用 **ntext**、 **nchar** 或 **nvarchar** 输入值的必需参数。 如果语句未参数化，则输入一个 NULL 值。  
  
 *stmt*  
 定义游标结果集。 *Stmt* 参数是必需的，并且调用了 **ntext**、 **nchar** 或 **nvarchar** 输入值。  
  
 *bound_param*  
 指示可选使用其他参数。 *bound_param* 调用任何数据类型的输入值来指定要使用的其他参数。  
  
## <a name="examples"></a>示例  
 下面的示例准备并执行一个简单的语句：  
  
```  
Declare @Out int;  
EXEC sp_prepexec @Out output,   
    N'@P1 nvarchar(128), @P2 nvarchar(100)',  
    N'SELECT database_id, name  
      FROM sys.databases  
      WHERE name=@P1 AND state_desc = @P2',   
          @P1 = 'tempdb', @P2 = 'ONLINE';   
EXEC sp_unprepare @Out;  
```  
  
## <a name="see-also"></a>请参阅  
 [sp_prepare &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-prepare-transact-sql.md)   
 [sp_execute &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-execute-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
