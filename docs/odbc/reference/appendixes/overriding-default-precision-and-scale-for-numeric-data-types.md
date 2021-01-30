---
description: 重写数字数据类型的默认精度和小数位数
title: 为数值数据类型重写默认的精度和小数位数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- numeric data type [ODBC], precision and scale
- precision [ODBC], numeric data types
- data types [ODBC], numeric data types
- numeric data type [ODBC]
- numeric literals [ODBC]
ms.assetid: 84292334-0e33-4a1b-84de-8c018dd787f3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 007df3acd040b3b227563c84eb68c0de495f0aef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194797"
---
# <a name="overriding-default-precision-and-scale-for-numeric-data-types"></a>重写数字数据类型的默认精度和小数位数
当 ARD 中的 SQL_DESC_TYPE 字段设置为 SQL_C_NUMERIC （通过调用 **SQLBindCol** 或 **SQLSetDescField**）时，ARD 中的 SQL_DESC_SCALE 字段将设置为0，并且 SQL_DESC_PRECISION 字段设置为驱动程序定义的默认精度。 如果 APD 中的 SQL_DESC_TYPE 字段设置为 SQL_C_NUMERIC （通过调用 **SQLBindParameter** 或 **SQLSetDescField**），也会出现这种情况。 这适用于输入、输入/输出或输出参数。  
  
 如果某个应用程序不接受上述任何一个默认值，则该应用程序应通过调用 **SQLSetDescField** 或 **SQLSetDescRec** 来设置 SQL_DESC_SCALE 或 SQL_DESC_PRECISION 字段。  
  
 如果应用程序调用 **SQLGetData** 将数据返回 SQL_C_NUMERIC 结构，则将使用默认的 SQL_DESC_SCALE 和 SQL_DESC_PRECISION 字段。 如果默认值是不可接受的，则应用程序必须调用 **SQLSetDescRec** 或 **SQLSetDescField** 来设置字段，然后将 **SQLGetData** 与 SQL_ARD_TYPE 的 *TargetType* 一起调用，以使用 "描述符" 字段中的值。  
  
 调用 **SQLPutData** 时，调用将使用与执行时数据参数或列对应的说明符记录的 SQL_DESC_SCALE 和 SQL_DESC_PRECISION 字段，这是 APD 字段，用于调用 **SQLExecute** 或 **SQLExecDirect**，或用于调用 **ARD** 或 **SQLBulkOperations** 的 SQLSetPos 字段。
