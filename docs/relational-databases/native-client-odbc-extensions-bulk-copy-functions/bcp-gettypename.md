---
description: bcp_gettypename
title: bcp_gettypename |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_gettypename
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_gettypename function
ms.assetid: 65f036d1-f60e-4b8a-97b3-76fccf0dfed4
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b8ac4f8233301f8c6b96f78a83fc5517a75eb888
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749547"
---
# <a name="bcp_gettypename"></a>bcp_gettypename
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回指定 BCP 类型标记的 SQL 类型名称。  
  
## <a name="syntax"></a>语法  
  
```  
  
RETCODE bcp_gettypename (  
        INT token,  
        DBBOOL fIsMaxType);  
```  
  
## <a name="arguments"></a>参数  
 *token*  
 指示 BCP 类型标记的值。  
  
 *定义域*  
 指示请求的标记是否为 max 类型。  
  
## <a name="returns"></a>返回  
 一个字符串，其中包含与 BCP 类型对应的 SQL 类型名称。 如果指定了无效的 BCP 类型，则返回空字符串。  
  
## <a name="remarks"></a>备注  
 BCP 类型标记在 sqlncli.h 头文件和 sqlncli11.lib 库中定义。  
  
 下表指定了可能的 BCP 类型、这些类型是否是 max 类型以及预期的输出。  
  
|BCP 类型名称|MaxType|输出|  
|-------------------|-------------|------------|  
|**SQLDECIMAL**|任一个|**decimal**|  
|**SQLNUMERIC**|任一个|**numeric**|  
|**SQLINT1**|任一个|**tinyint**|  
|**SQLINT2**|任一个|**smallint**|  
|**SQLINT4**|任一个|**int**|  
|**SQLMONEY**|任一个|**money**|  
|**SQLFLT8**|任一个|**float**|  
|**SQLDATETIME**|任一个|**datetime**|  
|**SQLBITN**|任一个|**bit-null**|  
|**SQLBIT**|任一个|**bit**|  
|**SQLBIGCHAR**|否|**char**|  
|**SQLCHARACTER**|否|**char**|  
|**SQLBIGVARCHAR**|否|**varchar**|  
|**SQLVARCHAR**|否|**varchar**|  
|**SQLTEXT**|任一个|**text**|  
|**SQLBIGBINARY**|否|**binary**|  
|**SQLBINARY**|否|**二进制**|  
|**SQLBIGVARBINARY**|否|**Varbinary**|  
|**SQLVARBINARY**|否|**Varbinary**|  
|**SQLIMAGE**|任一个|**图像**|  
|**SQLINTN**|任一个|**int-null**|  
|**SQLDATETIMN**|任一个|**datetime-null**|  
|**SQLMONEYN**|任一个|**money-null**|  
|**SQLFLTN**|任一个|**float-null**|  
|**SQLAOPSUM**|任一个|**Sum**|  
|**SQLAOPAVG**|任一个|**Avg**|  
|**SQLAOPCNT**|任一个|**计数**|  
|**SQLAOPMIN**|任一个|**Min**|  
|**SQLAOPMAX**|任一个|**Max**|  
|**SQLDATETIM4**|任一个|**smalldatetime**|  
|**SQLMONEY4**|任一个|**Smallmoney**|  
|**SQLFLT4**|任一个|**实际上**|  
|**SQLUNIQUEID**|任一个|**uniqueidentifier**|  
|**SQLNCHAR**|否|**Nchar**|  
|**SQLNVARCHAR**|否|**Nvarchar**|  
|**SQLNTEXT**|任一个|**Ntext**|  
|**SQLVARIANT**|任一个|**sql_variant**|  
|**SQLINT8**|任一个|**Bigint**|  
|**SQLCHARACTER**|是|**varchar(max)**|  
|**SQLBIGCHAR**|是|**varchar(max)**|  
|**SQLBIGVARCHAR**|是|**varchar(max)**|  
|**SQLVARCHAR**|是|**varchar(max)**|  
|**SQLBINARY**|是|**varbinary(max)**|  
|**SQLBIGBINARY**|是|**varbinary(max)**|  
|**SQLBIGVARBINARY**|是|**varbinary(max)**|  
|**SQLVARBINARY**|是|**varbinary(max)**|  
|**SQLNCHAR**|是|**nvarchar(max)**|  
|**SQLNVARCHAR**|是|**nvarchar(max)**|  
|**SQLXML**|是|**Xml**|  
|**SQLUDT**|任一个|**Udt**|  
  
## <a name="bcp_gettypename-support-for-enhanced-date-and-time-features"></a>bcp_gettypename 对日期和时间增强功能的支持  
 有关日期/时间类型的令牌参数值，请参阅 sqlncli.msi [OLE DB 和 ODBC&#41;的增强日期和时间 &#40;类型的大容量复制更改 ](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md)中表的 "Type in" 列。 返回值位于“文件存储类型”列的对应行中。  
  
 有关详细信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
