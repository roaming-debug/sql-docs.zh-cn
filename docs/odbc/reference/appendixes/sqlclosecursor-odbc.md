---
description: SQLCloseCursor_ODBC
title: SQLCloseCursor_ODBC |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLCloseCursor function [ODBC], ODBC
ms.assetid: 5e47e3f7-e1b8-451f-bf75-daa19b7c7271
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 06943851bc71e5a54792fa24ddb6e8e7908c13c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202906"
---
# <a name="sqlclosecursor_odbc"></a>SQLCloseCursor_ODBC
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 本主题讨论如何在游标库中使用 **SQLCloseCursor** 函数。 有关 **SQLCloseCursor** 的常规信息，请参阅 [SQLCloseCursor 函数](../../../odbc/reference/syntax/sqlclosecursor-function.md)。  
  
 游标库不支持在没有打开的游标的情况下调用 **SQLCloseCursor** 。 尝试此将返回 SQLSTATE 24000 (无效的游标状态) 。 当游标库支持未打开任何游标时，使用 SQL_CLOSE *选项* 调用 **SQLFreeStmt** 。
