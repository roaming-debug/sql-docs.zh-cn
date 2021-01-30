---
description: LIKE 转义序列
title: LIKE 转义序列 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC escape sequences [ODBC], LIKE
- LIKE escape sequence [ODBC]
- escape sequences [ODBC], LIKE
ms.assetid: 798d75ea-be9d-4bef-b297-318bc327f1ca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d32b0470cb2adf0960e23dac17d8cb4942f296c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184396"
---
# <a name="like-escape-sequence"></a>LIKE 转义序列
ODBC 对 LIKE 子句使用转义序列。 此转义序列的语法如下所示：  
  
```  
{'escape-character'}  
```  
  
## <a name="remarks"></a>备注  
 在 BNF 表示法中，语法如下：  
  
 *类似 ODBC-escape* ：： =  
  
 *Odbc-esc-发起程序* 转义符 \*转义* 符 ' *odbc-esc-终止符*  
  
 *转义* 符：： = *字符*  
  
 *ODBC-esc-发起方* ：： = {  
  
 *ODBC-esc-终止符* ：： =}  
  
 若要确定驱动程序是否支持类似的转义序列，应用程序可以使用 SQL_LIKE_ESCAPE_CLAUSE 信息类型调用 **SQLGetInfo** 。
