---
description: SQLSetConnectAttr（游标库）
title: SQLSetConnectAttr (游标库) |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectAttr function [ODBC], Cursor Library
ms.assetid: 6f70bbd0-a057-49ef-8b05-4c80b58fc6e6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 02f702991a4e03816f3e4a30a233d098198bfefe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202672"
---
# <a name="sqlsetconnectattr-cursor-library"></a>SQLSetConnectAttr（游标库）
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 本主题讨论如何在游标库中使用 **SQLSetConnectAttr** 函数。 有关 **SQLSetConnectAttr** 的常规信息，请参阅 [SQLSetConnectAttr 函数](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)。  
  
 应用程序调用带有 SQL_ATTR_ODBC_CURSORS 特性的 **SQLSetConnectAttr** ，以指定是否始终使用游标库，如果驱动程序不支持可滚动的游标，则使用此函数，或者从未使用过。 游标库假设当驱动程序返回 **SQLGetInfo** 中 SQL_STATIC_CURSOR_ATTRIBUTES1 信息类型 SQL_CA1_RELATIVE 时，该驱动程序支持可滚动的游标。  
  
 应用程序必须调用 **SQLSetConnectAttr** 来指定游标库的使用情况，然后再 **调用具有 SQL_HANDLE_DBC** 的 *HandleType* ，以分配连接并连接到数据源。 如果在连接仍处于活动状态时，应用程序使用 SQL_ATTR_ODBC_CURSORS 特性调用 **SQLSetConnectAttr** ，则游标库将返回错误。  
  
 若要为与连接相关联的所有语句设置游标库支持的语句属性，应用程序必须在连接到数据源之后以及在打开游标之前为该语句特性调用 **SQLSetConnectAttr** 。 如果应用程序使用语句特性调用 **SQLSetConnectAttr** ，并在与该连接关联的语句上打开了游标，则在关闭并重新打开该游标之前，语句属性将不会应用于该语句。
