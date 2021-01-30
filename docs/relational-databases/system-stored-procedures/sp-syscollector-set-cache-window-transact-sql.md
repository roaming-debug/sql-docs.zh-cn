---
description: sp_syscollector_set_cache_window (Transact-SQL)
title: sp_syscollector_set_cache_window (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_set_cache_window
- sp_syscollector_set_cache_window_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_set_cache_window stored procedure
- data collector [SQL Server], stored procedures
ms.assetid: 660f2749-392f-46bf-89f3-27764d848507
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a2fdab11e6cdd22b06005b2aa530e11b0c7e6173
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203666"
---
# <a name="sp_syscollector_set_cache_window-transact-sql"></a>sp_syscollector_set_cache_window (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  设置在数据上载失败时尝试上载数据的次数。 在失败时重新尝试上载可降低丢失所收集数据的风险。  

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_syscollector_set_cache_window [ @cache_window = ] cache_window   
```  
  
## <a name="arguments"></a>参数  
 [ @cache_window =] *cache_window*  
 将数据上载到管理数据仓库失败但不丢失数据的重试次数。 *cache_window* 为 **int** ，默认值为1。 *cache_window* 可以具有以下值之一：  
  
|值|说明|  
|-----------|-----------------|  
|-1|缓存先前上载失败的所有上载数据。|  
|0|不缓存上载失败的任何数据。|  
|*n*|从 n 个以前的上载失败中缓存数据，其中 *n* >= 1。|  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 在更改缓存窗口配置之前，必须禁用数据收集器。 如果数据收集器处于启用状态，此存储过程将失败。 有关详细信息，请参阅 [启用或禁用数据收集](../../relational-databases/data-collection/enable-or-disable-data-collection.md)和 [管理数据收集](../../relational-databases/data-collection/manage-data-collection.md)。  
  
## <a name="permissions"></a>权限  
 需要具有 dc_admin（拥有 EXECUTE 权限）固定数据库角色的成员身份才能执行此过程。  
  
## <a name="examples"></a>示例  
 以下示例禁用数据收集器，配置缓存窗口以最多保留三次上载失败的数据，然后启用数据收集器。  
  
```sql  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_disable_collector;  
GO  
EXECUTE dbo.sp_syscollector_set_cache_window 3;  
GO  
EXECUTE dbo.sp_syscollector_enable_collector;  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_syscollector_set_cache_directory (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-syscollector-set-cache-directory-transact-sql.md)  
  
  
