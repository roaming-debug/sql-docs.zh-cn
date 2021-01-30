---
description: SQLDriverToDataSource 函数
title: SQLDriverToDataSource 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDriverToDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDriverToDataSource
helpviewer_keywords:
- SQLDriverToDataSource function [ODBC]
ms.assetid: 0de28eb5-8aa9-43e4-a87f-7dbcafe800dc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2ea0697dcedcf27992e771382b47a4b6713b1535
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211910"
---
# <a name="sqldrivertodatasource-function"></a>SQLDriverToDataSource 函数
**SQLDriverToDataSource** 支持 ODBC 驱动程序的翻译。 启用了 ODBC 的应用程序不调用此函数;应用程序通过 **SQLSetConnectAttr** 请求转换。 与 **SQLSetConnectAttr** 中指定的 *ConnectionHandle* 关联的驱动程序将调用指定的 DLL，以执行从驱动程序流向数据源的所有数据的转换。 可以在 ODBC 初始化文件中指定默认的转换 DLL。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLDriverToDataSource(  
     UDWORD     fOption,  
     SWORD      fSqlType,  
     PTR        rgbValueIn,  
     SDWORD     cbValueIn,  
     PTR        rgbValueOut,  
     SDWORD     cbValueOutMax,  
     SDWORD *   pcbValueOut,  
     UCHAR *    szErrorMsg,  
     SWORD      cbErrorMsgMax,  
     SWORD *    pcbErrorMsg);  
```  
  
## <a name="arguments"></a>参数  
 *fOption*  
 送选项值。  
  
 *fSqlType*  
 送ODBC SQL 数据类型。 此参数通知驱动程序如何将 *rgbValueIn* 转换为数据源可接受的格式。 有关有效的 SQL 数据类型的列表，请参阅 [Sql 数据类型](../../../odbc/reference/appendixes/sql-data-types.md)。  
  
 *rgbValueIn*  
 送要转换的值。  
  
 *cbValueIn*  
 送 *RgbValueIn* 的长度。  
  
 *rgbValueOut*  
 输出转换的结果。  
  
> [!NOTE]  
>  转换 DLL 不为 null-终止此值。  
  
 *cbValueOutMax*  
 送 *RgbValueOut* 的长度。  
  
 *pcbValueOut*  
 输出 (排除可在 *rgbValueOut* 中返回的 null 终止字节) 的总字节数。  
  
 对于字符或二进制数据，如果该值大于或等于 *cbValueOutMax*，则 *rgbValueOut* 中的数据将被截断为 *cbValueOutMax* 字节。  
  
 对于所有其他数据类型，将忽略 *cbValueOutMax* 的值，并且转换 DLL 假定 *rgbValueOut* 的大小为 SQL 数据类型的默认 C 数据类型的大小，该数据类型为 *fSqlType* 指定的 SQL 数据类型。  
  
 *PcbValueOut* 参数可以为 null 指针。  
  
 *szErrorMsg*  
 输出用于错误消息的存储指针。 这是一个空字符串，除非转换失败。  
  
 *cbErrorMsgMax*  
 送 *SzErrorMsg* 的长度。  
  
 *pcbErrorMsg*  
 输出一个指针，指向在 *szErrorMsg* 中可返回的不包括 null 终止字节)  (的总字节数。 如果此值大于或等于 *cbErrorMsg*，则 *szErrorMsg* 中的数据将被截断为 *cbErrorMsgMax* 减去 null 终止字符。 *PcbErrorMsg* 参数可以为 null 指针。  
  
## <a name="returns"></a>返回  
 如果转换成功，则为 TRUE; 如果转换失败，则为 FALSE。  
  
## <a name="comments"></a>注释  
 驱动程序调用 **SQLDriverToDataSource** 来转换从驱动程序传递到数据源)  (SQL 语句、参数等的所有数据。 转换 DLL 可能不会转换某些数据，这取决于数据的类型和翻译 DLL 的用途。 例如，将字符数据从一个代码页转换到另一个代码页的 DLL 将忽略所有数字和二进制数据。  
  
 将 *fOption* 的值设置为通过使用 SQL_ATTR_TRANSLATE_OPTION 特性调用 **SQLSetConnectAttr** 来指定的 *vParam* 的值。 它是一个32位值，它具有给定翻译 DLL 的特定含义。 例如，它可以指定特定的字符集转换。  
  
 如果为 *rgbValueIn* 和 *rgbValueOut* 指定了相同的缓冲区，则会就地执行缓冲区中的数据转换。  
  
 尽管 *cbValueIn*、 *cbValueOutMax* 和 *pcbValueOut* 的类型为 SDWORD，但 **SQLDriverToDataSource** 不一定支持大指针。  
  
 如果 **SQLDriverToDataSource** 返回 FALSE，则在转换过程中可能发生数据截断。 如果 *pcbValueOut* (输出缓冲区中可返回的字节数) 大于 *cbValueOutMax* (输出缓冲区的长度) ，则发生截断。 驱动程序必须确定截断是否可接受。 如果未发生截断，则 **SQLDriverToDataSource** 返回 FALSE，原因是另一个错误。 在任一情况下，都将在 *szErrorMsg* 中返回特定的错误消息。  
  
 有关转换数据的详细信息，请参阅 [翻译 dll](../../../odbc/reference/develop-app/translation-dlls.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|转换从数据源返回的数据|[SQLDataSourceToDriver](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|  
|返回连接属性的设置|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|设置连接属性|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
