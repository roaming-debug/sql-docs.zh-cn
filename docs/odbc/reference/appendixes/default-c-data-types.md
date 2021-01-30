---
description: 默认 C 数据类型
title: 默认 C 数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], pseudo-type identifiers
- pseudo-type identifiers [ODBC], about pseudo-type identifiers
- pseudo-type identifiers [ODBC]
ms.assetid: 229140ae-af8f-4ec8-9ccf-1e92360e0bac
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fd7f50e58fd84cec5938aacc598d12e6d733fcdc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194897"
---
# <a name="default-c-data-types"></a>默认 C 数据类型
如果应用程序在 **SQLBindCol**、 **SQLGetData** 或 **SQLBindParameter** 中指定 SQL_C_DEFAULT，则驱动程序会假定输出或输入缓冲区的 C 数据类型对应于缓冲区绑定到的列或参数的 SQL 数据类型。  
  
> [!IMPORTANT]  
>  互操作应用程序不应使用 SQL_C_DEFAULT。 相反，它们应始终指定它们所使用的缓冲区的 C 类型。 这是因为，由于以下原因，驱动程序无法始终正确确定默认 C 类型：  
  
-   如果 DBMS 升级列或参数的 SQL 数据类型，则驱动程序无法确定列或参数的原始 SQL 数据类型。 因此，它不能确定相应的默认 C 数据类型。  
  
-   如果驱动程序无法确定特定的列或参数是否经过签名，则在 DBMS 处理此类或参数时通常是如此，驱动程序将无法确定是否应对相应的默认 C 数据类型进行签名或无符号。  
  
     由于 SQL_C_DEFAULT 仅作为编程便利提供，因此当应用程序指定实际 C 数据类型时，不会丢失任何功能。  
  
 在本附录后面的将 [数据从 SQL 转换为 c 数据](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)类型时，将包含显示每个 sql 数据类型的默认 C 数据类型的表。
