---
description: 数值函数
title: 数值函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], numeric functions
- numeric functions [ODBC]
ms.assetid: 4fa548dc-e8b0-4179-92ff-81d6a79d10c3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fb822d707b89dd0f7597ad4acf6783118ec92f54
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072571"
---
# <a name="numeric-functions"></a>数值函数
下表介绍 ODBC 标量函数集中包含的数值函数。 通过使用 SQL_NUMERIC_FUNCTIONS 的 *信息类型* 调用 **SQLGetInfo** ，应用程序可以确定驱动程序支持哪些数值函数。  
  
 除了 ABS、舍入、截断、符号、楼层和天花板（返回与输入参数相同数据类型的值）外，所有数值函数都将返回数据类型 SQL_FLOAT 的值。  
  
 表示为 *numeric_exp* 的参数可以是列的名称、其他标量函数的结果或 *数字 litera* l，其中，基础数据类型可表示为 SQL_NUMERIC、SQL_DECIMAL、SQL_TINYINT、SQL_SMALLINT、SQL_INTEGER、SQL_BIGINT、SQL_FLOAT、SQL_REAL 或 SQL_DOUBLE。  
  
 表示为 *float_exp* 的参数可以是列的名称、其他标量函数的结果或 *数字文本*，其中，基础数据类型可表示为 SQL_FLOAT。  
  
 表示为 *integer_exp* 的参数可以是列的名称、其他标量函数的结果或 *数字文本*，其中，基础数据类型可表示为 SQL_TINYINT、SQL_SMALLINT、SQL_INTEGER 或 SQL_BIGINT。  
  
 ODBC 3.0 中添加了 CURRENT_DATE、CURRENT_TIME 和 CURRENT_TIMESTAMP 标量函数，以使其符合 SQL-92。  
  
|功能|说明|  
|--------------|-----------------|  
|**)** (ODBC 1.0) 的 **ABS (** _numeric_exp_|返回 *numeric_exp* 的绝对值。|  
|**ACOS (** _FLOAT_EXP_ **)**  (ODBC 1.0) |以弧度表示的形式返回 *float_exp* 的反余弦。|  
|**ASIN (** _FLOAT_EXP_ **)**  (ODBC 1.0) |以弧度表示的角度返回 *float_exp* 的反正弦。|  
|**ATAN (** _FLOAT_EXP_ **)**  (ODBC 1.0) |返回 *float_exp* 的反正切值（以弧度表示）。|  
|**ATAN2 (** _float_exp1_， _float_exp2_ **)** (ODBC 2.0) |返回 *x* 和 *y* 坐标的反正切，分别 *float_exp1* 和 *float_exp2* 指定为以弧度表示的角度（以弧度表示）。|  
| (ODBC 1.0 _numeric_exp_ **)** 的 **天花板 (**) |返回大于或等于 *numeric_exp* 的最小整数。 返回值的数据类型与输入参数的数据类型相同。|  
|**COS (** _FLOAT_EXP_ **)**  (ODBC 1.0) |返回 *float_exp* 的余弦值，其中 *float_exp* 是以弧度表示的角。|  
|**COT (** _FLOAT_EXP_ **)**  (ODBC 1.0) |返回 *float_exp* 的余切，其中 *float_exp* 是以弧度表示的角。|  
| (ODBC 2.0 _numeric_exp_ **)** **度 (**) |返回 *numeric_exp* 弧度转换后的度数。|  
|**EXP (** _FLOAT_EXP_ **)**  (ODBC 1.0) |返回 *float_exp* 的指数值。|  
| (ODBC) 1.0) **(** _numeric_exp_ |返回小于或等于 *numeric_exp* 的最大整数。 返回值的数据类型与输入参数的数据类型相同。|  
| (ODBC 1.0 _float_exp_ **)** **日志 (**) |返回 *float_exp* 的自然对数。|  
|**LOG10 (** _FLOAT_EXP_ **)**  (ODBC 2.0) |返回 *float_exp* 以10为底的对数。|  
|**MOD (** _integer_exp1_， _integer_exp2_ **)** (ODBC 1.0) |返回 *integer_exp1* 除以 *integer_exp2* 所得的余数 (模数) 。|  
|**PI ( )**  (ODBC 1.0) |以浮点值的形式返回 pi 的常量值。|  
|**POWER (** _numeric_exp_， _integer_exp_ **)** (ODBC 2.0) |返回 *numeric_exp* 的 *integer_exp* 的幂的值。|  
|**RADIANS (** _NUMERIC_EXP_ **)**  (ODBC 2.0) |返回 *numeric_exp* 度转换后的弧度数。|  
|**RAND (**[*integer_exp*]**)**  (ODBC 1.0) |使用 *integer_exp* 作为可选种子值返回随机浮点值。|  
|**舍入 (** _numeric_exp_， _integer_exp_ **)** (ODBC 2.0) |返回 *numeric_exp* 舍入到小数点右侧 *integer_exp* 位置。 如果 *integer_exp* 为负数，则 *numeric_exp* 舍入到小数点左侧 &#124;*integer_exp*&#124; 位置。|  
|**)** (ODBC) 1.0 _numeric_exp_ **(签名**|返回 *numeric_exp* 的符号的指示器。 如果 *numeric_exp* 小于零，则返回-1。 如果 *numeric_exp* 等于零，则返回0。 如果 *numeric_exp* 大于零，则返回1。|  
|**SIN (** _FLOAT_EXP_ **)**  (ODBC 1.0) |返回 *float_exp* 的正弦值，其中 *float_exp* 是以弧度表示的角。|  
|**SQRT (** _FLOAT_EXP_ **)**  (ODBC 1.0) |返回 *float_exp* 的平方根。|  
| (ODBC 1.0 _float_exp_ **)** **(茶色**) |返回 *float_exp* 的正切值，其中 *float_exp* 是以弧度表示的角。|  
|**截断 (** _numeric_exp_， _integer_exp_ **)** (ODBC 2.0) |返回 *numeric_exp* 截断为小数点右边 *integer_exp* 位置。 如果 *integer_exp* 为负数，则 *numeric_exp* 将被截断为小数点左侧 &#124;*integer_exp*&#124; 位置。|
