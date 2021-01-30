---
description: sp_dropanonymousagent (Transact-SQL)
title: sp_dropanonymousagent (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropanonymousagent
- sp_dropanonymousagent_TSQL
helpviewer_keywords:
- sp_dropanonymousagent
ms.assetid: 4cb96efa-9358-44a3-a8ee-a7e181bed089
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: b75f8b4bc355fd71c469ee2c0593f56feaf9148a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200719"
---
# <a name="sp_dropanonymousagent-transact-sql"></a>sp_dropanonymousagent (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从发布服务器中删除分发服务器上进行复制监视的匿名代理。 此存储过程在发布服务器的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_dropanonymousagent [ @subid= ] sub_id    , [ @type= ] type  
```  
  
## <a name="arguments"></a>参数  
`[ @subid = ] sub_id` 匿名订阅的全局标识符。 *sub_id* 是 **uniqueidentifier**，无默认值。 可以使用 **sp_helppullsubscription** 在订阅服务器上检索此标识符。 返回的结果集的 **subid** 字段中的值是此全局标识符。  
  
`[ @type = ] type` 订阅的类型。 *类型* 为 **int**，无默认值。 有效值为 **1** 或 **2**。 如果使用分发代理快照复制或事务复制，请指定 **1**。 如果使用合并代理合并复制，则指定 **2**。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_dropanonymousagent** 在所有类型的复制中使用。  
  
 此存储过程只用于删除匿名订阅代理，而不能用于删除众所周知的订阅。  
  
## <a name="permissions"></a>权限  
 只有分发数据库中 **db_owner** 固定数据库角色的成员才能执行 **sp_dropanonymousagent**。  
  
## <a name="see-also"></a>另请参阅  
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
