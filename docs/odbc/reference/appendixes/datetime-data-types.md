---
description: 日期时间数据类型
title: Datetime 数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- time data type [ODBC]
- datetime data types [ODBC]
- date data type [ODBC]
- data types [ODBC], date
- backward compatibility [ODBC], datetime data types
- timestamp data type [ODBC]
- data types [ODBC], timestamp
- data types [ODBC], backward compatibility
- compatibility [ODBC], datetime data types
- data types [ODBC], time
ms.assetid: 6b9363c9-04bf-4492-a210-7aa15dea4af8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2bee41df18a0a05c7b7812c0a23321feec13ed7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194920"
---
# <a name="datetime-data-types"></a>日期时间数据类型
在 ODBC *3.x 中，* 日期、时间和时间戳 SQL 数据类型的标识符已从 SQL_DATE、SQL_TIME 和 SQL_TIMESTAMP (分别更改为9、10和 11 #define 的标头文件中的 **)** SQL_TYPE_DATE、SQL_TYPE_TIME 和 91 SQL_TYPE_TIMESTAMP (#define) 、92和 93  对应的 C 类型标识符已从 SQL_C_DATE、SQL_C_TIME 和 SQL_C_TIMESTAMP 更改为 SQL_C_TYPE_DATE、SQL_C_TYPE_TIME 和 SQL_C_TYPE_TIMESTAMP，并对 **#define** 的实例进行了相应的更改。  
  
 ODBC 2.x 中 SQL datetime 数据类型返回的列大小和小数位数与 *odbc 2.x* 中为其返回的精度和小数位数 *相同。* 这些值与 "SQL_DESC_PRECISION" 和 "SQL_DESC_SCALE 描述符" 字段中的值不同。  (有关详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。 )   
  
 这些更改会影响 **SQLDescribeCol**、 **SQLDescribeParam** 和 **SQLColAttributes**; **SQLBindCol**、 **SQLBindParameter** 和 **SQLGetData**;和 **SQLColumns**、 **SQLGetTypeInfo**、 **SQLProcedureColumns**、 **SQLStatistics** 和 **SQLSpecialColumns**。  
  
 *ODBC 2.x* 驱动程序根据 SQL_ATTR_ODBC_VERSION 环境属性的设置来处理上一段落中列出的函数调用。 对于 **SQLColumns**、 **SQLGetTypeInfo**、 **SQLProcedureColumns**、 **SQLSpecialColumns** 和 **SQLStatistics**，如果 SQL_ATTR_ODBC_VERSION 设置为 SQL_OV_ODBC3，则这些函数将在 SQL_TYPE_TIMESTAMP 字段中返回 SQL_TYPE_DATE、SQL_TYPE_TIME 和 DATA_TYPE。 **SQLColumns**、 **SQLGetTypeInfo**、 **SQLProcedureColumns** 和 **SQLSpecialColumns** 返回的结果集中 (COLUMN_SIZE 列) 包含近似数值类型的二进制精度。 **SQLColumns**、 **SQLGetTypeInfo** 和 **SQLProcedureColumns**) 返回的结果集中 (NUM_PREC_RADIX 列的值为2。 如果 SQL_ATTR_ODBC_VERSION 设置为 SQL_OV_ODBC2，则函数将在 SQL_TIMESTAMP 字段中返回 SQL_DATE、SQL_TIME 和 DATA_TYPE，则 COLUMN_SIZE 列包含近似数值类型的小数精度，而 NUM_PREC_RADIX 列包含值10。  
  
 当在对 **SQLGetTypeInfo** 的调用中请求所有数据类型时，函数返回的结果集将 *包含 odbc 2.x 中定义的 SQL_TYPE_DATE*、SQL_TYPE_TIME 和 SQL_TYPE_TIMESTAMP，以及 odbc 2.x 中定义的 SQL_DATE、SQL_TIME 和 *SQL_TIMESTAMP。*  
  
 ODBC *3. x* 驱动程序管理器如何执行 date、time 和 timestamp 数据类型的映射，odbc *3。 x* 驱动程序只需要识别91、92和93的 **#defines** ，适用于在 **SQLBindCol** 和 **SQLGetData** 的 *TargetType* 参数或 **SQLBindParameter** 的 *ValueType* 参数中输入的日期、时间和时间戳 C 数据类型，只需识别 **SQLBindParameter** 的 ParameterType 参数 *中* 输入的日期、时间和时间戳 SQL 数据类型的 #defines，**以及** SQLGetTypeInfo 的 **DataType** 参数 *。* 92 91 有关详细信息，请参阅 [Datetime 数据类型更改](../../../odbc/reference/develop-app/datetime-data-type-changes.md)。
