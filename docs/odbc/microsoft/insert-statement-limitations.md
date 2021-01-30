---
description: INSERT 语句限制
title: INSERT 语句限制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, INSERT statement limitations
- INSERT statement limitations [ODBC]
- truncation of data [ODBC]
ms.assetid: dea05698-527a-41ab-8729-bbed85556185
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c8decef6ed2a17a2aca4b107d46b856b6ba828a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211187"
---
# <a name="insert-statement-limitations"></a>INSERT 语句限制
如果插入的数据太长而无法放入列中，则会在右侧截断插入的数据。  
  
 如果尝试插入的值超出列的数据类型的范围，则会导致在列中插入 NULL。  
  
 使用 dBASE、Microsoft Excel、Paradox 或 Textdriver 时，将长度为零的字符串插入列实际上是插入 NULL。  
  
 使用 Microsoft Excel 驱动程序时，如果插入列中的空字符串，则将空字符串转换为 NULL;使用 WHERE 子句中的空字符串执行的搜索 SELECT 语句对该列不会成功。  
  
 在以下两种情况下，不能通过 Paradox 驱动程序来更新表：  
  
-   表上未定义唯一索引时。 这对于空表并不适用，即使表中未定义唯一索引，也可以使用单个行更新该表。 如果在没有唯一索引的空表中插入单个行，则在插入单个行后，应用程序将无法创建唯一索引或插入其他数据。  
  
-   如果未实现 Borland 数据库引擎，则只允许在 Paradox 表中使用 read 和 append 语句。  
  
 使用文本驱动程序时，在固定长度的文件中用空白填充的字符串表示 NULL 值，但用分隔的文件中的空格表示。 例如，在包含三个字段的下一行中，第二个字段为 NULL 值：  
  
```  
"Smith:,, 123  
```  
  
 使用文本驱动程序时，所有列值都可以用前导空格填充。 任何行的长度必须小于或等于65543字节。
