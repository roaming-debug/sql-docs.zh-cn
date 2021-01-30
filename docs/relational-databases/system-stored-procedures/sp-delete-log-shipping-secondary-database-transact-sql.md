---
description: sp_delete_log_shipping_secondary_database (Transact-SQL)
title: sp_delete_log_shipping_secondary_database (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_log_shipping_secondary_database_TSQL
- sp_delete_log_shipping_secondary_database
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_log_shipping_secondary_database
ms.assetid: c71b21c0-ec04-4fbd-9735-01128b736935
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4482987bf93b8cbf900f0b17cc062d5c1cba46a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211120"
---
# <a name="sp_delete_log_shipping_secondary_database-transact-sql"></a>sp_delete_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  此存储过程删除辅助数据库、本地历史记录和远程历史记录。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_delete_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database'  
```  
  
## <a name="arguments"></a>参数  
`[ @secondary_database = ] 'secondary_database'` 辅助数据库的名称。 *secondary_database* **sysname**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无。  
  
## <a name="remarks"></a>备注  
 必须从辅助服务器上的 **master** 数据库运行 **sp_delete_log_shipping_secondary_database** 。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能运行此过程。  
  
## <a name="see-also"></a>另请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
