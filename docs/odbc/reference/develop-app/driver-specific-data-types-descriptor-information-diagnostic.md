---
description: 特定于驱动程序的数据类型、描述符类型、信息类型、诊断类型和属性
title: Driver-Specific 类型-数据、描述符、信息、诊断 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver-specific diagnostic values [ODBC]
- diagnostic information [ODBC], driver-specific values
- ODBC drivers [ODBC], driver-specific diagnostic values
ms.assetid: ad4c76d3-5191-4262-b47c-5dd1d19d1154
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03cd6b0ed9a424a161f88f4bd525941895d3d201
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062562"
---
# <a name="driver-specific-data-types-descriptor-types-information-types-diagnostic-types-and-attributes"></a>特定于驱动程序的数据类型、描述符类型、信息类型、诊断类型和属性
驱动程序可以为以下各项分配特定于驱动程序的值：  
  
-   **SQL 数据类型指示器** 在 **SQLBindParameter** 中的 *ParameterType* 和 **SQLGetTypeInfo** 中的 *数据类型* 中使用这些方法， **SQLColAttribute**、 **SQLColumns**、 **SQLDescribeCol**、 **SQLGetTypeInfo**、 **SQLDescribeParam**、 **SQLProcedureColumns** 和 **SQLSpecialColumns** 返回这些数据。  
  
-   **描述符字段** 这些在 **SQLColAttribute**、 **SQLGetDescField** 和 **SQLSetDescField** 的 *FieldIdentifier* 中使用。  
  
-   **诊断字段** 这些在 **SQLGetDiagField** 和 **SQLGetDiagRec** 的 *DiagIdentifier* 中使用。  
  
-   **信息类型** 在 **SQLGetInfo** 的 *InfoType* 中使用这些。  
  
-   **连接和语句属性** 它们用于 **SQLGetConnectAttr**、 **SQLGetStmtAttr**、 **SQLSetConnectAttr** 和 **SQLSetStmtAttr** 的 *特性* 中。  
  
 对于上述每个项，有两组值：保留供 ODBC 使用的值，以及保留供驱动程序使用的值。 在实现驱动程序特定的值之前，驱动程序编写器必须为打开的组中的每个特定于驱动程序的类型、字段或属性请求一个值。 对于新的驱动程序开发，请使用下表中所述的范围。 如果使用的值不在下面所述的范围内，ODBC 3.8 驱动程序管理器将不会生成错误。 但是，如果收到的未知值不在范围内，驱动程序管理器的更高版本可能会生成错误。  
  
 当这些值中的任何一个传递到 ODBC 函数时，驱动程序必须检查该值是否有效。 驱动程序返回 SQLSTATE HYC00 (可选功能，该功能未实现适用于其他驱动程序的特定于驱动程序的值) 。  
  
 从 ODBC 3.8 开始，驱动程序编写器可以分配保留范围内的特定于驱动程序的属性。  
  
> [!NOTE]  
>  对于向后兼容性，ODBC 3.8 驱动程序管理器不会对这些范围进行验证和强制。 不过，驱动程序管理器的未来版本可能会强制实施它们。  
  
|属性类型|ODBC 数据类型|驱动程序特定的范围基数|特定于驱动程序的范围限制|驱动程序特定的值范围基准的 ODBC 常量|  
|--------------------|--------------------|---------------------------------|----------------------------------|---------------------------------------------------------|  
|SQL 数据类型指示器|SQLSMALLINT|0x4000|0x7FFF|SQL_DRIVER_SQL_TYPE_BASE|  
|描述符字段|SQLSMALLINT|0x4000|0x7FFF|SQL_DRIVER_DESCRIPTOR_BASE|  
|诊断字段|SQLSMALLINT|0x4000|0x7FFF|SQL_DRIVER_DIAGNOSTIC_BASE|  
|信息类型|SQLUSMALLINT|0x4000|0x7FFF|SQL_DRIVER_INFO_TYPE_BASE|  
|连接属性|SQLINTEGER|0x00004000|0x00007FFF|SQL_DRIVER_CONNECT_ATTR_BASE|  
|语句特性|SQLINTEGER|0x00004000|0x00007FFF|SQL_DRIVER_STATEMENT_ATTR_BASE|  
  
> [!NOTE]  
>  驱动程序文档中必须描述特定于驱动程序的数据类型、描述符字段、诊断字段、信息类型、语句特性和连接属性。 当这些值中的任何一个传递到 ODBC 函数时，驱动程序必须检查该值是否有效。 驱动程序返回 SQLSTATE HYC00 (可选功能，该功能未实现适用于其他驱动程序的特定于驱动程序的值) 。  
  
 定义基值是为了促进驱动程序开发。 例如，可以采用以下格式定义驱动程序特定的诊断属性：  
  
```  
SQL_DRIVER_DIAGNOSTIC_BASE+0, SQL_DRIVER_DIAGNOSTIC_BASE +1  
```
