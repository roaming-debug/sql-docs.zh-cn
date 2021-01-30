---
description: 处理 SQL 语句
title: 正在处理 SQL 语句 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], statement processing
- SQL statements [ODBC], cursor library
- cursor library [ODBC], statement processing
ms.assetid: 54dad6a3-e86c-477b-ba7c-4e95e0385ec1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed0e9bddba0b23d17e6e880e2b83af2bff359fe9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199877"
---
# <a name="processing-sql-statements"></a>处理 SQL 语句
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 ODBC 游标库将所有 SQL 语句直接传递给驱动程序，如下所示：  
  
-   定位更新和删除语句  
  
-   **选择 FOR UPDATE** 语句  
  
-   批处理 SQL 语句  
  
 若要执行定位的 update 和 delete 语句，并将光标放在一行上以对该行调用 **SQLGetData** ，则游标库将构造一个用于标识该行的搜索语句。  
  
 本部分包含以下主题。  
  
-   [处理定位更新和删除语句](../../../odbc/reference/appendixes/processing-positioned-update-and-delete-statements.md)  
  
-   [处理 SELECT FOR UPDATE 语句](../../../odbc/reference/appendixes/processing-select-for-update-statements.md)  
  
-   [批处理 SQL 语句](../../../odbc/reference/appendixes/processing-batches-of-sql-statements.md)  
  
-   [构造搜索语句](../../../odbc/reference/appendixes/constructing-searched-statements.md)
