---
description: 从 SQL 到 C：GUID
title: SQL 到 C： GUID |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], GUID
- data conversions from SQL to C types [ODBC], guid
- GUID data type [ODBC]
ms.assetid: cf56c684-c261-4b89-994a-db14ab2241d6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9b55b88414dfa78b80c49987af6dab82ba4abeda
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203025"
---
# <a name="sql-to-c-guid"></a>从 SQL 到 C：GUID
GUID ODBC SQL 数据类型的标识符是：  
  
 SQL_GUID  
  
 下表显示了可将 GUID SQL 数据转换到的 ODBC C 数据类型。 有关表中的列和字词的说明，请参阅将 [数据从 SQL 转换为 C 数据类型](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)。  
  
|C 类型标识符|测试|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|*BufferLength* > 字符字节长度|数据|36|不适用|  
||*BufferLength* < 37|Undefined|Undefined|22003|  
|SQL_C_WCHAR|*BufferLength* > 字符长度|数据|36|不适用|  
||*BufferLength* < 37|Undefined|Undefined|22003|  
|SQL_C_BINARY|数据 \< =  *BufferLength* 的字节长度|数据|数据的长度（以字节为单位）|不适用|  
||数据 > 的字节长度 *BufferLength*|Undefined|Undefined|22003|  
|SQL_C_GUID|None [a]|数据|16 [b]|不适用|  
  
 [a] 此转换将忽略 *BufferLength* 的值。 驱动程序假设大小 **TargetValuePtr* 是 C 数据类型的大小。  
  
 [b] 这是对应的 C 数据类型的大小。
