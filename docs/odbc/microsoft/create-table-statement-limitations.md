---
description: CREATE TABLE 语句限制
title: CREATE TABLE 语句限制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- CREATE TABLE statement limitations [ODBC]
- ODBC SQL grammar, CREATE TABLE statement limitations
ms.assetid: c5067855-20c9-456f-8d63-f375b4297f2e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 94b72930fc33c3abbf3920d1290811d0379413c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183193"
---
# <a name="create-table-statement-limitations"></a>CREATE TABLE 语句限制
使用 Microsoft Access、Microsoft Excel 或 Paradoxdriver 时，如果未指定 text 或 binary 列 (或指定为 0) ，则列长度将设置为255。  
  
 使用 dBASE 驱动程序时，如果未指定 text 或 binary 列的长度 (或指定为 0) ，则列长度将设置为254。  
  
 最多支持255列。  
  
 当在 MicrosoftExcel 5.0、7.0 或97数据源上使用 Microsoft Excel 驱动程序时，不能创建与先前删除的工作表同名的工作表。 当使用 Microsoft Excel 驱动程序访问版本5.0、7.0 或97工作表时，DROP TABLE 语句会清除工作表，但不会删除工作表名称。  
  
 使用 Paradox 驱动程序时，一旦对表定义了索引，将无法添加列。 如果 CREATE TABLE 语句的参数列表的第一列创建索引，则参数列表中不能包含第二列。
