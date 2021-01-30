---
description: xp_sqlmaint (Transact-SQL)
title: xp_sqlmaint (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_sqlmaint
- xp_sqlmaint_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- xp_sqlmaint
ms.assetid: bda66e1b-6bbd-49be-b86e-37efc920e912
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5b83e5e84d5712e9b1cf3e253222d1ef6d212720
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187953"
---
# <a name="xp_sqlmaint-transact-sql"></a>xp_sqlmaint (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  使用包含 **sqlmaint** 开关的字符串调用 **sqlmaint** 实用程序。 **Sqlmaint** 实用工具对一个或多个数据库执行一组维护操作。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
xp_sqlmaint 'switch_string'     
```  
  
## <a name="arguments"></a>参数  
 **"** *switch_string* **"**  
 一个字符串，其中包含 **sqlmaint** 实用工具开关。 开关及其值之间必须以空格分隔。  
  
 **-？** 开关对于 **xp_sqlmaint** 无效。  
  
## <a name="return-code-values"></a>返回代码值  
 无。 如果 **sqlmaint** 实用程序失败，将返回错误。  
  
## <a name="remarks"></a>备注  
 如果使用 SQL Server 身份验证登录的用户调用此过程，则在执行之前，将使用 **-U "**_login_id_*_"_* 和 **-P "**_password_*_"_* 开关进行 *switch_string* 。 如果用户已登录 Windows 身份验证，则 *switch_string* 传递，而不会更改为 **sqlmaint**。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 在以下示例中，`xp_sqlmaint` 调用 `sqlmaint` 执行完整性检查、创建报表文件并更新 `msdb.dbo.sysdbmaintplan_history`。  
  
```  
EXEC xp_sqlmaint '-D AdventureWorks2012 -PlanID 02A52657-D546-11D1-9D8A-00A0C9054212   
   -Rpt "C:\Program Files\Microsoft SQL Server\MSSQL\LOG\DBMaintPlan2.txt" -WriteHistory  -CkDB -CkAl';   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
The command(s) executed successfully.  
```  
  
## <a name="see-also"></a>另请参阅  
 [sqlmaint 实用程序](../../tools/sqlmaint-utility.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
