---
description: 参数标记
title: 参数标记 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
- parameter markers [ODBC]
ms.assetid: 07213d04-cd31-45fd-a8c8-2e16e09eeaf4
author: David-Engel
ms.author: v-daenge
ms.reviewer: ''
ms.openlocfilehash: 7ab9b0cd8682ecc722f97b563285bdd31665c1fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205336"
---
# <a name="parameter-markers"></a>参数标记
根据 SQL-92 规范，应用程序不能将参数标记置于以下位置。 有关更全面的列表，请参阅 SQL-92 规范。  
  
-   在 **选择** 列表中  
  
-   作为 *比较谓词* 中的两个 *表达式*  
  
-   作为二元运算符的两个操作数  
  
-   作为运算 **间** 第一个和第二个操作数  
  
-   作为运算 **间** 第一个和第三个操作数  
  
-   作为的表达式和 **中** 的第一个值  
  
-   作为一元 + 或-运算的操作数  
  
-   作为 *函数引用* 的参数  
  
 有关参数标记的详细信息，请参阅 SQL-92 规范。 有关参数的详细信息，请参阅 [语句参数](../../../odbc/reference/develop-app/statement-parameters.md)。
