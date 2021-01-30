---
description: 文本文件数据类型
title: 文本文件数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- text file driver [ODBC], data types
- ODBC desktop database drivers [ODBC], text file driver
- desktop database drivers [ODBC], text file driver
- text file data types [ODBC]
- Jet-based ODBC drivers [ODBC], text file driver
ms.assetid: e113112e-ae42-469e-8e4b-a365a10d9071
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b1e447d2f20c8efb5e0c0cddc793baf73384e556
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182540"
---
# <a name="text-file-data-types"></a>文本文件数据类型
下表显示了文本数据类型如何映射到 ODBC SQL 数据类型。 请注意，ODBC 文本驱动程序并不支持所有 ODBC SQL 数据类型。  
  
|文本数据类型|ODBC 数据类型|  
|--------------------|--------------------|  
|CHAR|SQL_VARCHAR|  
|DATETIME|SQL_TIMESTAMP|  
|FLOAT|SQL_DOUBLE|  
|INTEGER|SQL_INTEGER|  
|LONGCHAR|SQL_LONGVARCHAR|  
  
> [!NOTE]  
>  **SQLGetTypeInfo** 返回 ODBC 数据类型。 对于上表中列出的 SQL 数据类型，支持 *ODBC 程序员参考* 的附录 D 中的所有转换。  
  
 下表显示了对文本数据类型的限制。  
  
|数据类型|说明|  
|---------------|-----------------|  
|CHAR|创建零或未指定长度的 CHAR 列实际上将返回255位的列。<br /><br /> 在分隔文件中，CHAR 列的开头和结尾不能有双引号分隔符;在固定长度的文件中，不会将双引号用作分隔符。|  
|DATETIME|MM-DD-YY (例如，01-17-92) <br /><br /> 月 MMM (例如，Jan-17-92) <br /><br /> 月 (，例如 17-01-92) <br /><br /> YYYY-MM-DD (例如，1992-01-17) <br /><br /> YYYY 年 MMM (例如，1992-01-17) <br /><br /> 表中不允许使用混合日期分隔符。<br /><br /> 文本 ISAM 根据 Windows 控制面板中的 "国际" 设置，将 "日期时间" 字段的格式设置为 "美国" 或 "欧洲"。|  
|FLOAT|最大宽度包括符号和小数点。 在 Schema.ini 中，宽度表示如下：<br /><br /> 14.083 为浮动宽度6<br /><br /> -14.083 为浮动宽度7<br /><br /> + 14.083 为浮动宽度7<br /><br /> 14083。是浮动宽度6<br /><br /> 对于 FLOAT 列，ODBC 始终返回8。<br /><br /> FLOAT 列还可以采用科学记数法，例如：<br /><br /> -3.04 e + 2 是 Float Width Width 8<br /><br /> 25E4 是浮动宽度4<br /><br /> **注意** 在列中不能混合使用 Decimal 和科学记数法。<br /><br /> 空值由固定长度文件中空白填充的字符串表示，在分隔文件中被忽略。<br /><br /> Float 数据可以用前导空格填充。|  
|INTEGER|整数列的有效值为32767到-32766。<br /><br /> 在 Schema.ini 中，宽度表示如下：<br /><br /> 14083的整数宽度为5<br /><br /> 0是整数宽度1<br /><br /> 对于整数列，ODBC 始终返回4。<br /><br /> 最大宽度包含符号。 整数列的最大宽度为11，不过，由于固定格式表中允许的空白，宽度可能会更大。|  
|LONGCHAR|固定长度或分隔表中 LONGCHAR 列宽度的理论限制为65500K。 文本 ISAM 更有可能提供最多约32K 的可靠支持。|  
  
 [数据类型限制](../../odbc/microsoft/data-type-limitations.md)中可以找到更多有关数据类型的限制。
