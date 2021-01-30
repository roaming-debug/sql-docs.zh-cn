---
description: SQLBindParam 映射
title: SQLBindParam 映射 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLBindparam function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLBindParam
ms.assetid: 375f8f24-36de-4946-916e-c75abc6f070d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 62951dc5ae69e86eb5cbc2fba21407d63952e74b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202942"
---
# <a name="sqlbindparam-mapping"></a>SQLBindParam 映射
不能真正地调用 **SQLBindParam** ，因为它从未出现在 ODBC 中;但是，它仍表示重复的功能，驱动程序管理器需要将其导出，因为 ISO 和打开符合组的应用程序将使用它。 由于 **SQLBindParameter** 包含 **SQLBindParam** 的所有功能，因此当基础 *驱动程序为 ODBC 3.x* 驱动程序) 时， **SQLBindParam** 将在 **SQLBindParameter** (上进行映射。 *ODBC 2.x* 驱动程序不需要实现 **SQLBindParam**。  
  
## <a name="remarks"></a>备注  
 对 **SQLBindParam** 进行以下调用时：  
  
```  
SQLBindParam(   StatementHandle,    ParameterNumber,    ValueType,    ParameterType,    ColumnSize,    DecimalDigits,    ParameterValuePtr,    StrLen_or_IndPtr)  
```  
  
 驱动程序管理器将调用驱动程序中的 **SQLBindParameter** ，如下所示：  
  
```  
SQLBindParameter(   StatementHandle,    ParameterNumber,    SQL_PARAM_INPUT,    ValueType,    ParameterType,    ColumnSize,    DecimalDigits,    ParameterValuePtr,    BufferLength,    StrLen_or_IndPtr)  
```  
  
 如果你的应用程序将在64位操作系统上运行，请参阅 [ODBC 64 位信息](../../../odbc/reference/odbc-64-bit-information.md)。  
  
## <a name="see-also"></a>另请参阅  
 [映射已弃用的函数](../../../odbc/reference/appendixes/mapping-deprecated-functions.md)
