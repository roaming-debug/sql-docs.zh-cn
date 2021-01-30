---
description: sp_dbmmonitoraddmonitoring (Transact-SQL)
title: sp_dbmmonitoraddmonitoring (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitoraddmonitoring
- sp_dbmmonitoraddmonitoring_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], monitoring
- sp_dbmmonitoraddmonitoring
ms.assetid: 9489dc30-af29-4363-a172-4645947fc95e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 646602dc1ff6ed3d6db9fac9d2a3223b114291e4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190590"
---
# <a name="sp_dbmmonitoraddmonitoring-transact-sql"></a>sp_dbmmonitoraddmonitoring (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  创建数据库镜像监视器作业，该作业可定期更新服务器实例上每个镜像数据库的镜像状态。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dbmmonitoraddmonitoring [ update_period ]  
```  
  
## <a name="arguments"></a>参数  
 *update_period*  
 指定更新间隔（分钟）。 此值可以是介于 1 到 120 分钟之间的值。 默认值为 1 分钟。  
  
> [!NOTE]  
>  如果将更新持续时间设置得太低，客户端的响应时间可能会增加。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 此过程要求允许 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理在服务器实例上运行，为了运行数据库镜像监视器作业，必须运行该代理。  
  
 如果从开始数据库镜像 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，则 **sp_dbmmonitoraddmonitoring** 过程会自动运行。 如果使用 ALTER DATABASE 语句手动开始镜像，若要监视服务器实例上的镜像数据库，则必须手动运行 **sp_dbmmonitoraddmonitoring** 。  
  
> [!NOTE]  
>  如果在设置数据库镜像之前运行 **sp_dbmmonitoraddmonitoring** ，则监视作业将运行，但不会更新存储数据库镜像监视器历史记录的状态表。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例将以 `3` 分钟为更新持续时间来启动监视。  
  
```  
EXEC sp_dbmmonitoraddmonitoring 3;  
```  
  
## <a name="see-also"></a>另请参阅  
 [监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorchangemonitoring &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangemonitoring-transact-sql.md)   
 [sp_dbmmonitordropmonitoring &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropmonitoring-transact-sql.md)   
 [sp_dbmmonitorhelpmonitoring &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpmonitoring-transact-sql.md)   
 [sp_dbmmonitorresults (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
  
