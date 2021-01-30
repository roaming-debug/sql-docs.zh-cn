---
description: UPDATE - SQL 命令
title: UPDATE-SQL 命令 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- update [ODBC]
ms.assetid: ff1e0331-c060-4304-b280-039725b45f63
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3a3e38ad98f69faf2ed6288c767d43167314465b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209828"
---
# <a name="update---sql-command"></a>UPDATE - SQL 命令
使用新值更新表中的记录。  
  
 Visual FoxPro ODBC 驱动程序支持此命令的本机 Visual FoxPro 语言语法。 有关特定于驱动程序的信息，请参阅 **驱动程序备注**。  
  
## <a name="syntax"></a>语法  
  
```  
  
UPDATE [DatabaseName1!]TableName1  
SET Column_Name1 = eExpression1  
   [, Column_Name2 = eExpression2 ...]  
   WHERE FilterCondition1 [AND | OR FilterCondition2 ...]  
```  
  
## <a name="arguments"></a>参数  
 UPDATE [ *DatabaseName1！*] *TableName1*  
 指定用新值更新记录的表。  
  
 *DatabaseName1!* 指定数据库的名称，而不是使用包含表的数据源指定的数据库的名称。 如果数据库不是当前数据库，则必须包含该表的名称。 将感叹号 (！ ) 分隔符添加到数据库名称之后、表名称之前。  
  
 SET *Column_Name1* =  *eExpression1*[， *Column_Name2* =  *eExpression2*  
 指定更新的列及其新值。 如果省略 WHERE 子句，则将用相同的值更新列中的每一行。  
  
 WHERE *FilterCondition1*[AND &#124; OR *FilterCondition2*...]  
 指定更新了新值的记录。  
  
 *FilterCondition* 指定要用新值更新的记录必须满足的条件。 你可以根据需要包含多个筛选条件，并将它们与 AND 或 OR 运算符连接。 您也可以使用 NOT 运算符来反转逻辑表达式的值，也可以使用 **空** 的 ( ) 检查是否有空字段。  
  
## <a name="remarks"></a>备注  
 UPDATE-SQL 只能更新单个表中的记录。  
  
 与 REPLACE 不同，UPDATE-SQL 在更新为共享访问打开的表中的多个记录时使用记录锁定。 这会减少多用户情况下的记录争用，但会降低性能。 为了获得最佳性能，请打开表以供独占使用，或使用 **FLOCK** ( ) 锁定表。  
  
## <a name="driver-remarks"></a>驱动程序备注  
 当应用程序将 ODBC SQL 语句更新发送到数据源时，Visual FoxPro ODBC 驱动程序会将命令转换为 Visual FoxProUPDATE 命令，而不进行转换。  
  
## <a name="see-also"></a>另请参阅  
 [DELETE-SQL 命令](../../odbc/microsoft/delete-sql-command.md)   
 [INSERT - SQL 命令](../../odbc/microsoft/insert-sql-command.md)
