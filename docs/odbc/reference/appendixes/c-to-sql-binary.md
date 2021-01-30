---
description: 从 C 到 SQL：二进制
title: C 到 SQL：二进制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- binary data type [ODBC]
- data conversions from C to SQL types [ODBC], binary
- binary data transfers [ODBC]
- converting data from c to SQL types [ODBC], binary
ms.assetid: 3e9083f3-357b-41aa-833c-2c8aac2226cd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2e3e1d197a1999c6502848d92222f1c3d6ad5d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212430"
---
# <a name="c-to-sql-binary"></a>从 C 到 SQL：二进制
二进制 ODBC C 数据类型的标识符是：  
  
 SQL_C_BINARY  
  
 下表显示了二进制 C 数据可转换为的 ODBC SQL 数据类型。 有关表中的列和字词的说明，请参阅 [将数据从 C 转换为 SQL 数据类型](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)。  
  
|SQL 类型标识符|测试|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|数据 <的字节长度 = 列字节长度<br /><br /> 数据的字节长度 > 列字节长度|不适用<br /><br /> 22001|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|数据的字符长度 <= 列字符长度<br /><br /> 数据的字符长度 > 列字符长度|不适用<br /><br /> 22001|  
|SQL_DECIMAL<br /><br /> SQL_NUMERIC<br /><br /> SQL_TINYINT<br /><br /> SQL_SMALLINT<br /><br /> SQL_INTEGER<br /><br /> SQL_BIGINT<br /><br /> SQL_REAL<br /><br /> SQL_FLOAT<br /><br /> SQL_DOUBLE<br /><br /> SQL_BIT SQL_TYPE_DATE<br /><br /> SQL_TYPE_TIME<br /><br /> SQL_TYPE_TIMESTAMP|数据的字节长度 = SQL 数据长度<br /><br /> 数据的字节长度 <> SQL 数据长度|不适用<br /><br /> 22003|  
|SQL_BINARY<br /><br /> SQL_VARBINARY<br /><br /> SQL_LONGVARBINARY|数据 <长度 = 列长度<br /><br /> 数据 > 列长度的长度|不适用<br /><br /> 22001|
