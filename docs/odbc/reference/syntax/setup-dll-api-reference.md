---
description: Setup DLL API 函数
title: 安装程序 DLL API 参考 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC drivers [ODBC], driver setup DLL
- driver setup DLL [ODBC]
ms.assetid: f9d03f17-1c0d-4e7c-9c04-8c316e07ef25
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d80d6315227013d2de149d6847eb0ed5b8fa223a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174607"
---
# <a name="setup-dll-api-reference"></a>Setup DLL API 函数
本部分介绍了驱动程序安装程序 DLL API 的语法，其中包含两个函数 (**ConfigDriver** 和 **ConfigDSN**) 。 **ConfigDriver** 和 **ConfigDSN** 可以在驱动程序 dll 中或单独的安装程序 dll 中。  
  
 此外，本部分还介绍了转换器安装程序 DLL API 的语法，其中包含单个函数 (**ConfigTranslator**) 。 **ConfigTranslator** 可以在转换器 dll 中或单独的安装 dll 中。  
  
 每个函数都带有引入该函数的 ODBC 版本进行标记。  
  
 本部分包含以下主题。  
  
-   [ConfigDriver 函数](../../../odbc/reference/syntax/configdriver-function.md)  
  
-   [ConfigDSN 函数](../../../odbc/reference/syntax/configdsn-function.md)  
  
-   [ConfigTranslator 函数](../../../odbc/reference/syntax/configtranslator-function.md)
