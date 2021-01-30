---
title: sys.sp_rda_reconcile_columns (Transact-sql) |Microsoft Docs
description: 了解 sys.sp_rda_reconcile_columns。 使用此存储过程来协调远程 Azure 表和启用 Stretch SQL Server 表中的列。
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_reconcile_columns
- sys.sp_rda_reconcile_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_reconcile_columns stored procedure
ms.assetid: 60d9cc4e-1828-450b-9d88-5b8485800d73
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8eb33cbb1fd2975d96a727f6a7fde457c9827cc8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211780"
---
# <a name="syssp_rda_reconcile_columns-transact-sql"></a>sys.sp_rda_reconcile_columns (Transact-sql) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  将远程 Azure 表中的列与启用 Stretch SQL Server 表中的列协调。  
    
  **sp_rda_reconcile_columns** 将列添加到已启用 Stretch SQL Server 表中但不在远程表中的远程表中。 这些列可能是您意外从远程表中删除的列。 但是， **sp_rda_reconcile_columns** 不会删除远程表中存在的远程表中的列，而不会删除 SQL Server 表中的列。
  
  > [!IMPORTANT]
  > 当 **sp_rda_reconcile_columns** 重新创建你从远程表中意外删除的列时，不会还原之前位于已删除列中的数据。
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
   
## <a name="syntax"></a>语法  
  
```  
  
sp_rda_reconcile_columns @objname = '@objname'  
  
```  
  
## <a name="arguments"></a>参数  
 \@objname = '*\@ objname*'  
 已启用延伸的 SQL Server 表的名称。  
  
## <a name="return-code-values"></a>返回代码值  
 0 (成功) 或 >0 (故障)   
  
## <a name="permissions"></a>权限  
 需要 db_owner 权限。  
   
## <a name="remarks"></a>备注  
 如果远程 Azure 表中存在已启用延伸的 SQL Server 表中不复存在的列，这些额外的列不会阻止 Stretch Database 正常运行。 你可以选择手动删除额外列。  
  
## <a name="example"></a>示例  
 若要协调远程 Azure 表中的列，请运行以下语句。  
  
```sql  
EXEC sp_rda_reconcile_columns @objname = N'StretchEnabledTableName';  
```  
  
  
