---
description: sp_msx_defect (Transact-SQL)
title: sp_msx_defect (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_msx_defect
- sp_msx_defect_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_msx_defect
ms.assetid: 0dfd963a-3bc5-4b58-94f7-aec976da2883
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2ca7f1d044ddfe0730052ff811e6084d5c070b6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174452"
---
# <a name="sp_msx_defect-transact-sql"></a>sp_msx_defect (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从多服务器操作中删除当前服务器。  
  
> [!CAUTION]  
>  **sp_msx_defect** 编辑注册表。 建议不要手动编辑注册表，因为不适当或不正确的更改会导致严重的系统配置问题。 因此，只有有经验的用户才可以使用注册表编辑器程序编辑注册表。 有关详细信息，请参阅 Microsoft Windows 文档。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_msx_defect [@forced_defection =] forced_defection  
```  
  
## <a name="arguments"></a>参数  
`[ @forced_defection = ] forced_defection` 指定是否在主 SQLServerAgent 由于不可逆转损坏的 **msdb** 数据库而永久丢失或没有 **msdb** 数据库备份的情况下，强制脱离。 *forced_defection* 为 **bit**，默认值为 **0**，指示不应发生强制脱离。 值为 **1** 时强制脱离。  
  
 通过执行 **sp_msx_defect** 强制脱离后，在主 SQLServerAgent 上， **sysadmin** 固定服务器角色的成员必须运行以下命令才能完成脱离：  
  
```  
EXECUTE msdb.dbo.sp_delete_targetserver @server_name = 'tsx-server', @post_defection =  0;  
```  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 当 **sp_msx_defect** 正确完成时，将返回一条消息。  
  
## <a name="permissions"></a>权限  
 若要执行此存储过程，用户必须为 **sysadmin** 固定服务器角色的成员。  
  
## <a name="see-also"></a>另请参阅  
 [sp_msx_enlist &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-msx-enlist-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
