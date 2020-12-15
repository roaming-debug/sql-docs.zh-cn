---
description: SQLSetDescField
title: SQLSetDescField |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLSetDescField function
ms.assetid: de4bed15-15be-4825-994c-1046255e725a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c0a6599bd1f0c7c91078f0afbf92079b415436be
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438710"
---
# <a name="sqlsetdescfield"></a>SQLSetDescField
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  SQLSetDescField 可用于为表值参数和表值参数列设置描述符字段。 有关可用字段的信息，请参阅 [表值参数描述符字段](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-descriptor-fields.md) 和 [Table-Valued 参数构成列的描述符字段](../../relational-databases/native-client-odbc-table-valued-parameters/descriptor-fields-for-table-valued-parameter-constituent-columns.md)。  
  
## <a name="remarks"></a>备注  
 表值参数列仅在将描述符标头字段 SQL_SOPT_SS_PARAM_FOCUS 设置为特定记录（其 SQL_DESC_TYPE 设置为 SQL_SS_TABLE）的序数时可用。 有关 SQL_SOPT_SS_PARAM_FOCUS 的详细信息，请参阅 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)。  
  
 如果尝试将 SQL_SOPT_SS_PARAM_FOCUS 设置为不是表值参数的参数的序号，则 SQLSetStmtAttr 将返回 SQL_ERROR，并且将使用 SQLSTATE = HY024 和消息 "属性值无效" 创建诊断记录。 返回 SQL_ERROR 时，不更改 SQL_SOPT_SS_PARAM_FOCUS。  
  
 将 SQL_SOPT_SS_PARAM_FOCUS 设置为 0 可以恢复对参数的描述符记录的访问权限。  
  
 有关表值参数的详细信息，请参阅 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)。  
  
## <a name="sqlsetdescfield-support-for-enhanced-date-and-time-features"></a>SQLSetDescField 对日期和时间增强功能的支持  
 ODBC 中已增强了日期/时间功能。 有关为新的日期/时间类型提供的描述符字段的信息，请参阅 [参数和结果元数据](../../relational-databases/native-client-odbc-date-time/metadata-parameter-and-result.md)。  
  
 有关详细信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqlsetdescfield-support-for-large-clr-udts"></a>SQLSetDescField 对大型 CLR UDT 的支持  
 SQLSetDescField 支持 (Udt) 的大型 CLR 用户定义类型。 有关详细信息，请参阅 [ODBC&#41;&#40;的大型 CLR User-Defined 类型 ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="sqlsetdescfield-support-for-sparse-columns"></a>SQLSetDescField 对稀疏列的支持  
 可以使用 SQLSetDecField 将应用程序参数描述符中的 SQL_SOPT_SS_NAME_SCOPE 设置 (APD) 到值 SQL_SS_NAME_SCOPE_EXTENDED 和 SQL_SS_NAME_SCOPE_SPARSE_COLUMN_SET。  
  
 有关详细信息，请参阅 [稀疏列支持 &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLSetDescField](../../odbc/reference/syntax/sqlsetdescfield-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
