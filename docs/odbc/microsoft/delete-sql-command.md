---
description: DELETE - SQL 命令
title: DELETE-SQL 命令 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DELETE [ODBC]
ms.assetid: 0d5bd477-626f-4f22-a05a-f531d9f8c5e7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42c5ff48b0ac330c2a305650141ada2255629c1f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200506"
---
# <a name="delete---sql-command"></a>DELETE - SQL 命令
标记要删除的记录。  
  
 Visual FoxPro ODBC 驱动程序支持此命令的本机 Visual FoxPro 语言语法。 有关特定于驱动程序的信息，请参阅 "备注"。  
  
## <a name="syntax"></a>语法  
  
```  
  
DELETE FROM [DatabaseName!]TableName  
   [WHERE FilterCondition1 [AND | OR FilterCondition2 ...]]  
```  
  
## <a name="arguments"></a>参数  
 从 [ *DatabaseName！*] *TableName*  
 指定将记录标记为删除的表。  
  
 *Database!* 如果包含数据库不是使用数据源指定的数据库，则指定包含表的数据库的名称。 如果数据库不是使用数据源指定的数据库，则必须包含包含表的数据库的名称。 将感叹号 (！ ) 分隔符添加到数据库名称之后、表名称之前。  
  
 WHERE *FilterCondition1*[AND &#124; OR *FilterCondition2*...]  
 指定视觉 FoxPro 仅标记某些要删除的记录。  
  
 *FilterCondition* 指定要标记为删除的记录必须满足的条件。 您可以根据需要包含多个筛选条件，并将它们与 AND 或 OR 运算符连接。 您也可以使用 NOT 运算符来反转逻辑表达式的值，也可以使用 **空** 的 ( ) 检查是否有空字段。  
  
## <a name="remarks"></a>备注  
 如果设置为 "已删除"，则所有包含作用域的命令都将忽略标记为删除的记录。  
  
 在为共享访问打开的表中标记要删除的多个记录时，删除-SQL 使用记录锁定。 这会减少多用户情况下的记录争用，但会降低性能。 为了获得最佳性能，请打开表以供独占使用。  
  
## <a name="driver-remarks"></a>驱动程序备注  
 当应用程序将 ODBC SQL 语句删除发送到数据源时，Visual FoxPro ODBC 驱动程序会将命令转换为 Visual FoxPro DELETE 命令，而无需进行转换。  
  
## <a name="see-also"></a>另请参阅  
 [SET DELETED 命令](../../odbc/microsoft/set-deleted-command.md)
