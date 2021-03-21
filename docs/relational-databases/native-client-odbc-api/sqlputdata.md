---
description: SQLPutData
title: SQLPutData |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLPutData function
ms.assetid: d39aaa5b-7fbc-4315-a7f2-5a7787e04f25
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 21142e393b0367453a83187086e009217ebd5e0c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104736177"
---
# <a name="sqlputdata"></a>SQLPutData
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  当使用 SQLPutData 向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本 6.0 SQL Server (400) 4.21 发送超过65535个字节的数据 (时，以下限制适用于) SQL_LONGVARCHAR **文本** (**) SQL_WLONGVARCHAR (**) SQL_LONGVARBINARY ()   
  
-   引用的参数可以是 INSERT 语句中的 *insert_value* 。  
  
-   引用的参数可以是 UPDATE 语句的 SET 子句中的 *表达式* 。  
  
 当使用版本6.5 或更低版本时，取消将向运行的服务器提供块数据的 SQLPutData 调用序列将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导致列值的部分更新。 调用 SQLCancel 时引用的 **text**、 **ntext** 或 **image** 列被设置为中间占位符值。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序不支持连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 6.5 版和更低版本。  
  
## <a name="diagnostics"></a>诊断  
 对于 SQLPutData，有一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 特定的 SQLSTATE：  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|22026|字符串数据，长度不匹配|如果应用程序已指定要发送的数据的长度（以字节为单位），例如，使用 SQL_LEN_DATA_AT_EXEC (*n*) 其中 *n* 大于0，则应用程序通过 SQLPutData 指定的字节总数必须与指定的长度匹配。|  
  
## <a name="sqlputdata-and-table-valued-parameters"></a>SQLPutData 和表值参数  
 使用带有表值参数的可变行绑定时，应用程序将使用 SQLPutData。 *StrLen_Or_Ind* 参数指示它已准备好供驱动程序为表值参数数据的下一行或多行收集数据，或者没有更多的可用行：  
  
-   大于 0 的值指示可以使用下一组行值。  
  
-   0 值指示已没有更多的行要发送。  
  
-   任何小于 0 的值则会出错，导致记录一个诊断记录，该记录包含 SQLState HY090 和消息“字符串或缓冲区长度无效”。  
  
 *DataPtr* 参数将被忽略，但必须设置为非 NULL 值。 有关详细信息，请参阅 [Table-Valued 参数和列值的数据传输绑定](../../relational-databases/native-client-odbc-table-valued-parameters/binding-and-data-transfer-of-table-valued-parameters-and-column-values.md)中的变量 TVP 行绑定部分。  
  
 如果 *StrLen_Or_Ind* 包含除 SQL_DEFAULT_PARAM 以外的任何值或介于0和 SQL_PARAMSET_SIZE (，即 SQLBindParameter) 的 *ColumnSize* 参数，则为错误。 此错误导致 SQLPutData 返回 SQL_ERROR：SQLSTATE=HY090，“字符串或缓冲区长度无效”。  
  
 有关表值参数的详细信息，请参阅 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)。  
  
## <a name="sqlputdata-support-for-enhanced-date-and-time-features"></a>SQLPutData 对增强的日期和时间功能的支持  
 日期/时间类型的参数值按 [从 C 转换到 SQL](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-c-to-sql.md)中所述的方式进行转换。  
  
 有关详细信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqlputdata-support-for-large-clr-udts"></a>SQLPutData 对大型 CLR UDT 的支持  
 **SQLPutData** 支持 (udt) 的大型 CLR 用户定义类型。 有关详细信息，请参阅 [ODBC&#41;&#40;的大型 CLR User-Defined 类型 ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLPutData 函数](../../odbc/reference/syntax/sqlputdata-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
