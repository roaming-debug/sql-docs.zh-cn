---
description: 显式数据类型转换函数
title: 显式数据类型转换函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- explicit data type conversion functions [ODBC]
- data type conversion functions [ODBC]
- functions [ODBC], explicit data type conversion functions
ms.assetid: d5789450-b668-4753-96c8-6789e955e7ed
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 927c911b839e7aa07b087edb0fb3b457d0825b6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194815"
---
# <a name="explicit-data-type-conversion-function"></a>显式数据类型转换函数
显式数据类型转换是根据 SQL 数据类型定义进行指定的。  
  
 显式数据类型转换函数的 ODBC 语法不限制转换。 将一种数据类型转换为另一种数据类型的特定转换的有效性取决于每个特定于驱动程序的实现。 驱动程序将将 ODBC 语法转换为本机语法，并拒绝那些虽然数据源不支持在 ODBC 语法中合法的转换。 ODBC 函数 **SQLGetInfo**（包含转换选项 (如 SQL_CONVERT_BIGINT、SQL_CONVERT_BINARY、SQL_CONVERT_INTERVAL_YEAR_MONTH 等) ）提供了一种方法来查询数据源支持的转换。  
  
 **CONVERT** 函数的格式为：  
  
 **转换 (** _value_exp_， _data_type_ **)**  
  
 函数返回由 *value_exp* 转换为指定的 *data_type* 指定的值，其中 *data_type* 是以下关键字之一：  

:::row:::
    :::column:::
        SQL_BIGINT  
        SQL_BINARY  
        SQL_BIT  
        SQL_CHAR  
        SQL_DATE  
        SQL_DECIMAL  
        SQL_DOUBLE  
        SQL_FLOAT  
        SQL_GUID  
        SQL_INTEGER  
        SQL_INTERVAL_DAY  
        SQL_INTERVAL_DAY_TO_HOUR  
    :::column-end:::
    :::column:::
        SQL_INTERVAL_DAY_TO_MINUTE  
        SQL_INTERVAL_DAY_TO_SECOND  
        SQL_INTERVAL_HOUR  
        SQL_INTERVAL_HOUR_TO_MINUTE  
        SQL_INTERVAL_HOUR_TO_SECOND  
        SQL_INTERVAL_MINUTE  
        SQL_INTERVAL_MINUTE_TO_SECOND  
        SQL_INTERVAL_MONTH  
        SQL_INTERVAL_SECOND  
        SQL_INTERVAL_YEAR  
        SQL_INTERVAL_YEAR_TO_MONTH  
        SQL_LONGVARBINARY  
    :::column-end:::
    :::column:::
        SQL_LONGVARCHAR  
        SQL_NUMERIC  
        SQL_REAL  
        SQL_SMALLINT  
        SQL_TIME  
        SQL_TIMESTAMP  
        SQL_TINYINT  
        SQL_VARBINARY  
        SQL_VARCHAR  
        SQL_WCHAR  
        SQL_WLONGVARCHAR  
        SQL_WVARCHAR  
    :::column-end:::
:::row-end:::

 显式数据类型转换函数的 ODBC 语法不支持转换格式的规范。 如果基础数据源支持显式格式规范，则驱动程序必须指定默认值或实现格式规范。  
  
 参数 *value_exp* 可以是列名称、其他标量函数的结果，也可以是数字或字符串。 例如：  
  
```  
{ fn CONVERT( { fn CURDATE() }, SQL_CHAR ) }  
```  
  
 将 CURDATE 标量函数的输出转换为字符串。  
  
 由于 ODBC 不会为标量函数的返回值强制数据类型 (因为函数通常是特定于数据源的) ，所以，应用程序应尽可能使用 CONVERT 标量函数来强制进行数据类型转换。  
  
 下面两个示例说明了 **CONVERT** 函数的用法。 这些示例假定存在名为 EMPLOYEES 的表，其中 EMPNO 类型为的列 SQL_SMALLINT 和类型 SQL_CHAR 的 EMPNAME 列。  
  
 如果应用程序指定了以下 SQL 语句：  
  
```  
SELECT EMPNO FROM EMPLOYEES WHERE {fn CONVERT(EMPNO,SQL_CHAR)} LIKE '1%'  
```  
  
-   ORACLE 驱动程序将 SQL 语句转换为：  
  
    ```  
    SELECT EMPNO FROM EMPLOYEES WHERE to_char(EMPNO) LIKE '1%'  
    ```  
  
-   SQL Server 的驱动程序将 SQL 语句转换为：  
  
    ```  
    SELECT EMPNO FROM EMPLOYEES WHERE convert(char,EMPNO) LIKE '1%'  
    ```  
  
 如果应用程序指定了以下 SQL 语句：  
  
```  
SELECT {fn ABS(EMPNO)}, {fn CONVERT(EMPNAME,SQL_SMALLINT)}  
   FROM EMPLOYEES WHERE EMPNO <> 0  
```  
  
-   ORACLE 驱动程序将 SQL 语句转换为：  
  
    ```  
    SELECT abs(EMPNO), to_number(EMPNAME) FROM EMPLOYEES WHERE EMPNO <> 0  
    ```  
  
-   SQL Server 的驱动程序将 SQL 语句转换为：  
  
    ```  
    SELECT abs(EMPNO), convert(smallint, EMPNAME) FROM EMPLOYEES  
       WHERE EMPNO <> 0  
    ```  
  
-   Ingres 的驱动程序将 SQL 语句转换为：  
  
    ```  
    SELECT abs(EMPNO), int2(EMPNAME) FROM EMPLOYEES WHERE EMPNO <> 0  
    ```
