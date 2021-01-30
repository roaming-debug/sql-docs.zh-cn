---
title: sys.sp_rda_set_query_mode (Transact-sql) |Microsoft Docs
description: 使用 sys.sp_rda_set_query_mode 指定是否针对当前启用 Stretch 的数据库及其表的查询返回本地和远程数据或仅返回本地数据。
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_set_query_mode
- sys.sp_rda_set_query_mode_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_set_query_mode stored procedure
ms.assetid: 65a0b390-cf87-4db7-972a-1fdf13456c88
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed9e5afda379b094a61c9f6d3130b986e0b83ca3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211729"
---
# <a name="syssp_rda_set_query_mode-transact-sql"></a>sys.sp_rda_set_query_mode (Transact-sql) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  指定是否对当前启用 Stretch 的数据库及其表进行查询同时返回默认)  (本地和远程数据，或仅返回本地数据。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_rda_set_query_mode [ @mode = ] @mode   
    [ , [ @force = ]  @force ]  
```  
  
## <a name="arguments"></a>参数  
 [ @mode =] *\@ 模式*  
 是以下值之一。  
  
-   **已禁用** 针对启用 Stretch 的表的所有查询都将失败。  
  
-   **LOCAL_ONLY** 针对启用 Stretch 的表的查询仅返回本地数据。  
  
-   **LOCAL_AND_REMOTE** 针对启用 Stretch 的表的查询将返回本地和远程数据。 这是默认行为。  
  
 [ @force =] *\@ force*  
 如果希望在不验证的情况下更改查询模式，则可以将此值设置为1。  
  
## <a name="return-code-values"></a>返回代码值  
 0 (成功) 或 >0 (故障)   
  
## <a name="permissions"></a>权限  
 需要 db_owner 权限。  
  
## <a name="remarks"></a>备注  
 以下扩展存储过程还为已启用延伸的数据库设置查询模式。  
  
-   **sp_rda_deauthorize_db**  
  
     运行 **sp_rda_deauthorize_db** 后，针对已启用延伸的数据库和表的所有查询都将失败。 也就是说，查询模式设置为 "已禁用"。 若要退出此模式，请执行以下操作之一。  
  
    -   运行 [sys.sp_rda_reauthorize_db &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) 重新连接到远程 Azure 数据库。 此操作会自动将查询模式重置为 LOCAL_AND_REMOTE，这是 Stretch Database 的默认行为。 也就是说，查询从本地和远程数据返回结果。  
  
    -   运行带有 LOCAL_ONLY 参数的 [sys.sp_rda_set_query_mode](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md) ，以允许查询仅针对本地数据运行。  
  
-   **sp_rda_reauthorize_db**  
  
     当你运行 [sys.sp_rda_reauthorize_db &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) 重新连接到远程 Azure 数据库时，此操作会自动将查询模式重置为 LOCAL_AND_REMOTE，这是 Stretch Database 的默认行为。 也就是说，查询从本地和远程数据返回结果。  
  
## <a name="see-also"></a>另请参阅  
 [sys.sp_rda_deauthorize_db &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md)   
 [Stretch 数据库](../../sql-server/stretch-database/stretch-database.md)  
  
  
