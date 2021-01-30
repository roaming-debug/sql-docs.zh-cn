---
description: 从 SQL 到 C：二进制
title: SQL 到 C：二进制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to c types [ODBC], binary
- binary data type [ODBC]
- data conversions from SQL to C types [ODBC], binary
- binary data transfers [ODBC]
ms.assetid: 8c519072-ae4c-4d32-9d4e-775e3d3d6389
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 30fbabde654f69af7a0cb68e2606c0f2d93697c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160820"
---
# <a name="sql-to-c-binary"></a>从 SQL 到 C：二进制
二进制 ODBC SQL 数据类型的标识符为：  
  
 SQL_BINARY  
  
 SQL_VARBINARY  
  
 SQL_LONGVARBINARY  
  
 下表显示了二进制 SQL 数据可转换为的 ODBC C 数据类型。 有关表中的列和字词的说明，请参阅将 [数据从 SQL 转换为 C 数据类型](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)。  
  
|C 类型标识符|测试|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|数据的 (字节长度) \* 2 < *BufferLength*<br /><br /> 数据) 2 (字节长度 \* >= *BufferLength*|数据<br /><br /> 截断的数据|数据的长度（以字节为单位）<br /><br /> 数据的长度（以字节为单位）|不适用<br /><br /> 01004|  
|SQL_C_WCHAR|数据) 2 的 (字符长度 \* < *BufferLength*<br /><br /> 数据) 2 (字符长度 \* >= *BufferLength*|数据<br /><br /> 截断的数据|数据的长度（以字符为长度）<br /><br /> 数据的长度（以字符为长度）|不适用<br /><br /> 01004|  
|SQL_C_BINARY|Data <的字节长度 = *BufferLength*<br /><br /> 数据 > 的字节长度 *BufferLength*|数据<br /><br /> 截断的数据|数据的长度（以字节为单位）<br /><br /> 数据的长度（以字节为单位）|不适用<br /><br /> 01004|  
  
 当二进制 SQL 数据转换为字符 C 数据时，每个字节 (8 位) 源数据将表示为两个 ASCII 字符。 这些字符是数字的十六进制形式的 ASCII 字符表示形式。 例如，二进制00000001转换为 "01"，二进制11111111转换为 "FF"。  
  
 驱动程序始终将单个字节转换为十六进制数字对，并使用 null 字节终止字符串。 因此，如果 *BufferLength* 为偶数并且小于转换后的数据的长度，则不会使用 **TargetValuePtr* 缓冲区的最后一个字节。  (转换后的数据需要偶数个字节，第二个字节为空字节，不能使用最后一个字节。 )   
  
> [!NOTE]  
>  不建议应用程序开发人员将二进制 SQL 数据绑定到字符 C 数据类型。 这种转换通常效率低下且速度缓慢。
