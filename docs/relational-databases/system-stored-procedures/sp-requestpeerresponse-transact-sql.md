---
description: sp_requestpeerresponse (Transact-SQL)
title: sp_requestpeerresponse (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_requestpeerresponse_TSQL
- sp_requestpeerresponse
helpviewer_keywords:
- sp_requestpeerresponse
ms.assetid: cbe13c22-4d7d-4a36-b194-7a13ce68ef27
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ce25686ab2b7947438d207a3acb5fc5b7f6549e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199964"
---
# <a name="sp_requestpeerresponse-transact-sql"></a>sp_requestpeerresponse (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从对等拓扑中的节点执行此过程时，此过程将从拓扑中的其他每个节点请求响应。 通过执行此过程并检查对应的响应，可以保证所有先前命令都已传递到响应的节点。 此存储过程在请求的节点上对任何数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_requestpeerresponse [ @publication = ] 'publication'  
    [ , [ @description = ] 'description'  
    [ , [ @request_id = ] request_id OUTPUT ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 要验证其状态的对等拓扑中的发布名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @description = ] 'description'` 可用于标识各个状态请求的用户定义的信息。 *描述* 为 **nvarchar (4000)**，默认值为 NULL。  
  
`[ @request_id = ] request_id` 返回新请求的 ID。 *request_id* 为 **int** ，并且是一个 OUTPUT 参数。 此值可在执行 [sp_helppeerresponses &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helppeerresponses-transact-sql.md) 查看对状态请求的所有响应时使用。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_requestpeerresponse** 用于对等事务复制。  
  
 **sp_requestpeerresponse** 用于确保在还原在对等拓扑中发布的数据库之前所有其他节点已接收到所有的命令。 此过程还用于复制节点脱机时所做的数据定义语言 (DDL) 更改，以评估这些更改到达其他节点的时间。  
  
 不能在用户定义的事务中执行 **sp_requestpeerresponse** 。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_requestpeerresponse**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_deletepeerrequesthistory &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-deletepeerrequesthistory-transact-sql.md)   
 [sp_helppeerrequests &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helppeerrequests-transact-sql.md)  
  
  
