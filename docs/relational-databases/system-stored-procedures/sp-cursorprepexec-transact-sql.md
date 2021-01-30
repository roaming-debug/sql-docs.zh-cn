---
description: sp_cursorprepexec (Transact-SQL)
title: sp_cursorprepexec (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/20/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursorprepexec_TSQL
- sp_cursorprepexec
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursorprepexec
ms.assetid: 8094fa90-35b5-4cf4-8012-0570cb2ba1e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b59597048ba7edc1102c3151d720e9bc6ff36942
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209701"
---
# <a name="sp_cursorprepexec-transact-sql"></a>sp_cursorprepexec (Transact-SQL)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  为提交的游标语句或批处理编译计划，然后创建并填充游标。 sp_cursorprepexec 结合了 sp_cursorprepare 和 sp_cursorexecute 的函数。 此过程通过在表格格式数据流 (TDS) 数据包中指定 ID = 5 来调用。  
  
 ![链接图标](../../database-engine/configure-windows/media/topic-link.gif "“链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_cursorprepexec prepared handle OUTPUT, cursor OUTPUT, params , statement , options  
    [ , scrollopt [ , ccopt [ , rowcount ] ] ]  
    [, '@parameter_name[,...n ]']
```  
  
## <a name="arguments"></a>参数  
 *准备的句柄*  
 是生成的已 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 准备的 *句柄* 标识符。 需要 *准备的句柄* 并返回 **int**。  
  
 *cursor*  
 是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成的游标标识符。 *cursor* 是必需的参数，必须在对此游标执行操作的所有后续过程（例如 sp_cursorfetch）上提供。  
  
 *params*  
 标识参数化语句。 变量的参数定义将替换为语句 *中的参数* 标记。 *params* 是调用 **ntext**、 **nchar** 或 **nvarchar** 输入值的必需参数。  
  
> [!NOTE]  
>  当 *stmt* 已参数化且 *scrollopt* PARAMETERIZED_STMT 值为 ON 时，使用 **ntext** 字符串作为输入值。  
  
 *语句*  
 定义游标结果集。 *语句* 参数是必需的，并且调用了 **ntext**、 **nchar** 或 **nvarchar** 输入值。  
  
> [!NOTE]  
>  用于指定 stmt 值的规则与 sp_cursoropen 的规则相同，但 *stmt* 字符串数据类型必须为 **ntext**。  
  
 *options*  
 一个可选参数，它返回游标结果集列的说明。 * 选项需要以下 **int** 输入值。  
  
|值|说明|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
 *scrollopt*  
 滚动选项。 *scrollopt* 是一个可选参数，它需要以下 **整数** 输入值之一。  
  
|值|说明|  
|-----------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
|0x10|FAST_FORWARD|  
|0x1000|PARAMETERIZED_STMT|  
|0x2000|AUTO_FETCH|  
|0x4000|AUTO_CLOSE|  
|0x8000|CHECK_ACCEPTED_TYPES|  
|0x10000|KEYSET_ACCEPTABLE|  
|0x20000|DYNAMIC_ACCEPTABLE|  
|0x40000|FORWARD_ONLY_ACCEPTABLE|  
|0x80000|STATIC_ACCEPTABLE|  
|0x100000|FAST_FORWARD_ACCEPTABLE|  
  
 因为请求的选项可能不适合于定义的游标 *\<stmt>* ，所以，此参数可同时用作输入和输出。 在此类情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分配一个适当的类型并修改此值。  
  
 *ccopt*  
 并发控制选项。 *ccopt* 是一个可选参数，它需要以下 **整数** 输入值之一。  
  
|值|说明|  
|-----------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS（以前称为 LOCKCC）|  
|0x0004|**乐观** (以前称为 OPTCC) |  
|0x0008|OPTIMISTIC（以前称为 OPTCCVAL）|  
|0x2000|ALLOW_DIRECT|  
|0x4000|UPDT_IN_PLACE|  
|0x8000|CHECK_ACCEPTED_OPTS|  
|0x10000|READ_ONLY_ACCEPTABLE|  
|0x20000|SCROLL_LOCKS_ACCEPTABLE|  
|0x40000|OPTIMISTIC_ACCEPTABLE|  
|0x80000|OPTIMISITC_ACCEPTABLE|  
  
 与 *scrollpt* 一样， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以分配一个与请求的值不同的值。  
  
 *数*  
 一个可选参数，指示要用于 AUTO_FETCH 的提取缓冲区行数。 默认值为 20 行。 指定为输入值与返回值时，*行计数* 的行为不同。  
  
|作为输入值|作为返回值|  
|--------------------|---------------------|  
|当指定 AUTO_FETCH 时，将 FAST_FORWARD cursor 行 *计数* ，表示要放入提取缓冲区中的行数。|表示结果集中的行数。 当指定 *scrollopt* AUTO_FETCH 值时， *rowcount* 返回提取到提取缓冲区中的行数。|  

*parameter_name* 指定 params 参数中定义的一个或多个参数名称。  必须为 params 中包含的每个参数提供一个参数。 如果未定义任何参数，则不需要此参数。
  
## <a name="return-code-values"></a>返回代码值  
 如果 params 返回 NULL 值，则不会对该语句进行参数化。  
  
## <a name="see-also"></a>另请参阅  
 [sp_cursoropen &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorexecute &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursorexecute-transact-sql.md)   
 [sp_cursorprepare &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursorprepare-transact-sql.md)   
 [sp_cursorfetch &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
