---
description: 表值参数描述符字段
title: Table-Valued 参数描述符字段 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), descriptor fields
ms.assetid: 4e009eff-c156-4d63-abcf-082ddd304de2
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6c45999c04377bb88f48e7f97a85b46020673eaf
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755047"
---
# <a name="table-valued-parameter-descriptor-fields"></a>表值参数描述符字段
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  作为对表值参数的支持，ODBC 应用程序参数描述符 (APD) 和实现参数描述符 (IPD) 中新增了特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的字段。  
  
## <a name="remarks"></a>备注  
  
|名称|位置|类型|说明|  
|----------|--------------|----------|-----------------|  
|SQL_CA_SS_TYPE_NAME|IPD|SQLTCHAR*|表值参数的服务器类型名称。<br /><br /> 如果在对 SQLBindParameter 的调用上指定了表值参数类型名称，则它必须始终指定为 Unicode 值，即使是在作为 ANSI 应用程序生成的应用程序中。 用于参数 *StrLen_or_IndPtr* 的值应为 SQL_NTS 或名称的字符串长度乘以 SIZEOF (WCHAR) 。<br /><br /> 当通过 SQLSetDescField 指定表值参数类型名称时，可以使用符合应用程序生成方式的文本指定它。 ODBC 驱动程序管理器将执行任何所需的 Unicode 转换。|  
|SQL_CA_SS_TYPE_CATALOG_NAME（只读）|IPD|SQLTCHAR*|定义该类型的目录。|  
|SQL_CA_SS_TYPE_SCHEMA_NAME|IPD|SQLTCHAR*|定义该类型的架构。|  
  
 应用程序不得为表值参数设置 SQL_CA_SS_TYPE_CATALOG_NAME。 这样做将返回 SQL_ERROR，并记录包含 SQLSTATE = HY091 和消息“描述符字段标识符无效”的诊断记录。  
  
 如果将参数焦点设置为表值参数，则以下语句属性和描述符标头字段将应用于表值参数：  
  
|名称|位置|类型|说明|  
|----------|--------------|----------|-----------------|  
|SQL_ATTR_PARAMSET_SIZE<br /><br /> （这等同于 APD 中的 SQL_DESC_ARRAY_SIZE。）|APD|SQLUINTEGER|用于表值参数的缓冲区数组的数组大小。 这是缓冲区将容纳的最大行数或缓冲区的行数大小；表值参数值本身所具有的行数可能大于或小于缓冲区可以容纳的行数。 默认值为 1。<br /><br /> 注意：如果 SQL_SOPT_SS_PARAM_FOCUS 设置为其默认值0，则 SQL_ATTR_PARAMSET_SIZE 指的是语句，并指定参数集的数目。 如果将 SQL_SOPT_SS_PARAM_FOCUS 设置为表值参数的序号，则它引用该表值参数，并为该表值参数指定每个参数集具有的行数。|  
|SQL_ATTR_PARAM _BIND_TYPE|APD|SQLINTEGER|默认值是 SQL_PARAM_BIND_BY_COLUMN。<br /><br /> 若要选择按行绑定，则该字段将设置为将要绑定到一组表值参数行的结构或缓冲区实例的长度。 此长度必须包括所有绑定列的空间以及结构或缓冲区的任何填充大小。 这将确保当绑定列的地址按指定长度递增时，结果将指向下一行中相同列的开头。 在 ANSI C 中使用 **sizeof** 运算符时，此行为是保证的。|  
|SQL_ATTR_PARAM_BIND_OFFSET_PTR|APD|SQLINTEGER*|默认为 Null 指针。<br /><br /> 如果该字段为非 Null，则驱动程序取消对该指针的引用，并将取消引用的值添加到描述符记录（SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR）中每个延迟的字段，然后使用新指针值访问数据值。|  
  
 这些字段仅对表值参数有效，对于其他数据类型，将忽略它们。  
  
 SQL_CA_SS_TYPE_NAME 对于存储过程调用是可选项。 对于不是过程调用的 SQL 语句，则必须指定它，才能使服务器能够确定表值参数的类型。  
  
 如果类型名称是必需的，并且在不同于该存储过程的架构中定义了表值参数的表类型，则必须在实现参数描述符 (IPD) 中指定 SQL_CA_SS_TYPE_SCHEMA_NAME。 如果不这样，则服务器无法确定表值参数的类型。 当你调用 SQLExecute 或 SQLExecDirect 时，这将导致错误。 错误将包含 SQLSTATE= 07006 和消息“受限制的数据类型属性冲突”。  
  
 表值参数列可以使用按行或按列绑定。 默认为按列绑定。 通过设置 SQL_ATTR_PARAM_BIND_TYPE 和 SQL_ATTR_ PARAM_BIND_OFFSET_PTR，可以指定按行绑定。 这与对列和参数的按行绑定类似。  
  
 SQL_CA_SS_TYPE_CATALOG_NAME 和 SQL_CA_SS_TYPE_SCHEMA_NAME 还可以用于检索与 CLR 用户定义类型参数关联的目录和架构。 对于这些类型，它们是特定于类型的现有目录架构属性的替代项。  
  
## <a name="see-also"></a>另请参阅  
 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  
