---
description: 聚合函数、CALC 函数和 NEW 关键字
title: 聚合函数、CALC 函数和 NEW 关键字 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], functions
- CALC function [ADO]
- NEW keyword [ADO]
- aggregate functions [ADO]
ms.assetid: 0590b466-2a36-49a2-868e-028ef5e49394
author: rothja
ms.author: jroth
ms.openlocfilehash: 458a0f051e9bbf21c04b0c84125a1c438693f00d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037833"
---
# <a name="aggregate-functions-the-calc-function-and-the-new-keyword"></a>聚合函数、CALC 函数和 NEW 关键字
数据定形支持以下功能。 分配给包含要操作的列的章节的名称是 *章节别名*。  
  
 章节别名可以是完全限定的，其中包含每个章节列名，其中包含 *列名称，* 并以句点分隔。 例如，如果父章节 chap1 包含一个 chap2，其中包含一个 "金额" 列，则该限定名称为 "chap1"。  
  
|聚合函数|说明|  
|-------------------------|-----------------|  
|SUM (*章节别名*。*列名*) |计算指定列中所有值的总和。|  
|AVG (*章节别名*。*列名*) |计算指定列中所有值的平均值。|  
|最大 (*章节别名*。*列名*) |计算指定列中的最大值。|  
|最小 (*章节别名*。*列名*) |计算指定列中的最小值。|  
|计数 (*章节-别名*[。*列名*] ) |计算指定别名中的行数。 如果指定了列，则只会在计数中包含该列非 Null 的行。|  
|STDEV (*章节别名*。*列名*) |计算指定列中的标准偏差。|  
|任何 (*章节别名*。*列名*) |指定列的值。 只有当章节中所有行的列的值相同时，ANY 才具有可预测的值。<br /><br /> **注意** 如果列中的所有行都不包含相同的值，则 SHAPE 命令将随意返回值之一作为 ANY 函数的值。|  
  
|计算表达式|说明|  
|---------------------------|-----------------|  
|计算 (*表达式*) |计算任意表达式，但仅在包含 CALC 函数的 **记录集** 的行上。 允许使用这些 [Visual Basic for Applications (VBA) 函数](./visual-basic-for-applications-functions.md) 的任何表达式。|  
  
|NEW 关键字|说明|  
|-----------------|-----------------|  
|新 *字段类型* [ (*width* &#124; *缩放* &#124; *精度* &#124; *错误* [， *小数位数* &#124; *错误*] ) ]|将指定类型的空列添加到 **记录集**。|  
  
 用 NEW 关键字传递的 *字段类型* 可以是以下任意数据类型。  
  
|OLE DB 数据类型|ADO 数据类型等效 (s) |  
|-----------------------|-----------------------------------|  
|DBTYPE_BSTR|adBSTR|  
|DBTYPE_BOOL|adBoolean|  
|DBTYPE_DECIMAL|adDecimal|  
|DBTYPE_UI1|adUnsignedTinyInt|  
|DBTYPE_I1|adTinyInt|  
|DBTYPE_UI2|adUnsignedSmallInt|  
|DBTYPE_UI4|adUnsignedInt|  
|DBTYPE_I8|adBigInt|  
|DBTYPE_UI8|adUnsignedBigInt|  
|DBTYPE_GUID|adGuid|  
|DBTYPE_BYTES|adBinary, AdVarBinary, adLongVarBinary|  
|DBTYPE_STR|adChar, adVarChar, adLongVarChar|  
|DBTYPE_WSTR|adWChar, adVarWChar, adLongVarWChar|  
|DBTYPE_NUMERIC|adNumeric|  
|DBTYPE_DBDATE|adDBDate|  
|DBTYPE_DBTIME|adDBTime|  
|DBTYPE_DBTIMESTAMP|adDBTimeStamp|  
|DBTYPE_VARNUMERIC|adVarNumeric|  
|DBTYPE_FILETIME|adFileTime|  
|DBTYPE_ERROR|adError|  
  
 如果新字段的类型为 decimal (OLE DB、DBTYPE_DECIMAL 或 ADO adDecimal) 中，则必须指定精度和小数位数。  
  
## <a name="see-also"></a>另请参阅  
 [数据定形示例](./data-shaping-example.md)   
 [正式形状语法](./formal-shape-grammar.md)   
 [常用 Shape 命令](./shape-commands-in-general.md)