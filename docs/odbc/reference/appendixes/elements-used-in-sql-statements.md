---
description: SQL 语句中使用的元素
title: SQL 语句中使用的元素 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL statements [ODBC], elements supported
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: 85777525-1555-4731-8309-63a464c6b43a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5911de3ce17fc0557b9fad885fa7cdc6ce5024ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194866"
---
# <a name="elements-used-in-sql-statements"></a>SQL 语句中使用的元素
以下元素用于前面列出的 SQL 语句。  
  
## <a name="element"></a>元素  
 *基表标识符* ：： = *用户定义的名称*  
  
 *基表名称* ：： = *基表标识符*  
  
 *布尔型系数* ：： = [NOT] *boolean-primary*  
  
 *布尔-主要* ：： = 比较 *-谓词* &#124; ( *搜索条件* )   
  
 *布尔值* ：： = *布尔因子* [和 *布尔值*]  
  
 *字符-字符串* ：： = "" {*character*} ... ""  (*字符* 是驱动程序/数据源字符集中的任意字符。 若要在字符串文本中包含单个文本引号字符 ( "" ) ，请使用两个文本引号字符 ["' '"]。 )   
  
 *列标识符* ：： = *用户定义的名称*  
  
 *列名* ：： = [*表名称*.]*列标识符*  
  
 *比较-operator* ：： = < &#124; > &#124; \<= &#124; > = &#124; = &#124; <>  
  
 *比较-谓词* ：： = *表达式* 比较运算符表达式  
  
 *数据类型* ：： = *字符串类型* (*字符字符串* 类型是任何数据类型，在 SQLGetTypeInfo 返回的结果集中的 "" DATA_TYPE "" 列是 SQL_CHAR 或 SQL_VARCHAR。 )   
  
 *数字* ：： = 0 &#124; 1 &#124; 2 &#124; 3 &#124; 4 &#124; 5 &#124; 6 &#124; 7 &#124; 8 &#124; 9  
  
 *动态参数* ：： =？  
  
 *expression* ：： = term &#124; expression {+&#124;-} 术语  
  
 *因素* ：： = [ *+*&#124;*-* ]*primary*  
  
 *插入值* ：： =  
  
 *动态参数*  
  
 &#124; *文字*  
  
 &#124; NULL  
  
 &#124; 用户  
  
 *字母*：： =*小写字母 &#124;* 大写字母  
  
 *literal* ：： = *字符-字符串*  
  
 *小写字母* ：： = a &#124; b &#124; c &#124; d &#124; e &#124; f &#124; g &#124; h &#124; i &#124; j &#124; k &#124; l &#124; &#124; &#124; &#124; &#124; &#124; &#124; &#124; &#124; &#124; &#124; &#124; &#124; &#124; z  
  
 *按子句* ：： = 按 *排序规范* 排序 [， *排序规范*] .。。  
  
 *主要* ：： = *列名称*  
  
 &#124; *动态参数*  
  
 &#124; *文字*  
  
  ( *表达式* 的 &#124; )   
  
 *搜索条件* ：： = *布尔词* [或 *搜索条件*]  
  
 *选择* 列表：： = \* &#124; *选择-* 子列表 [， *选择-* 子列表] ... (*选择列表* 不能包含参数。 )   
  
 *选择-子列表* ：： = *表达式*  
  
 *排序规范* ：： = {*无符号整数 &#124; 列名*} [*ASC &#124; DESC*]  
  
 *表标识符* ：： = *用户定义的名称*  
  
 *表名* ：： = *表标识符*  
  
 *表引用* ：： = *表名*  
  
 *表引用列表* ：： = *表引用* [，*表引用*] .。。  
  
 *term* ：： = *因数* &#124; *term* { \*&#124;*/* } *因子*  
  
 *无符号整数* ：： = {*位数*}  
  
 *大写字母*：： = *A &#124; B &#124; C &#124; D &#124; E &#124; F &#124; G &#124; H &#124; I &#124; J &#124; K &#124; L &#124; &#124; &#124;* &#124; &#124; &#124; &#124; &#124; &#124; &#124; &#124; &#124; &#124; &#124; Z  
  
 *用户定义的名称* ：： = *letter*[*数字* &#124; *字母* &#124; *_*] .。。
