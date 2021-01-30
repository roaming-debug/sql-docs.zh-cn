---
description: sp_browsesnapshotfolder (Transact-SQL)
title: sp_browsesnapshotfolder (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_browsesnapshotfolder
- sp_browsesnapshotfolder_TSQL
helpviewer_keywords:
- sp_browsesnapshotfolder
ms.assetid: 0872edf2-4038-4bc1-a68d-05ebfad434d2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a74c1325b2af76a96c0fc544dfbbf136b0561225
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199187"
---
# <a name="sp_browsesnapshotfolder-transact-sql"></a>sp_browsesnapshotfolder (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回为发布生成的最新快照的完整路径。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_browsesnapshotfolder [@publication= ] 'publication'  
    { [ , [ @subscriber = ] 'subscriber' ]  
 [ , [ @subscriber_db = ] 'subscriber_db' ] }  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 包含项目的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @subscriber = ] 'subscriber'` 订阅服务器的名称。 *订阅服务器* 的值为 **sysname**，默认值为 NULL。  
  
`[ @subscriber_db = ] 'subscriber_db'` 订阅数据库的名称。 *subscriber_db* 的默认值为 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**snapshot_folder**|**nvarchar(512)**|快照目录的完整路径。|  
  
## <a name="remarks"></a>备注  
 **sp_browsesnapshotfolder** 用于快照复制和事务复制。  
  
 如果 *订阅服务器* 和 *SUBSCRIBER_DB* 字段为空，则存储过程将返回该发布的最新快照的快照文件夹。 如果指定了 *订阅服务器* 和 *subscriber_db* 字段，则存储过程将返回指定订阅的快照文件夹。 如果没有为发布生成快照，则返回空结果集。  
  
 如果将发布设置为同时在发布服务器的工作目录和发布服务器的快照文件夹中生成快照文件，则结果集将包含两行。 第一行包含发布快照文件夹，第二行包含发布服务器工作目录。 **sp_browsesnapshotfolder** 可用于确定生成快照文件的目录。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_browsesnapshotfolder**。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
