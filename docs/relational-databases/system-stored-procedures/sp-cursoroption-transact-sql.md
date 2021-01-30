---
description: sp_cursoroption (Transact-SQL)
title: sp_cursoroption (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursoroption_TSQL
- sp_cursoroption
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursoroption
ms.assetid: 88fc1dba-f4cb-47c0-92c2-bf398f4a382e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 787c54be4c1030f9c091dbfead90c569a63bb270
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210473"
---
# <a name="sp_cursoroption-transact-sql"></a>sp_cursoroption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  设置游标选项或返回由 sp_cursoropen 存储过程创建的游标信息。 sp_cursoroption 通过在表格格式数据流 (TDS) 包中指定 ID = 8 来调用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_cursoroption cursor, code, value  
```  
  
## <a name="arguments"></a>参数  
 *cursor*  
 是由生成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并由 sp_cursoropen 存储过程返回的句柄值。 *游标* 需要用于执行的 **int** 输入值。  
  
 *code*  
 用于规定游标返回值的不同因素。 *代码* 需要以下 **int** 输入值之一：  
  
|值|名称|说明|  
|-----------|----------|-----------------|  
|0x0001|TEXTPTR_ONLY|返回某些指定文本或图像列的文本指针，而非实际数据。<br /><br /> TEXTPTR_ONLY 允许将文本指针用作 blob 对象的 *句柄* ，稍后可以使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 或 DBLIB (设施（例如 [!INCLUDE[tsql](../../includes/tsql-md.md)] READTEXT 或 DBLIB DBWRITETEXT) ）有选择地检索或更新它们。<br /><br /> 如果分配了值“0”，则选择列表中的所有文本和图像列将返回文本指针而非数据。|  
|0x0002|CURSOR_NAME|将 " *值* " 中指定的名称分配给游标。 这反过来允许 ODBC 对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 通过 sp_cursoropen 打开的游标使用定位的 UPDATE/DELETE 语句。<br /><br /> 可以将此字符串指定为任何字符或 Unicode 数据类型。<br /><br /> 由于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 定位的 update/DELETE 语句在默认情况下会在 fat 游标的第一行中运行，因此在发出定位的 UPDATE/DELETE 语句之前，应使用 SP_CURSOR SETPOSITION 来定位游标。|  
|0x0003|TEXTDATA|为后续提取中的某些文本或图像列返回实际数据，而非文本指针（也即，此操作撤消了 TEXTPTR_ONLY 的效果）。<br /><br /> 如果为某特定列启用了 TEXTDATA，则将重新提取或刷新此行，然后将它发送回 TEXTPTR_ONLY。 借助于 TEXTPTR_ONLY，值参数是一个整数，它指定列编号，并且零值返回所有文本或图像列。|  
|0x0004|SCROLLOPT|滚动选项。 有关其他信息，请参阅本主题后面的“返回代码值”。|  
|0x0005|CCOPT|并发控制选项。 有关其他信息，请参阅本主题后面的“返回代码值”。|  
|0x0006|ROWCOUNT|结果集中的当前行数。<br /><br /> 注意：如果正在使用异步填充，则在 sp_cursoropen 返回的值之后，行计数可能已更改。 如果行数未知，则返回值-1。|  
  
 *value*  
 指定由 *代码* 返回的值。 *值* 是一个必需参数，它调用0x0001、0x0002 或 0x0003 *代码* 输入值。  
  
> [!NOTE]  
>  *代码* 值2是字符串数据类型。 任何其他 *代码* 值输入或按 *值* 返回均为整数。  
  
## <a name="return-code-values"></a>返回代码值  
 *Value* 参数可能会返回以下 *代码* 值之一。  
  
|返回值|说明|  
|------------------|-----------------|  
|0x0004|SCROLLOPT|  
|0X0005|CCOPT|  
|0X0006|ROWCOUNT|  
  
 *Value* 参数返回以下 SCROLLOPT 值之一。  
  
|返回值|说明|  
|------------------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
  
 *Value* 参数返回以下 CCOPT 值之一。  
  
|返回值|说明|  
|------------------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS|  
|0x0004 或 0x0008|OPTIMISTIC|  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_cursor &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursor-transact-sql.md)   
 [sp_cursoropen &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)  
  
  
