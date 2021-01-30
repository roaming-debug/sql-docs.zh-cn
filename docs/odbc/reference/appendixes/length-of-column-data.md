---
description: 列数据的长度
title: 列数据的长度 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- column data [ODBC]
- length of column data [ODBC]
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: c762c881-ebe0-4eac-84d5-f30281fc3eca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: be0ad52f040c195d4e8eeab9d307aa8f92bcf62d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184399"
---
# <a name="length-of-column-data"></a>列数据的长度
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 游标库在缓存中为绑定到结果集的每个长度/指示器缓冲区在 **SQLBindCol** 中创建一个缓冲区。 它在模拟定位的 update 或 delete 语句时，使用这些缓冲区中的值来构造 **WHERE** 子句。 它在从数据源提取数据时和在执行定位的 update 语句时，从行集缓冲区中更新这些缓冲区。  
  
 如果数据缓冲区的 C 类型为 SQL_C_CHAR 或 SQL_C_BINARY，且长度/指示器值 SQL_NTS，则将数据的字符串长度放入长度/指示器缓冲区。  
  
> [!NOTE]  
>  如果相应行集缓冲区中的 **StrLen_or_IndPtr* SQL_DATA_AT_EXEC 或 SQL_LEN_DATA_AT_EXEC 宏的结果，则游标库不会为该列更新其缓存。
