---
description: 使用数据类型标识符
title: 使用数据类型标识符 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], identifiers
- identifiers [ODBC], data types
ms.assetid: 467e0c0c-a818-4737-8a24-3d8e15c7e162
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cd38f5ddb62a28bc3ec2658dca621769e13ab481
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202440"
---
# <a name="using-data-type-identifiers"></a>使用数据类型标识符
应用程序通过两种方式使用数据类型标识符：向驱动程序描述其缓冲区，并从驱动程序检索有关结果集的元数据，以便它们能够确定要用于存储数据的 C 缓冲区类型。 应用程序调用以下函数来执行这些任务：  
  
-   **SQLBindParameter**、 **SQLBindCol** 和 **SQLGetData** -描述应用程序缓冲区的 C 数据类型。  
  
-   **SQLBindParameter** -描述动态参数的 SQL 数据类型。  
  
-   **SQLColAttribute** 和 **SQLDescribeCol** -检索结果集列的 SQL 数据类型。  
  
-   **SQLDescribeParameter** -检索参数的 SQL 数据类型。  
  
-   **SQLColumns**、 **SQLProcedureColumns** 和 **SQLSpecialColumns** -检索各种架构信息的 SQL 数据类型  
  
-   **SQLGetTypeInfo** -检索支持的数据类型的列表  
  
 数据类型标识符存储在描述符的 SQL_DESC_CONCISE_TYPE 字段中。 描述符函数 **SQLSetDescField** 和 **SQLSetDescRec** 可用于适当的类型，以执行上列表中列出的任务。 有关详细信息，请参阅 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)。
