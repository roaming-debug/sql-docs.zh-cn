---
description: SQL_ARD_TYPE
title: SQL_ARD_TYPE |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], pseudo-type identifiers
- pseudo-type identifiers [ODBC], SQL_ARD_TYPE
- SQL_ARD_TYPE [ODBC]
ms.assetid: 8d87ca10-f955-4284-8689-e9f4cc31e7ae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43d8ce804fc66fd8d9a305868cf1a8b22cf8ff7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187073"
---
# <a name="sql_ard_type"></a>SQL_ARD_TYPE
SQL_ARD_TYPE 类型标识符用于指示缓冲区中的数据将为 ARD 的 SQL_DESC_CONCISE_TYPE 字段中指定的类型。 SQL_ARD_TYPE 输入到对 **SQLGetData** 的调用而不是特定数据类型的 *TargetType* 参数中，并使应用程序可以通过更改描述符字段来更改缓冲区的数据类型。 此值将 *\* TargetValuePtr* 缓冲区的数据类型与描述符字段联系在一起。 在对 **SQLBindCol** 或 **SQLBindParameter** 的调用中未输入 (SQL_ARD_TYPE，因为绑定缓冲区的类型已与 SQL_DESC_TYPE 和 SQL_DESC_CONCISE_TYPE 字段相关联，并且可以通过更改其中一个字段随时更改。 )   
  
 SQL_ARD_TYPE 类型标识符可用于为间隔数据类型的前导精度和秒精度指定非默认值，并可用于指定 SQL_C_NUMERIC 数据类型的精度和小数位数值。 有关详细信息，请参阅本附录后面的 [重写间隔数据类型的默认前导和秒精度](../../../odbc/reference/appendixes/overriding-default-leading-and-seconds-precision-for-interval-data-types.md) 和 [覆盖数值数据类型的默认精度和小数位数](../../../odbc/reference/appendixes/overriding-default-precision-and-scale-for-numeric-data-types.md)。
