---
description: SQLColAttributes 映射
title: SQLColAttributes 映射 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLColAttributes
- SQLColAttribute function [ODBC], mapping
ms.assetid: 30e25719-176b-4c48-97d4-920766b22412
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7c8ff9d861663e1bb9e52ad5e083fa4fed809139
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202897"
---
# <a name="sqlcolattributes-mapping"></a>SQLColAttributes 映射
当应用程序 *通过 ODBC 1.x* 驱动程序调用 **SQLColAttributes** 时，对 **SQLColAttributes** 的调用将映射到 **SQLColAttribute** ，如下所示：  
  
> [!NOTE]
>  ODBC 2.x 中 *FieldIdentifier* 值中使用的 *前缀已从* odbc 2.x 中使用的值 *更改。* 新前缀为 "SQL_DESC";旧前缀为 "SQL_COLUMN"。  
  
1.  如果 *应用程序是 ODBC 2.x* 应用程序， *fDescType* 是 SQL_COLUMN_TYPE 的，并且返回的类型为简洁日期时间类型，则驱动程序管理器会将日期、时间和时间戳代码的返回值进行映射。  
  
2.  如果 *fDescType* 是 SQL_COLUMN_NAME、SQL_COLUMN_NULLABLE 或 SQL_COLUMN_COUNT，则驱动程序管理器会根据需要将 **SQLColAttribute** 中的 FieldIdentifier 参数映射到 SQL_DESC_NAME、SQL_DESC_NULLABLE 或 SQL_DESC_COUNT 的参数 *。* *FDescType* 的所有其他值将传递给驱动程序。  
  
 ODBC 2.x *驱动程序* 必须支持为 **SQLColAttribute** 列出的所有 odbc *FieldIdentifiers* 。  
  
 *ODBC 2.x* 驱动程序必须支持 SQL_COLUMN_PRECISION 和 SQL_DESC_PRECISION、SQL_COLUMN_SCALE 和 SQL_DESC_SCALE 以及 SQL_COLUMN_LENGTH 和 SQL_DESC_LENGTH。 这些值是不同的，因为 *在 odbc 3.x* 中，精度、小数位数和长度的定义不同。  有关详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。
