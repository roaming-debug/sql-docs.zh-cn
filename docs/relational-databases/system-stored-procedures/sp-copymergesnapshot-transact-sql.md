---
description: sp_copymergesnapshot (Transact-SQL)
title: sp_copymergesnapshot (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_copymergesnapshot
- sp_copymergesnapshot_TSQL
helpviewer_keywords:
- sp_copymergesnapshot
ms.assetid: eaecd6e0-8486-4e5d-ace7-8ae75768c0a8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 49418b8df94d987b1050591fb6e4cd1d88ddc82d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185797"
---
# <a name="sp_copymergesnapshot-transact-sql"></a>sp_copymergesnapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  将指定发布的快照文件夹复制到 **\@ destination_folder** 中列出的文件夹。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_copymergesnapshot [ @publication = ] 'publication', [ @destination_folder = ] 'destination_folder'  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 要复制其快照内容的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @destination_folder = ] 'destination_folder'` 是要将发布快照内容复制到其中的文件夹的名称。 *destination_folder* 为 **nvarchar (255)**，无默认值。 *Destination_folder* 可以是备用位置，如其他服务器、网络驱动器或可移动媒体 (，如 CD-ROMs 或可移动磁盘) 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_copymergesnapshot** 用于合并复制。 运行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本7.0 及更早版本的订阅服务器不能使用备用快照位置。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_copymergesnapshot**。  
  
## <a name="see-also"></a>另请参阅  
 [备用快照文件夹位置](../../relational-databases/replication/snapshot-options.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
