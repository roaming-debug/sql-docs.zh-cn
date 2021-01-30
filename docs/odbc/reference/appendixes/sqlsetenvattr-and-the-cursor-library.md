---
description: SQLSetEnvAttr 和游标库
title: SQLSetEnvAttr 和游标库 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetEnvAttr function [ODBC], Cursor Library
ms.assetid: 59cc8eae-09ae-4796-869a-c5806488ae83
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 791933e7c4edd263e282437b06fb79fca65fca65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202635"
---
# <a name="sqlsetenvattr-and-the-cursor-library"></a>SQLSetEnvAttr 和游标库
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 本主题讨论了如何将 **SQLSetEnvAttr** 函数与游标库结合使用。 有关 **SQLSetEnvAttr** 的常规信息，请参阅 [SQLSetEnvAttr 函数](../../../odbc/reference/syntax/sqlsetenvattr-function.md)。  
  
 无论应用程序版本或驱动程序版本是什么，游标库都不受 SQL_ATTR_ODBC_VERSION 环境属性的设置影响。
