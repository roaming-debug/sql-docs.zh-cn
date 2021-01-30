---
description: sp_cursorfetch (Transact-SQL)
title: sp_cursorfetch (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursorfetch
- sp_cursorfetch_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursorfetch
ms.assetid: 14513c5e-5774-4e4c-92e1-75cd6985b6a3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bd6ad166a48d259ab925ae9369abe0d73a73f13b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99126311"
---
# <a name="sp_cursorfetch-transact-sql"></a>sp_cursorfetch (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从数据库中提取由一行或多行组成的缓冲区。 此缓冲区中的行组称为游标的 *提取缓冲区*。 sp_cursorfetch 通过在表格格式数据流 (TDS) 数据包中指定 ID = 7 来调用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_cursorfetch cursor  
    [ , fetchtype[ , rownum [ , nrows] ]]   
```  
  
## <a name="arguments"></a>参数  
 *cursor*  
 是由生成并由 sp_cursoropen 返回的 *句柄* 值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *cursor* 是为 **int** 输入值调用的必需参数。 有关详细信息，请参阅本主题后面的 "备注" 部分。  
  
 *fetchtype*  
 指定要提取的游标缓冲区。 *fetchtype* 是一个可选参数，它需要以下整数输入值之一。  
  
|值|名称|说明|  
|-----------|----------|-----------------|  
|0x0001|FIRST|提取 *nrows* 行的第一个缓冲区。 如果 *nrows* 等于0，则游标位于结果集之前，不返回任何行。|  
|0x0002|NEXT|提取 *nrows* 行的下一个缓冲区。|  
|0x0004|PREV|提取 *nrows* 行的上一个缓冲区。<br /><br /> 注意：对 FORWARD_ONLY 游标使用 "上一步" 会返回一条错误消息，因为 FORWARD_ONLY 只支持单向滚动。|  
|0x0008|LAST|提取 *nrows* 行的最后一个缓冲区。 如果 *nrows* 等于0，则游标位于结果集之后，并且不返回任何行。<br /><br /> 注意：对 FORWARD_ONLY 游标使用 LAST 后，将返回错误消息，因为 FORWARD_ONLY 只支持单向滚动。|  
|0x10|ABSOLUTE|从 *rownum* 行开始提取 *nrows* 行的缓冲区。<br /><br /> 注意：对动态游标或 FORWARD_ONLY 游标使用绝对将返回错误消息，因为 FORWARD_ONLY 只支持单向滚动。|  
|0x20|RELATIVE|从指定为当前块中第一行的行的 *rownum* 值的行开始，提取 *nrows* 行的缓冲区。 在这种情况下， *rownum* 可以为负数。<br /><br /> 注意：对 FORWARD_ONLY 游标使用相对的会返回一条错误消息，因为 FORWARD_ONLY 只支持单向滚动。|  
|0x80|REFRESH|重新填充基础表中的缓冲区。|  
|0x100|INFO|检索有关游标的信息。 使用 *rownum* 和 *nrows* 参数返回此信息。 因此，当指定 INFO 时， *rownum* 和 *nrows* 将成为输出参数。|  
|0x200|PREV_NOADJUST|用法类似于 PREV。 但是，如果过早遇到结果集顶部，结果可能有所不同。|  
|0x400|SKIP_UPDT_CNCY|必须与其他 *fetchtype* 值之一一起使用，但 INFO 除外。|  
  
> [!NOTE]  
>  不支持值 0x40。  
  
 有关详细信息，请参阅本主题后面的 "备注" 部分。  
  
 *rownum*  
 是一个可选参数，用于通过只为输入或输出使用整数值指定绝对值和 INFO *fetchtype* 值的行位置。 *rownum* 用作 *FETCHTYPE* 位值相对的行偏移量。 对于所有其他值，将忽略 *rownum* 。 有关详细信息，请参阅本主题后面的 "备注" 部分。  
  
 *nrows*  
 一个可选参数，用于指定要提取的行数。 如果未指定 *nrows* ，则默认值为20行。 若要设置位置但不返回数据，请将值指定为0。 当 *nrows* 应用于 *fetchtype* INFO 查询时，它将返回该查询中的总行数。  
  
> [!NOTE]  
>  REFRESH *fetchtype* 位值将忽略 *nrows* 。  
>   
>  有关详细信息，请参阅本主题后面的 "备注" 部分。  
  
## <a name="return-code-values"></a>返回代码值  
 当指定位值 INFO 时，以下各表显示可能返回的值。  
  
> [!NOTE]  
>  ：如果不返回行，则缓冲区内容保持不变。  
  
|*\<rownum>*|设置为|  
|------------------|------------|  
|如果未打开|0|  
|如果定位在结果集前面|0|  
|如果定位在结果集后面|-1|  
|对于 KEYSET 游标和 STATIC 游标|结果集中当前位置的绝对行号|  
|对于 DYNAMIC 游标|1|  
|对于 ABSOLUTE|-1 返回集中的最后一行。<br /><br /> -2 返回集中的倒数第二行，依此类推。<br /><br /> 注意：如果请求在这种情况下提取多行，则返回结果集的最后两行。|  
  
|*\<nrows>*|设置为|  
|-----------------|------------|  
|如果未打开|0|  
|对于 KEYSET 游标和 STATIC 游标|通常为当前键集大小。<br /><br /> **-m** 如果光标处于异步创建中，则会在此点找到 *m* 行。|  
|对于 DYNAMIC 游标|-1|  
  
## <a name="remarks"></a>备注  
  
## <a name="cursor-parameter"></a>cursor 参数  
 在执行任何提取操作之前，游标的默认位置位于结果集第一行的前面。  
  
## <a name="fetchtype-parameter"></a>fetchtype 参数  
 除了 SKIP_UPD_CNCY 之外， *fetchtype* 值是相互排斥的。  
  
 如果指定 SKIP_UPDT_CNCY，则在提取或刷新行时，时间戳列的值不会写入到键集表中。 如果正在更新键集行，则时间戳列的值保持为先前的值。 如果正在插入键集行，则时间戳列的值不确定。  
  
 对于 KEYSET 游标，这意味着在执行最后一个非跳过 FETCH 的过程中（如果已执行一个），键集表设置了值。 否则，将在填充过程中设置值。  
  
 对于 DYNAMIC 游标，这意味着如果使用刷新来执行跳过，则它生成与 KEYSET 相同的结果。 对于任何其他提取类型，键集表会被截断。 这意味着正在插入行，且时间戳列的值不确定。 因此，当您针对 DYNAMIC 游标运行 sp_cursorfetch 时，对于除 REFRESH 之外的任何操作应避免使用 SKIP_UPDT_CNCY。  
  
 如果由于所请求的游标位置超出结果集而导致提取操作失败，则应将游标位置设置为恰好在最后一行的后面。 如果由于所请求的游标位置位于结果集的前面而导致提取操作失败，则应将游标位置设置在第一行的前面。  
  
## <a name="rownum-parameter"></a>rownum 参数  
 当使用 *rownum* 时，将从指定的行开始填充缓冲区。  
  
 *Fetchtype* 值绝对值指的是 *rownum* 在整个结果集中的位置。 具有 ABSOLUTE 的负数指定该操作从结果集的末尾开始对行进行计数。  
  
 *Fetchtype* 值相对引用相对于当前缓冲区开始处的游标位置的 *rownum* 位置。 具有 RELATIVE 的负数指定该游标从当前游标位置向后计数。  
  
## <a name="nrows-parameter"></a>nrows 参数  
 *Fetchtype* 值 REFRESH 和 INFO 将忽略此参数。  
  
 如果指定的 *fetchtype* 值为 FIRST，其 *nrow* 值为0，则游标将定位在提取缓冲区中没有行的结果集之前。  
  
 如果指定的 *fetchtype* 值为 *nrow* ，则值为0时，游标将位于当前提取缓冲区中没有行的结果集之后。  
  
 对于 "下一步"、"上一个"、"绝对"、"fetchtype" 和 "PREV_NOADJUST" 的 "  " 值，" *nrow* " 值无效。  
  
## <a name="rpc-considerations"></a>RPC 注意事项  
 RPC 返回状态指示键集大小参数是否为最终的（也即，键集表或临时表是否异步填充）。  
  
 RPC 状态参数设置为下表中显示的值之一。  
  
|值|说明|  
|-----------|-----------------|  
|0|过程已成功执行。|  
|0x0001|过程失败。|  
|0x0002|当处于负方向中的提取在逻辑上应位于结果集前面时，该提取可能导致将游标位置设置在结果集的开头。|  
|0x10|快进游标自动关闭。|  
  
 这些行将作为典型的结果集返回，也即：列格式 (0x2a)、行 (0xd1)，后跟完成 (0xfd)。 元数据标记使用与为 sp_cursoropen 指定的相同格式发送，也即：对于 SQL Server 7.0 用户为 0x81、0xa5 和 0xa4 等。 行状态指示器在具有列名称 rowstat 和数据类型 INT4 的每一行的末尾作为隐藏列发送（类似于 BROWSE 模式）。 此 rowstat 列具有下表中显示的值之一。  
  
|值|说明|  
|-----------|-----------------|  
|0x0001|FETCH_SUCCEEDED|  
|0x0002|FETCH_MISSING|  
  
 因为 TDS 协议不提供发送尾随状态列而不发送先前列的方法，所以为缺少的行发送虚数据（根据需要，将可以为 Null 的字段设置为 Null，将固定长度的字段设置为 0、空白或该列的默认值）。  
  
 DONE 行计数将始终为零。 DONE 消息包含实际结果集行计数，在任何 TDS 消息之间可能出现错误消息或信息性消息。  
  
 若要请求在 TDS 流中返回有关游标选择列表的元数据，请将 RPC RETURN_METADATA 输入标志设置为 1。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-prev-to-change-a-cursor-position"></a>A. 使用 PREV 更改游标位置  
 假定游标 h2 将生成具有以下内容的结果集，且当前位置如下所示：  
  
```  
row 1 contents      
row 2 contents  
row 3 contents  
row 4 contents  <-- current position  
row 5 contents   
row 6 contents  
```  
  
 接下来， *nrows* 的值为5的 sp_cursorfetch 将以逻辑方式将游标定位在结果集的第一行之前的两行之前。 在此类情况下，游标将调整为从第一行开始，并返回请求的行数。 这通常意味着它将返回位于 PRIOR 提取缓冲区中的行。  
  
> [!NOTE]  
>  这是 RPC 状态参数设置为 2 时的确切情况。  
  
### <a name="b-using-prev_noadjust-to-return-fewer-rows-than-prev"></a>B. 使用 PREV_NOADJUST 返回少于 PREV 的行  
 PREV_NOADJUST 从不包括任何位于它返回的行块中当前游标位置或在其之后的行。 如果上一项在当前位置后返回行，PREV_NOADJUST 返回的行数少于 *nrows* 中请求的行数。 前面的示例 A 中提供了当前位置，当应用 PREV 时，sp_cursorfetch（h2、4、1、5）将提取以下各行：  
  
```  
row1 contents   
row2 contents  
row3 contents  
row4 contents  
row5 contents  
```  
  
 但是，当应用 PREV_NOADJUST 时，sp_cursorfetch（h2、512、6、5）仅提取以下各行：  
  
```  
row1 contents   
row2 contents  
row3 contents   
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_cursoropen &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
