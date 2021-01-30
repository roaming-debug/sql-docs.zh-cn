---
description: ALTER TABLE 语句限制
title: ALTER TABLE 语句限制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, ALTER TABLE statement limitations
- ALTER TABLE statement limitations [ODBC]
ms.assetid: f3e88f85-edf4-47cd-a822-292b106ddb34
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d36e5dfb2457a9d3453dbe3bc877c5d267731ea1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205743"
---
# <a name="alter-table-statement-limitations"></a>ALTER TABLE 语句限制
使用 dBASE 或 Paradox 驱动程序时，一旦创建了一个索引并添加了新记录，ALTER TABLE 语句就不能更改该表的结构，除非删除该索引并删除该表的内容。  
  
 Microsoft Excel 或文本驱动程序不支持 ALTER TABLE 语句。  
  
> [!NOTE]  
>  如果在不实现 Borland 数据库引擎的情况下使用 Paradox 驱动程序，则不支持 ALTER TABLE 语句;只允许使用 read 和 append 语句。
