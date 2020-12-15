---
description: SQL Server Native Client 日期和时间改进
title: 日期和时间改进 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.reviewer: ''
ms.prod: sql
ms.technology: native-client
ms.topic: reference
ms.assetid: 9b1d0d9d-1f6e-4399-8f61-e23f9a486a7a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c7b5a802c430ce958e448edeac3066cd0012021f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463388"
---
# <a name="sql-server-native-client-date-and-time-improvements"></a>SQL Server Native Client 日期和时间改进
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题介绍 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 对于 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 中已添加的日期和时间数据类型的支持。  
  
 有关日期/时间改进的详细信息，请参阅 [日期和时间改进 &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md) 以及 [&#40;ODBC&#41;的日期和时间改进 ](../../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
 有关演示此功能的示例应用程序的信息，请参阅 [SQL Server 数据编程示例](https://msftdpprodsamples.codeplex.com/)。  
  
## <a name="usage"></a>使用情况  
 以下各节介绍使用新的日期和时间类型的各种方法。  
  
### <a name="use-date-as-a-distinct-data-type"></a>将日期用作非重复数据类型  
 从 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 开始，借助于对日期/时间类型的增强支持，可以更高效地使用 SQL_TYPE_DATE ODBC 类型（对于 ODBC 2.0 应用程序为 SQL_DATE）和 DBTYPE_DBDATE OLE DB 类型。  
  
### <a name="use-time-as-a-distinct-data-type"></a>将时间用作非重复数据类型  
 OLE DB 已具有一种只包含时间的数据类型 DBTYPE_DBTIME，它的精度为 1 秒。 在 ODBC 中，等效类型为 SQL_TYPE_TIME（对于 ODBC 2.0 应用程序为 SQL_TIME）。  
  
 新的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时间数据类型具有秒的小数形式，其准确度可达 100 纳秒。 这需要 Native Client 中的新类型 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ： DBTYPE_DBTIME2 (OLE DB) 和 SQL_SS_TIME2 (ODBC) 。 已编写的所用时间不带秒的小数部分的现有应用程序可以使用 time(0) 列。 现有的 OLE DB DBTYPE_TIME 和 ODBC SQL_TYPE_TIME 类型及其对应的结构应正常工作，除非应用程序依赖于元数据中返回的类型。  
  
### <a name="use-time-as-a-distinct-data-type-with-extended-fractional-seconds-precision"></a>将具有扩展的秒的小数部分精度的时间用作非重复数据类型  
 某些应用程序（如过程控制和生产应用程序）要求能够处理精度高达 100 纳秒的时间数据。 可满足这一用途的新类型为 DBTYPE_DBTIME2 (OLE DB) 和 SQL_SS_TIME2 (ODBC)。  
  
### <a name="use-datetime-with-extended-fractional-seconds-precision"></a>使用具有扩展的秒的小数部分精度的日期时间  
 OLE DB 已定义了一个精度高达 1 纳秒的类型。 但是，此类型已由现有的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 应用程序使用，并且此类应用程序预计只需 1/300 秒精度。 新的 datetime2(3) 类型与现有的日期时间类型不直接兼容  。 如果这一点将影响应用程序行为而导致风险，则应用程序必须使用新的 DBCOLUMN 标志以确定实际的服务器类型。  
  
 ODBC 还定义了一个精度高达 1 纳秒的类型。 但是，此类型已由现有的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 应用程序使用，并且此类应用程序预计只需 3 毫秒的精度。 新 **datetime2 (3)** 类型与现有 **datetime** 类型不直接兼容。 **datetime2 (3)** 的精度为1毫秒， **datetime** 的精度为1/300 秒。 在 ODBC 中，应用程序可以确定哪一个服务器类型用于描述符字段 SQL_DESC_TYPE_NAME。 因此，现有类型 SQL_TYPE_TIMESTAMP（对于 ODBC 2.0 应用程序为 SQL_TIMESTAMP）可用于这两个类型。  
  
### <a name="use-datetime-with-extended-fractional-seconds-precision-and-timezone"></a>使用具有扩展的秒的小数部分精度和时区的日期时间  
 一些应用程序要求带有时区信息的日期时间值。 新的 DBTYPE_DBTIMESTAMPOFFSET (OLE DB) 和 SQL_SS_TIMESTAMPOFFSET (ODBC) 类型支持这一要求。  
  
### <a name="use-datetimedatetimedatetimeoffset-data-with-client-side-conversions-consistent-with-existing-conversions"></a>将 Date/Time/Datetime/Datetimeoffset 数据用于与现有转换一致的客户端转换  
 ODBC 标准描述了如何在现有的日期、时间和时间戳类型之间进行转换。 这些将以一致的方式进行扩展，以包括在 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 中引入的所有日期和时间类型之间的转换。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client 功能](../../../relational-databases/native-client/features/sql-server-native-client-features.md)  
  
  
