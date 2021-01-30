---
description: 外部联接转义序列
title: 外部联接转义序列 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- outer join escape sequence [ODBC]
- escape sequences [ODBC], outer join
- ODBC escape sequences [ODBC], outer join
ms.assetid: 2cfd1525-6677-4d36-9b9e-730496853750
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e25cde1a4da326d06efe23bc2f7c642e61589ff3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160855"
---
# <a name="outer-join-escape-sequence"></a>外部联接转义序列
ODBC 对外部联接使用转义序列。 此转义序列的语法如下所示：  
  
```  
{oj outer-join}  
```  
  
## <a name="remarks"></a>备注  
 在 BNF 表示法中，语法如下：  
  
 *ODBC-外部联接-escape* ：： =  
  
 *Odbc* -oj *外部联接 odbc-esc-终止符*  
  
 *外部联接* ：： = *表名* [*相关名称*] {LEFT &#124; RIGHT &#124; FULL}  
  
 外部联接 {*表名* [*相关名称*] &#124; *外接联接*}  
  
 *寻找*  
  
 *状态*  
  
 *相关名称* ：： = *用户定义的名称*  
  
 *ODBC-esc-发起方* ：： = {  
  
 *ODBC-esc-终止符* ：： =}  
  
 若要确定支持此语句的哪些部分，应用程序需要使用 SQL_OJ_CAPABILITIES 信息类型调用 **SQLGetInfo** 。 对于外部联接， *搜索条件* 必须只包含指定 *表名称* 之间的联接条件。
