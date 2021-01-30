---
description: 参数数据类型
title: 参数数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], parameters
- parameter data type [ODBC]
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: fd7e99d8-d26a-408c-9733-6ffccde99f75
author: David-Engel
ms.author: v-daenge
ms.reviewer: ''
ms.openlocfilehash: fbc01ee7f6c4e7cd3ca7dbe4deb6e78e5ff78449
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160823"
---
# <a name="parameter-data-types"></a>参数数据类型
尽管使用 **SQLBindParameter** 指定的每个参数都是使用 sql 数据类型定义的，但 sql 语句中的参数没有内部数据类型。 因此，仅当参数标记的数据类型可以从语句中的另一个操作数推断出来时，才可以将其包含在 SQL 语句中。 例如，在算术表达式中，例如？ + COLUMN1，可以从 COLUMN1 所表示的命名列的数据类型推断出参数的数据类型。 如果无法确定数据类型，则应用程序不能使用参数标记。  
  
 下表说明了根据 SQL-92 如何确定多种类型参数的数据类型。 有关使用其他 SQL 子句时推断参数类型的更全面的规范，请参阅 SQL-92 规范。  
  
|参数的位置|假定的数据类型|  
|---------------------------|-----------------------|  
|二元算法或比较运算符的一个操作数|与另一个操作数相同|  
|**BETWEEN** 子句中的第一个操作数|与第二个操作数相同|  
|**BETWEEN** 子句中的第二个或第三个操作数|与第一个操作数相同|  
|与 **IN** 一起使用的表达式|与子查询的第一个值或结果列相同|  
|与 **IN** 一起使用的值|如果表达式中有参数标记，则与表达式或第一个值相同|  
|用于 **LIKE** 的模式值|VARCHAR|  
|与 **update** 一起使用的更新值|与更新列相同|
