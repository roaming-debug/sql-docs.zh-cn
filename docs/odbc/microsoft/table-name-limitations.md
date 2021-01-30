---
description: 表名称限制
title: 表名限制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, table name limitations
- table name limitations [ODBC]
- Excel driver [ODBC], table name limitations
ms.assetid: d9843e4b-1d05-4d5a-9dc3-ee9ec59edb97
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1b3a13919a20a8092c730ce3dfbe35358d1898df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182548"
---
# <a name="table-name-limitations"></a>表名称限制
表名可以包含任何有效的字符 (例如，空格) 。 如果表名包含除字母、数字和下划线之外的任何字符，则必须用引号将该名称括起来 (") 。  
  
 当使用 Microsoft Excel 驱动程序时，如果数据库引用不限定表名，则默认数据库是隐含的。 如果 Microsoft Excel 中的某个名称包含 "！" 字符，则它将自动转换为 "$" 字符。  
  
 \<filename>Microsoft excel 3.0 和4.0 文件支持引用的 Microsoft excel 表名称。 \<workbook-name>Microsoft excel 5.0、7.0 或97文件支持引用的 Microsoft excel 表名称。  
  
 使用 dBASE 驱动程序时，将 ASCII 值大于127的字符转换为下划线。  
  
 使用 Microsoft Access 驱动程序时，表名称限制为64个字符。  
  
 使用 dBASE、Microsoft Excel 3.0 或4.0、Paradox 或文本驱动程序时，不应将特殊的 MS-DOS 关键字 CON、AUX、LPT1 和 LPT2 用作表名称。
