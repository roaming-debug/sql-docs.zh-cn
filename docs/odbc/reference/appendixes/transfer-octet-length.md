---
description: 传输八位字节长度
title: 传输八位字节长度 |Microsoft Docs
ms.custom: ''
ms.date: 10/28/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transfer octet length of data types [ODBC]
- size of data types [ODBC]
- SQL data types [ODBC], column characteristics
- data types [ODBC], transfer octet length
ms.assetid: 9fdc9762-e203-4cff-9212-54f450bf18d9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b910f544be952a18ae2961e9939960820e3805bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202483"
---
# <a name="transfer-octet-length"></a>传输八位字节长度
在将数据传输到默认 C 数据类型时，列的传输的字节数是指返回给应用程序的最大字节数。 对于字符数据，传输八位字节长度不包括 null 终止字符的空间。 列的传输八位字节长度可能不同于在数据源中存储数据所需的字节数。  
  
 下表显示了为每个 ODBC SQL 数据类型定义的传输八位字节长度。  
  
|SQL 类型标识符|长度|  
|-------------------------|------------|  
|所有字符类型 [a]|为变量类型指定的或最大 (列长度的) 长度（以字节为单位）。 此值与描述符字段 SQL_DESC_OCTET_LENGTH 相同。|  
|SQL_DECIMAL<br />SQL_NUMERIC|如果字符集为 ANSI，则保留此数据的字符表示形式所需的字节数; 如果字符集为 UNICODE，则为此数字的两倍。 这是位数的最大数目加两，因为数据以字符串形式返回，而数字、符号和小数点需要字符。 例如，定义为数值 (10，3) 的列的传输长度为12。|  
|SQL_TINYINT|1|  
|SQL_SMALLINT|2|  
|SQL_INTEGER|4|  
|SQL_BIGINT| 8 |  
|SQL_REAL|4|  
|SQL_FLOAT|8|  
|SQL_DOUBLE|8|  
|SQL_BIT|1|  
|所有二进制类型 [a]|为固定类型保留定义的 (所需的字节数) 或变量类型的最大 () 字符数。|  
|SQL_TYPE_DATE<br />SQL_TYPE_TIME|6 (SQL_DATE_STRUCT 或 SQL_TIME_STRUCT 结构) 的大小。|  
|SQL_TYPE_TIMESTAMP|16 (SQL_TIMESTAMP_STRUCT 结构) 大小。|  
|所有间隔数据类型|34 (间隔结构) 的大小。|  
|SQL_GUID|16 (GUID 结构) 的大小。|  
| &nbsp; | &nbsp; |

 [a] 如果驱动程序无法确定变量类型的列或参数长度，它将返回 SQL_NO_TOTAL。
