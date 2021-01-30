---
description: 过程调用转义序列
title: 过程调用转义序列 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- escape sequences [ODBC], procedure call
- procedure call escape sequence [ODBC]
- ODBC escape sequences [ODBC], procedure call
ms.assetid: 269fbab0-e5f2-4a98-86c0-2d7b647acaae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a3177c134009ed6f8979637980e96f39ab9badd2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205330"
---
# <a name="procedure-call-escape-sequence"></a>过程调用转义序列
ODBC 对过程调用使用转义序列。 此转义序列的语法如下所示：  
  
 **{**[？ =]**call** *procedure-name*[**(**[*参数*] [，[*parameter*]] .。。**)**]**}**  
  
 在 BNF 表示法中，语法如下：  
  
 *ODBC-过程-escape* ：： =  
  
 &#124; *odbc-发起* 方 [？ =] 调用 *过程 ODBC-终止符*  
  
 *procedure* ：： = *procedure* &#124; *过程-name* (*过程-参数列表*)   
  
 *过程标识符* ：： = *用户定义的名称*  
  
 *过程-name* ：： = *过程标识符*  
  
 &#124; *所有者名称*。*过程标识符*  
  
 &#124;*目录名称目录分隔符**过程标识符*  
  
 &#124; *目录名称目录分隔符* [*所有者名称*]。*过程标识符*  
  
 仅当数据源不支持所有者时，才 (第三个语法有效。 )   
  
 *所有者-名称* ：： = *用户定义的名称*  
  
 *目录名称* ：： = *用户定义的名称*  
  
 *目录分隔符* ：： = {*实现定义*}  
  
  (使用 SQL_CATALOG_NAME_SEPARATOR 信息选项通过 **SQLGetInfo** 返回目录分隔符。 )   
  
 *过程参数-list* ：： = *procedure-参数*  
  
 &#124;*过程* 参数（*过程参数列表*）  
  
 *过程参数* ：： = *动态参数* &#124; *文本* &#124; *为空字符串*  
  
 *空字符串* ：： =  
  
 *ODBC-esc-发起方* ：： = {  
  
 *ODBC-esc-终止符* ：： =}  
  
  (如果过程参数是空字符串，则该过程将使用该参数的默认值。 )   
  
 若要确定数据源是否支持过程并且驱动程序支持 ODBC 过程调用语法，应用程序可以使用 SQL_PROCEDURES 信息类型调用 **SQLGetInfo** 。
