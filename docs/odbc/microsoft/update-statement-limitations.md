---
description: UPDATE 语句限制
title: UPDATE 语句限制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- UPDATE statement limitations [ODBC]
- ODBC SQL grammar, UPDATE statement limitations
ms.assetid: 14700aac-e135-4dc0-9138-4b01224461d5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 721af98991f4d63c459ba5df44bc425c245d2dc7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190627"
---
# <a name="update-statement-limitations"></a>UPDATE 语句限制
为了使 Paradox 驱动程序更新表，表必须具有唯一索引 (Paradox 主键) 。 使用 Paradox 驱动程序时，如果不实现 Borland 数据库引擎，将无法更新 Paradox 表。  
  
 不受文本驱动程序支持。  
  
 当使用 Microsoft Excel 驱动程序时，可能会更新值，但无法基于 Microsoft Excel 电子表格从表中删除行。 因此，Microsoft Excel 驱动程序不会将 UPDATE 语句视为正式支持。 仅将 INSERT 语句视为受支持。
