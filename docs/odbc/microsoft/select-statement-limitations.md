---
description: SELECT 语句限制
title: SELECT 语句限制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, SELECT statement limitations
- SELECT statement limitations [ODBC]
ms.assetid: c6b05955-f8fd-4706-a1a7-a8dbd74870c2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5bfbdbf4396b51624173e676f4b0c9bea1f4cf87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208677"
---
# <a name="select-statement-limitations"></a>SELECT 语句限制
不能将聚合函数列与 SELECT 语句中的非聚合列混合使用。  
  
 具有 GROUP BY 子句的 SELECT 语句的选择列表只能包含 GROUP BY 子句中的表达式或 set 函数。  
  
 不支持使用星号 (选择包含 GROUP BY 子句的 SELECT 语句) 中的所有列。 必须指定要选择的列的名称。  
  
 不支持在 SELECT 语句中使用竖线。 如果需要引用包含竖线的数据值，请在 SELECT 语句中使用参数。  
  
 在 SELECT 语句中使用列别名时，该别名前面必须是 "as"。 例如，"选择 col1 作为 a from b"。 如果没有 "as"，语句将返回错误。  
  
 如果在 SELECT 语句中输入了不正确的列名称，则会返回 SQLSTATE 07001 错误，"参数的数目不正确"，而不是 SQLSTATE S0022 错误，"找不到列"。  
  
 使用 Microsoft Excel 驱动程序时，如果插入列中的空字符串，则将空字符串转换为 NULL;使用 WHERE 子句中的空字符串执行的搜索 SELECT 语句对该列不会成功。
