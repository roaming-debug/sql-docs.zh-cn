---
description: sp_cursorclose (Transact-SQL)
title: sp_cursorclose (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_close_TSQL
- sp_cursor_close
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursorclose
ms.assetid: d9b7b44d-cdff-456e-97df-7031a3b9beb6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dc19fd9d00cde0bb29582c5bc0079be4039436af
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205170"
---
# <a name="sp_cursorclose-transact-sql"></a>sp_cursorclose (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  关闭并取消分配游标，并释放所有关联的资源;也就是说，它会删除用于支持键集或静态 **游标** 的临时表。 sp_cursorclose 通过在表格格式数据流 (TDS) 数据包中指定 ID = 9 来调用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_cursorclose cursor  
```  
  
## <a name="arguments"></a>参数  
 *cursor*  
 是由生成并由 sp_cursoropen 过程返回的游标 *句柄* 值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *cursor* 是为 **int** 输入值调用的必需参数。  
  
> [!NOTE]  
>  输入值 -1 将应用于当前连接上的所有游标。  
  
## <a name="remarks"></a>备注  
 如果在游标关闭之后或指定了无效的句柄，则 *游标* 将返回错误消息。  
  
 RPC 状态指示总体成功或失败。  
  
 DONE 行计数始终为 0。  
  
## <a name="see-also"></a>另请参阅  
 [sp_cursoropen &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
