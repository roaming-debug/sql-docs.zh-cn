---
description: sp_OASetProperty (Transact-SQL)
title: sp_OASetProperty (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OASetProperty
- sp_OASetProperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OASetProperty
ms.assetid: 0fe7d554-6b67-4d55-9d3e-4096802c47f8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ff901b8f3aa7ad7c31680b8664fb753f9873cadc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195420"
---
# <a name="sp_oasetproperty-transact-sql"></a>sp_OASetProperty (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将 OLE 对象的属性设置为新值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_OASetProperty objecttoken , propertyname , newvalue [ , index... ]  
```  
  
## <a name="arguments"></a>参数  
 *objecttoken*  
 是先前由 **sp_OACreate** 创建的 OLE 对象的对象标记。  
  
 propertyname  
 要设置新值的 OLE 对象的属性名。  
  
 *newvalue*  
 属性的新值，并且必须是适当数据类型的值。  
  
 *index*  
 索引参数。 如果已指定，则 *index* 必须为适当数据类型的值。  
  
 有些属性包含参数。 这些属性称为索引化属性，相应的参数被称为索引参数。 一个属性可有多个索引参数。  
  
> [!NOTE]  
>  此存储过程的参数按位置（而不是按名称）指定。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或非零数字（失败），是由 OLE 自动化对象返回的 HRESULT 整数值。  
  
 有关 HRESULT 返回代码的详细信息，请参阅 [OLE 自动化返回代码和错误信息](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md)。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份或直接对此存储过程执行权限。 `Ole Automation Procedures` 必须 **启用** 配置才能使用与 OLE 自动化相关的任何系统过程。  
  
## <a name="examples"></a>示例  
 下面的示例将 `HostName` 以前创建的 **SQLServer** 对象的属性 (设置) 为新值。  
  
```  
EXEC @hr = sp_OASetProperty @object, 'HostName', 'Gizmo';  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END'  
```  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 OLE 自动化存储过程 ](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [OLE 自动化脚本示例](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
