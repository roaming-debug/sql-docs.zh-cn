---
description: xp_sprintf (Transact-SQL)
title: xp_sprintf (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 01/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_sprintf_TSQL
- xp_sprintf
dev_langs:
- TSQL
helpviewer_keywords:
- xp_sprintf
ms.assetid: 1eedd65c-03cc-4eab-b76e-04684fdfec52
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 75d36ece3d2875e7b6ae30a19626ee96bf85e0af
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99122706"
---
# <a name="xp_sprintf-transact-sql"></a>xp_sprintf (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  设置一系列字符和值的格式并将其存储到字符串输出参数中。 每个格式参数都用相应的参数替换。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
xp_sprintf { string OUTPUT , format }  
     [ , argument [ ,...n ] ]  
```  
  
## <a name="arguments"></a>参数  
 *string*  
 接收输出的 **varchar** 变量。  
  
 OUTPUT  
 如果指定，则将变量值放在输出参数中。  
  
 *format*  
 带有 *参数* 值占位符的格式字符串，类似于 C 语言的 **sprintf** 函数所支持的值。 目前仅支持 %s 格式参数。  
  
 argument   
 字符串，代表相应格式参数的值。  
  
 *n*  
 是一个占位符，指示最多可指定 50 个参数。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 **xp_sprintf** 返回以下消息：  
  
 `The command(s) completed successfully.`  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [&#40;Transact-sql&#41;的常规扩展存储过程 ](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [xp_sscanf &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/xp-sscanf-transact-sql.md)  
  
  
