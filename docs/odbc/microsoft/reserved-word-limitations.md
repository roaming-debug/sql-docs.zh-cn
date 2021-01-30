---
description: 保留关键字限制
title: 保留字限制 |Microsoft Docs
ms.custom: ''
ms.date: 05/01/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC]
- desktop database drivers [ODBC]
ms.assetid: ed42f083-c9e8-4ee4-9d64-d879bf955c78
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e615ab4c8222ec7ac679f96f360458dfaab5abb9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182571"
---
# <a name="reserved-keyword-limitations"></a>保留关键字限制

避免在 SQL 表或相关对象中使用任何 ODBC 保留关键字作为标识符。 如果在必须将保留关键字用作标识符的情况下出现异常，则必须使用 *反撇号* (") 来包围标识符。 *反撇号* 的另一个名称是 *后退引号*。

保留关键字限制还适用于保留关键字的任何简写形式。

ODBC 保留关键字的列表位于：

- [ODBC 保留关键字](../reference/appendixes/reserved-keywords.md)。

- 有关 *ODBC 程序员参考指南*，请参阅 [附录 C： SQL 语法](../reference/appendixes/appendix-c-sql-grammar.md)。