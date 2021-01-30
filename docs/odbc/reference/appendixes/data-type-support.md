---
description: 数据类型支持
title: 数据类型支持 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
- data types [ODBC], ODBC drivers
- ODBC drivers [ODBC], data types
ms.assetid: 782b4490-372b-4366-aad7-a486fb8a07c8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5b9249539c87e727b293c73ee12ed0fa734e08fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179538"
---
# <a name="data-type-support"></a>数据类型支持
ODBC 驱动程序必须支持 SQL_CHAR 和 SQL_VARCHAR 中的至少一个。 对其他数据类型的支持取决于驱动程序的或数据源的 SQL-92 一致性级别。 应用程序应调用 **SQLGetTypeInfo** 来确定驱动程序所支持的数据类型。  
  
 有关数据类型的详细信息，请参阅 [附录 D：数据类型](../../../odbc/reference/appendixes/appendix-d-data-types.md)。
