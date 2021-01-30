---
description: sp_scriptpublicationcustomprocs (Transact-SQL)
title: sp_scriptpublicationcustomprocs (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_scriptpublicationcustomprocs
- sp_scriptpublicationcustomprocs_TSQL
helpviewer_keywords:
- sp_scriptpublicationcustomprocs
ms.assetid: b06102d5-4284-4834-b126-bc0baea49be5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 733ca066ba7c11754fde7b82b228c7ac3796a42e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189966"
---
# <a name="sp_scriptpublicationcustomprocs-transact-sql"></a>sp_scriptpublicationcustomprocs (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  在启用了自动生成自定义过程架构选项的发布中，为所有表项目编写自定义 INSERT、UPDATE 和 DELETE 过程的脚本。 **sp_scriptpublicationcustomprocs** 对于手动应用快照的设置订阅特别有用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_scriptpublicationcustomprocs [ @publication = ] 'publication_name'  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication_name'` 发布的名称。 *publication_name* **sysname** ，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 返回由单个 **nvarchar (4000)** 列组成的结果集。 该结果集构成创建自定义存储过程所需的完整 CREATE PROCEDURE 语句。  
  
## <a name="remarks"></a>备注  
 如果未启用自动生成自定义过程 (0x2) 架构选项，则不会为项目编写自定义过程的脚本。  
  
 **Sp_scriptpublicationcustomprocs** 使用以下过程创建订阅服务器，而不应直接执行此过程：  
  
 **sp_script_reconciliation_delproc**  
  
 **sp_script_reconciliation_insproc**  
  
 **sp_script_reconciliation_vdelproc**  
  
 **sp_script_reconciliation_xdelproc**  
  
 **sp_scriptdelproc**  
  
 **sp_scriptinsproc**  
  
 **sp_scriptmappedupdproc**  
  
 **sp_scriptupdproc**  
  
 **sp_scriptvdelproc**  
  
 **sp_scriptvupdproc**  
  
 **sp_scriptxdelproc**  
  
 **sp_scriptxupdproc**  
  
## <a name="permissions"></a>权限  
 向 **public** 授予 Execute 权限;在此存储过程中执行过程安全检查，以限制对 **sysadmin** 固定服务器角色成员的访问，并在当前数据库中 **db_owner** 固定数据库角色。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
