---
description: 显示大小
title: 显示大小 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- display size of data types [ODBC]
- size of data types [ODBC]
- data types [ODBC], display size
- SQL data types [ODBC], column characteristics
ms.assetid: 9f7f766f-2492-463c-aab7-f2476e222042
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a39a0f986164cf6bc14e8be6e221232dd936e4bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194876"
---
# <a name="display-size"></a>显示大小
列的显示大小是以字符形式显示数据所需的最大字符数。 下表定义了每个 ODBC SQL 数据类型的显示大小。  
  
|SQL 类型标识符|屏幕大小|  
|-------------------------|------------------|  
|所有字符类型 [a]|为固定类型定义的 () 或最大变量类型 (，) 以字符形式显示数据所需的字符数。|  
|SQL_DECIMAL SQL_NUMERIC|列和 2 (符号、 *精度* 数字和小数点) 的精度。 例如，列的显示大小 (10，3) 定义为12。|  
|SQL_BIT|1 (1 位) 。|  
|SQL_TINYINT|如果带符号的 (符号和3位) ，则为 4; 如果符号 (3 位数) ，则为3。|  
|SQL_SMALLINT|如果签名 (符号和 5) 位数字，则为 6; 如果符号 (5 位数) ，则为5。|  
|SQL_INTEGER|11如果签名 (符号和10位数) 或10（如果无符号 (10 位数) ）。|  
|SQL_BIGINT|20 (带有符号的数字和19位数字（如果有符号) ，则为20位数字）。|  
|SQL_REAL|14 (符号、7位数、小数点、字母 *E*、正负号和2位数) 。|  
|SQL_FLOAT SQL_DOUBLE|24 (一个符号、15位数、小数点、字母 *E*、正负号和3位) 。|  
|所有二进制类型 [a]|变量类型的已定义或最大 (列的长度) 倍。 每个二进制字节 (用2位十六进制数字表示。 ) |  
|SQL_TYPE_DATE|10以 *yyyy-mm-dd*) 格式 (日期。|  
|SQL_TYPE_TIME|8 (格式为 *hh： mm： ss* 的时间) <br /><br /> - 或 -<br /><br /> 9 + *s* (格式为 *hh： mm： ss*[. fff ...] 的时间，其中 *s* 为秒的小数部分精度) 。|  
|SQL_TYPE_TIMESTAMP|19 (*yyyy-mm-dd hh： mm： ss* 格式的时间戳) <br /><br /> - 或 -<br /><br /> 对于 *yyyy-mm-dd hh： mm： ss*[node.js ...] 格式的时间戳，20个 *以上的* (，其中 *s* 为秒的小数部分精度) 。|  
|所有间隔数据类型|请参阅 [间隔数据类型长度](../../../odbc/reference/appendixes/interval-data-type-length.md)。|  
|SQL_GUID|36 (*aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee* 格式的字符数|  
  
 [a] 如果驱动程序无法确定变量类型的列或参数长度，它将返回 SQL_NO_TOTAL。
