---
description: SQL_C_TCHAR
title: SQL_C_TCHAR |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- sql_c_tchar [ODBC]
- pseudo-type identifiers [ODBC], SQL_C_TCHAR
- data types [ODBC], pseudo-type identifiers
ms.assetid: 9e27c8bd-ee15-4ce9-b70a-34cf1bf16f4c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a399570a15f04d8ace61664a445c5283d629bf6a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193051"
---
# <a name="sql_c_tchar"></a>SQL_C_TCHAR
SQL_C_TCHAR 类型标识符实际上不标识数据类型;它是用于 Unicode 转换的标头文件中的宏。 根据 UNICODE **#define** 的设置，它将替换为 SQL_C_CHAR 或 SQL_C_WCHAR。 它可用于传输同时编译为 ANSI 和 Unicode 应用程序的字符数据的应用程序。
