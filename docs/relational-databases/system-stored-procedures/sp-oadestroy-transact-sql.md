---
description: sp_OADestroy (Transact-SQL)
title: sp_OADestroy (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OADestroy_TSQL
- sp_OADestroy
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OADestroy
ms.assetid: 0bd1cff4-ceff-4095-9ae4-e1e65a80f5d6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ba5e524eac26b41552ad977d86ae951d0bfff17d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174413"
---
# <a name="sp_oadestroy-transact-sql"></a>sp_OADestroy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  破坏已创建的 OLE 对象。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_OADestroy objecttoken      
```  
  
## <a name="arguments"></a>参数  
 *objecttoken*  
 是先前使用 **sp_OACreate** 创建的 OLE 对象的对象标记。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或非零数字（失败），是由 OLE 自动化对象返回的 HRESULT 整数值。  
  
 有关 HRESULT 返回代码的详细信息，请参阅 [OLE 自动化返回代码和错误信息](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md)。  
  
## <a name="remarks"></a>备注  
 如果未调用 **sp_OADestroy** ，则创建的 OLE 对象在批处理结束时自动销毁。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份或直接对此存储过程执行权限。 `Ole Automation Procedures` 必须 **启用** 配置才能使用与 OLE 自动化相关的任何系统过程。  
  
## <a name="examples"></a>示例  
 下面的示例将销毁先前创建的 **SQLServer** 对象。  
  
```  
EXEC @hr = sp_OADestroy @object;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END;  
```  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 OLE 自动化存储过程 ](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [OLE 自动化脚本示例](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
