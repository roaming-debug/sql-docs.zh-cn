---
description: 列数据
title: 列数据 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- column data [ODBC]
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: 0425818c-9469-493f-9e3c-fc03d9411c5c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1287a4deadc6f35bf83f6c42dcebc633f6557fed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212348"
---
# <a name="column-data"></a>列数据
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 游标库在缓存中为绑定到结果集的每个数据缓冲区在 **SQLBindCol** 中创建一个缓冲区。 它在模拟定位的 update 或 delete 语句时，使用这些缓冲区中的值来构造 **WHERE** 子句。 它在从数据源提取数据时和在执行定位的 update 语句时，从行集缓冲区中更新这些缓冲区。  
  
 当游标库从行集缓冲区更新缓存时，它将根据 **SQLBindCol** 中指定的 C 数据类型传输数据。 例如，如果行集缓冲区的 C 数据类型为 SQL_C_SLONG，则游标库传输四个字节的数据;如果 SQL_C_CHAR 并且 *BufferLength* 为10，则游标库将传输10个字节的数据。 游标库不对其传输的数据执行任何类型检查或转换。  
  
> [!NOTE]  
>  如果相应行集缓冲区中的 **StrLen_or_IndPtr* SQL_DATA_AT_EXEC 或 SQL_LEN_DATA_AT_EXEC 宏的结果，则游标库不会为该列更新其缓存。  
  
 更新列时，数据源将填充固定长度的固定长度字符数据，并在必要时填充固定长度的固定长度的二进制数据。 例如，一个数据源在 CHAR (10) 列中存储 "smith" 作为 "Smith"。 游标库在执行定位的 update 语句后将此数据复制到其缓存后，不会将此数据复制到其缓存中，而不会将此数据复制到其缓存中。 因此，如果应用程序要求游标库的缓存中的值为空白填充的值或填充了零的值，则在执行定位的 update 语句之前，它必须为空或零填充行集缓冲区中的值。
