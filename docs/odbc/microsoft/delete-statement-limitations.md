---
description: DELETE 语句限制
title: DELETE 语句限制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DELETE statement limitations [ODBC]
- ODBC SQL grammar, DELETE statement limitations
ms.assetid: 084761fe-e65b-4f38-ba4f-69884b2a7700
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b188d782f82e23d031b820bba5f56a30dad414bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181583"
---
# <a name="delete-statement-limitations"></a>DELETE 语句限制
Microsoft Excel 或文本驱动程序不支持 DELETE 语句。 请注意，文本驱动程序支持 INSERT 语句。  
  
 DBASE 驱动程序不支持对表进行封装以删除 "已删除" 值。  
  
 对于 Paradox 驱动程序，要从表中删除行，表必须具有唯一索引 (Paradox 主键) 。
