---
description: 转换规则
title: 转换规则 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- numeric data type [ODBC], literals
- conversions with numeric literals [ODBC]
- numeric literals [ODBC]
- literals [ODBC], numeric
ms.assetid: 89f846a3-001d-496a-9843-ac9c38dc1762
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b015a60e376e47a4c3c931d4739a884db23fc98b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187131"
---
# <a name="rules-for-conversions"></a>转换规则
本部分中的规则适用于涉及数值的转换。 对于这些规则，定义了以下术语：  
  
-   *存储分配：* 将数据发送到数据库中的表列时。 在调用 **SQLExecute**、 **SQLExecDirect** 和 **SQLSetPos** 的过程中会出现这种情况。 在存储分配过程中，"目标" 指的是数据库列，而 "源" 指的是应用程序缓冲区中的数据。  
  
-   *检索分配：* 在将数据从数据库检索到应用程序缓冲区时。 在调用 **SQLFetch**、 **SQLGetData**、 **SQLFetchScroll** 和 **SQLSetPos** 的过程中会出现这种情况。 在检索分配过程中，"target" 指的是应用程序缓冲区，"source" 指的是数据库列。  
  
-   *CS：* 字符源中的值。  
  
-   *NT：* 数值目标中的值。  
  
-   *NS：* 数值源中的值。  
  
-   *CT：* 字符目标中的值。  
  
-   精确数值的精度：它包含的位数。  
  
-   精确数值的小数位数：表示或隐含时间段右侧的位数。  
  
-   近似数字文本的精度：其尾数的精度。  
  
## <a name="character-source-to-numeric-target"></a>字符源到数值目标  
 下面是从字符源 (CS) 转换为 (NT) 的数值目标的规则：  
  
1.  将 CS 替换为通过删除 CS 中的任何前导空格或尾随空格获取的值。 如果 CS 不是有效的 *数字文本*，则返回 SQLSTATE 22018 (无效的转换规格) 字符值。  
  
2.  将 CS 替换为获取的值，方法是删除小数点前面的前导零、小数点后的尾随零或两者。  
  
3.  将 CS 转换为 NT。 如果转换导致有效位数丢失，则返回 SQLSTATE 22003 (数值超出范围) 。 如果转换导致 nonsignificant 的数字丢失，则返回 SQLSTATE 01S07 (小数截断) 。  
  
## <a name="numeric-source-to-character-target"></a>数值源到字符目标  
 下面是从数值源 (NS) 转换为字符目标 (CT) 的规则：  
  
1.  让 LT 为 CT 的字符长度。 对于检索分配，LT 等于该字符集的 null 终止字符中的字节数（以字符为单位）。  
  
2.  这  
  
    -   如果 NS 是精确的数值类型，则让 YP 等于符合 *精确数字文本* 定义的最短字符串，以便 YP 的小数位数与 ns 的小数位数相同，而 YP 的解释值为 ns 的绝对值。  
  
    -   如果 NS 是近似数值类型，则让 YP 为字符串，如下所示：  
  
         大小写：  
  
         如果 NS 等于0，则 YP 为0。  
  
         让 YSN 是符合精确 *数字文本* 定义的最短字符串，其解释值为 NS 的绝对值。 如果 YSN 的长度小于 NS 的数据类型 (*精度* + 1) ，则允许 YP 等于 YSN。  
  
         否则，YP 是符合 *近似数字文本* 的定义的最短字符串，其解释值为 NS 的绝对值，其 *尾数* 包含一个不是 "0" 的 *数字* ，后跟一个 *句点* 和一个 *无符号整数*。  
  
3.  大小写：  
  
    -   如果 NS 小于0，则让 Y 成为的结果：  
  
         "-"  &#124;&#124; YP  
  
         其中，"&#124;&#124;" 是字符串串联运算符。  
  
         否则，让 Y 等于 YP。  
  
4.  让 LY 以 Y 字符为长度。  
  
5.  大小写：  
  
    -   如果 LY 等于 LT，则 CT 设置为 Y。  
  
    -   如果 LY 小于 LT，则在适当的空格数后，CT 设置为 Y 的 "扩展"。  
  
         否则 (LY > LT) ，将 Y 的第一个 LT 字符复制到 CT。  
  
         大小写：  
  
         如果这是一个存储分配，将返回错误 SQLSTATE 22001 (字符串数据，并按) 右截断。  
  
         如果此为检索分配，将返回警告 SQLSTATE 01004 (字符串数据，并将) 右截断。 如果复制导致小数位丢失 (尾随零) 以外，则它是由驱动程序定义的，无论是否发生以下情况之一：  
  
          (1) 驱动程序将 Y 中的字符串截断为适当的小数位数 (也可以为零) 并将结果写入 CT。  
  
          (2) 驱动程序将 Y 中的字符串舍入为适当的小数位数 (也可以为零) 并将结果写入 CT。  
  
          (3) 驱动程序既不截断也不舍入，而只是将 Y 的第一个 LT 字符复制到 CT。
