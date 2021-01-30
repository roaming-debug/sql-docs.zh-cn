---
description: 处理 SELECT FOR UPDATE 语句
title: 处理 SELECT FOR UPDATE 语句 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], statement processing
- SQL statements [ODBC], select for update statements
- select for update [ODBC]
- SQL statements [ODBC], cursor library
- cursor library [ODBC], select for update statements
- ODBC cursor library [ODBC], select for update statements
- cursor library [ODBC], statement processing
ms.assetid: 8d2e79a4-5daf-458e-a536-d8b6e588753e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 116a87e12d1f765e3e33aa5b446743acfd8d429b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189738"
---
# <a name="processing-select-for-update-statements"></a>处理 SELECT FOR UPDATE 语句
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 为了实现最大的互操作性，应用程序应通过执行 **SELECT FOR update** 语句来生成将通过定位的 update 语句更新的结果集。 尽管游标库不需要此功能，但大多数支持定位 update 语句的数据源也需要它。  
  
 游标库忽略 **SELECT FOR update** 语句的 **for update** 子句中的列;它在将语句传递给驱动程序之前删除此子句。 在游标库中，"SQL_ATTR_CONCURRENCY 语句" 属性和上一部分中所述的限制都控制是否可以更新结果集中的列。
