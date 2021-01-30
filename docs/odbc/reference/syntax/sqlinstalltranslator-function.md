---
description: SQLInstallTranslator 函数
title: SQLInstallTranslator 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallTranslator
helpviewer_keywords:
- SQLInstallTranslator function [ODBC]
ms.assetid: 453b21ff-3c2b-4069-8ff7-5c727f062d89
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 40f4c4a5df497c64d081a8d03d1765a4fe5e060a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189661"
---
# <a name="sqlinstalltranslator-function"></a>SQLInstallTranslator 函数
**度**  
 引入的版本： ODBC 2.5，不推荐使用  
  
 **摘要**  
 在 ODBC 3.0 中， **SQLInstallTranslator** 已替换为 [SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)。 对 **SQLInstallTranslator** 的调用将映射到 **SQLInstallTranslatorEx**。 有关详细信息，请参阅 **SQLInstallTranslatorEx**。  
  
 如果应用程序 *在 ODBC 2.X* 驱动程序管理器中调用该应用程序，并将 *lpszInfFile* 参数设置为 NULL 以外的值，则 **SQLInstallTranslator** 将返回 FALSE。 Odbc 2.x 中使用的 Odbc .inf 文件在 ODBC *2.x 中不再受支持，**甚至是为了* 向后兼容。
