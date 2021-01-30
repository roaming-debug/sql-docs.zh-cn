---
description: sp_cursorexecute (Transact-SQL)
title: sp_cursorexecute (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursorexecute
- sp_cursorexecute_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor_execute
ms.assetid: 6a204229-0a53-4617-a57e-93d4afbb71ac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6aab1670ae9d517ec60d3af572d15eaf12432f7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205153"
---
# <a name="sp_cursorexecute-transact-sql"></a>sp_cursorexecute (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  基于 sp_cursorprepare 创建的执行计划创建并填充游标。 此过程与 sp_cursorprepare 结合在一起具有与 sp_cursoropen 相同的功能，但拆分为两个阶段。 sp_cursorexecute 通过在表格格式数据流 (TDS) 数据包中指定 ID = 4 来调用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_cursorexecute prepared_handle, cursor  
    [ , scrollopt[ OUTPUT ]  
    [ , ccopt[ OUTPUT ]  
    [ ,rowcount OUTPUT [ ,bound param][,...n]]]]]  
```  
  
## <a name="arguments"></a>参数  
 *prepared_handle*  
 Sp_cursorprepare 返回的已准备语句 *句柄* 值。 *prepared_handle* 是为 **int** 输入值调用的必需参数。  
  
 *cursor*  
 为 SQL Server 生成的游标标识符。 *cursor* 是必需的参数，必须在所有对游标执行操作的后续过程（如 sp_cursorfetch  
  
 *scrollopt*  
 滚动选项。 *scrollopt* 是一个可选参数，它需要 **int** 输入值。 Sp_cursorexecute *scrollopt* 参数与 sp_cursoropen 的参数具有相同的值选项。  
  
> [!NOTE]  
>  不支持 PARAMETERIZED_STMT 值。  
  
> [!IMPORTANT]  
>  如果未指定 *scrollopt* 值，则默认值为键集，而不考虑 sp_cursorprepare 中指定的 *scrollopt* 值。  
  
 *ccopt*  
 币种控制选项。 *ccopt* 是一个可选参数，它需要 **int** 输入值。 Sp_cursorexecute *ccopt* 参数与 sp_cursoropen 的参数具有相同的值选项。  
  
> [!IMPORTANT]  
>  如果未指定 *ccopt* 值，则默认值为乐观，而不考虑在 sp_cursorprepare 中指定的 *ccopt* 值。  
  
 *数*  
 一个可选参数，指示要用于 AUTO_FETCH 的提取缓冲区行数。 默认值为 20 行。 指定为输入值与返回值时，*行计数* 的行为不同。  
  
|作为输入值|作为返回值|  
|--------------------|---------------------|  
|当指定 AUTO_FETCH 时，将 FAST_FORWARD cursor 行 *计数* ，表示要放入提取缓冲区中的行数。|表示结果集中的行数。 当指定 *scrollopt* AUTO_FETCH 值时， *rowcount* 返回提取到提取缓冲区中的行数。|  
  
 *bound_param*  
 指示可选使用其他参数。  
  
> [!NOTE]  
>  第五个参数之后的任何参数都将作为输入参数传递到语句计划。  
  
## <a name="code-return-value"></a>代码返回值  
 *rowcount* 可能返回以下值。  
  
|值|说明|  
|-----------|-----------------|  
|-1|未知的行数。|  
|-n|异步填充生效。|  
  
## <a name="remarks"></a>备注  
  
## <a name="scrollopt-and-ccopt-parameters"></a>scrollopt 和 ccopt 参数  
 当对服务器缓存抢占缓存的计划时， *scrollopt* 和 *ccopt* 非常有用，这意味着必须重新编译标识该语句的准备好的句柄。 *Scrollopt* 和 *ccopt* 参数值必须与在 sp_cursorprepare 的原始请求中发送的值匹配。  
  
> [!NOTE]  
>  不应将 PARAMETERIZED_STMT 分配给 *scrollopt*。  
  
 如果无法提供匹配值，则将导致重新编译计划，而取消 prepare 和 execute 操作。  
  
## <a name="rpc-and-tds-considerations"></a>RPC 和 TDS 注意事项  
 RPC RETURN_METADATA 输入标志可设置为 1，以请求在 TDS 流中返回游标选择列表元数据。  
  
## <a name="see-also"></a>另请参阅  
 [sp_cursoropen &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorfetch &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
