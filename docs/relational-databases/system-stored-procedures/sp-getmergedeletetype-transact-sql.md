---
description: sp_getmergedeletetype (Transact-SQL)
title: sp_getmergedeletetype (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_getmergedeletetype
- sp_getmergedeletetype_TSQL
helpviewer_keywords:
- sp_getmergedeletetype
ms.assetid: 64450e4d-844d-4176-874e-f3845536f7d2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 816815f36511cb64f52ec5a48f20c04121fe67bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183157"
---
# <a name="sp_getmergedeletetype-transact-sql"></a>sp_getmergedeletetype (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回合并删除的类型。 该存储过程在发布服务器的发布数据库中或在订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_getmergedeletetype [ @source_object = ] 'source_object', [ @rowguid =] 'rowguid', [ @delete_type=] delete_type OUTPUT  
```  
  
## <a name="arguments"></a>参数  
`[ @source_object = ] 'source_object'` 源对象的名称。 *source_object* 为 **nvarchar (386)**，无默认值。  
  
`[ @rowguid = ] 'rowguid'` 删除类型的行标识符。 *rowguid* 是 **uniqueidentifier**，无默认值。  
  
`[ @delete_type = ] delete_type OUTPUT` 表示删除类型的代码。 *delete_type* 为 **int**，没有默认值。 *delete_type* 也是 OUTPUT 参数，并且可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**1**|用户删除|  
|**5**|部分删除|  
|**6**|系统删除|  
  
## <a name="remarks"></a>备注  
 **sp_getmergedeletetype** 用于合并复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_getmergedeletetype**。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
