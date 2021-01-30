---
description: Visual FoxPro 字段数据类型
title: Visual FoxPro 字段数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- field data types [ODBC]
- Visual FoxPro ODBC driver [ODBC], data types
ms.assetid: 50b733dc-679a-4b10-bc5d-98bb474dead2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2032ee25039364186f33486e4662b84c36c06947
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207477"
---
# <a name="visual-foxpro-field-data-types"></a>Visual FoxPro 字段数据类型
下表列出了 ALTER TABLE 和 CREATE TABLE 中的 *FieldType* 参数的值，并指示是否需要 *nFieldWidth* 和 *nPrecision* 参数。  
  
|*FieldType*|*NFieldWidth*|*nPrecision*|说明|  
|-----------------|-------------------|------------------|-----------------|  
|B|-|d|Double|  
|C|N|-|宽度为 *n* 的字符字段|  
|D|-|-|Date|  
|F|N|d|宽度为 *n* 且带有 *d* 个小数点的浮动数值字段|  
|G|-|-|常规|  
|I|-|-|Integer|  
|L|-|-|逻辑|  
|M|-|-|备忘录|  
|N|N|d|宽度为 *n* 且带有 *d* 小数位的数值字段|  
|T|-|-|DateTime|  
|Y|-|-|货币|
