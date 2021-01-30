---
description: SQLFreeStmt（游标库）
title: SQLFreeStmt (游标库) |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLFreeStmt function [ODBC], Cursor Library
ms.assetid: 47bfbd4d-9453-4609-958d-1e05794cb223
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3b7ebdcfc5a9997efb4301852b1aae3c1408464d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202814"
---
# <a name="sqlfreestmt-cursor-library"></a>SQLFreeStmt（游标库）
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 本主题讨论如何在游标库中使用 **SQLFreeStmt** 函数。 有关 **SQLFreeStmt** 的常规信息，请参阅 [SQLFreeStmt 函数](../../../odbc/reference/syntax/sqlfreestmt-function.md)。  
  
 如果应用程序在调用 **SQLExtendedFetch**、 **SQLFetch** 或 **SQLFetchScroll** 后使用 SQL_UNBIND 选项调用 **SQLFreeStmt** ，则游标库将返回错误。 应用程序必须使用 SQL_CLOSE 选项调用 **SQLCloseCursor** 或 **SQLFreeStmt** ，然后才能取消对结果集列的绑定。
