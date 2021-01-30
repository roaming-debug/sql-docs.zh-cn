---
description: 附录 E：标量函数
title: 附录 E：标量函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL-92 functions [ODBC]
- scalar functions [ODBC]
- functions [ODBC], scalar
ms.assetid: 59c7cd5e-32d6-43ab-bac3-7010322d105a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 94ad0914477de0e2dfd25667755757bbc36a8b29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212537"
---
# <a name="appendix-e-scalar-functions"></a>附录 E：标量函数
ODBC 指定以下类型的标量函数，其中包含有关此附录的相应部分中提供的每个函数类型的详细信息。 函数说明包括相关的语法。  
  
 本附录包含以下主题。  
  
-   [字符串函数](../../../odbc/reference/appendixes/string-functions.md)  
  
-   [数值函数](../../../odbc/reference/appendixes/numeric-functions.md)  
  
-   [时间、日期和时间间隔函数](../../../odbc/reference/appendixes/time-date-and-interval-functions.md)  
  
-   [系统函数](../../../odbc/reference/appendixes/system-functions.md)  
  
-   [显式数据类型转换函数](../../../odbc/reference/appendixes/explicit-data-type-conversion-function.md)  
  
-   [SQL-92 CAST 函数](../../../odbc/reference/appendixes/sql-92-cast-function.md)  
  
 ODBC 不会对标量函数的返回值强制使用数据类型，因为函数通常是特定于数据源的。 应用程序应尽可能使用 CONVERT 标量函数来强制进行数据类型转换。  
  
## <a name="odbc-and-sql-92-scalar-functions"></a>ODBC 和 SQL-92 标量函数  
 本附录中的表包括在 ODBC 3.0 中添加的与 SQL-92 一致的函数。 对于为特定类型的标量函数（在 ODBC 中定义）添加的那些函数，将在每个部分中指明。  
  
 ODBC 和 SQL-92 以不同的方式对其标量函数进行分类。 ODBC 按参数类型对标量函数进行分类;SQL-92 按返回值对它们进行分类。 例如，提取函数被 ODBC 归类为 timedate.cpl 函数，因为提取字段参数是 datetime 关键字，提取-source 参数是 datetime 或 interval 表达式。 另一方面，SQL-92 将提取为数值标量函数，因为返回值为数值。  
  
 应用程序可以通过调用 **SQLGetInfo** 来确定驱动程序支持的标量函数。 同时为 ODBC 和 SQL-92 分类的标量函数提供信息类型。 由于这些分类是不同的，因此，对某些标量函数的支持可能会在不对应于 ODBC 和 SQL-92 的信息类型中指示。 例如，在 ODBC 中对提取的支持由 SQL_TIMEDATE_FUNCTIONS 信息类型指示;另一方面，对在 SQL-92 中提取的支持由 SQL_SQL92_NUMERIC_VALUE_FUNCTIONS 信息类型指示。
