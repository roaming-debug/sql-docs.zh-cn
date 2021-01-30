---
description: sp_helpextendedproc (Transact-SQL)
title: sp_helpextendedproc (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpextendedproc
- sp_helpextendedproc_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpextendedproc
ms.assetid: 7e1f017e-c898-4225-b375-6a73ef9aac7b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e8256d22c8565b56b328119e6b5618ca4f8d226b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197622"
---
# <a name="sp_helpextendedproc-transact-sql"></a>sp_helpextendedproc (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  报告当前定义的扩展存储过程，以及该过程（函数）所属的动态链接库 (DLL) 的名称。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [CLR 集成](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpextendedproc [ [@funcname = ] 'procedure' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @funcname = ] 'procedure'` 要报告其信息的扩展存储过程的名称。 *过程* 的值为 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|扩展存储过程的名称。|  
|**.dll**|**nvarchar(255)**|DLL 的名称。|  
  
## <a name="remarks"></a>备注  
 指定 *procedure* 时， **sp_helpextendedproc** 有关指定扩展存储过程的报表。 如果未提供此参数， **sp_helpextendedproc** 将返回所有扩展存储过程名称以及每个扩展存储过程所属的 DLL 名称。  
  
## <a name="permissions"></a>权限  
 向 **public** 授予执行 **sp_helpextendedproc** 的权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-reporting-help-on-all-extended-stored-procedures"></a>A. 报告所有扩展存储过程的帮助  
 以下示例将对所有扩展存储过程进行报告。  
  
```  
USE master;  
GO  
EXEC sp_helpextendedproc;  
GO  
```  
  
### <a name="b-reporting-help-on-a-single-extended-stored-procedure"></a>B. 报告单个扩展存储过程的帮助  
 下面的示例将对 `xp_cmdshell` 扩展存储过程进行报告。  
  
```  
USE master;  
GO  
EXEC sp_helpextendedproc xp_cmdshell;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_addextendedproc &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproc-transact-sql.md)   
 [sp_dropextendedproc &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproc-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
