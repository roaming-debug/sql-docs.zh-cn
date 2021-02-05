---
description: getFunctionColumns 方法 (SQLServerDatabaseMetaData)
title: getFunctionColumns 方法 (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e2b0e0f7-717c-48e6-bcd2-a325d938a833
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03a5bbe04c9fdf511659a2abef7e6e4135e87370
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175756"
---
# <a name="getfunctioncolumns-method-sqlserverdatabasemetadata"></a>getFunctionColumns 方法 (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索关于指定目录的系统函数或用户函数参数和返回类型的说明。  
  
## <a name="syntax"></a>语法  
  
```  
  
public ResultSet getFunctionColumns(java.lang.String catalog,  
                       java.lang.String schemaPattern,  
                       java.lang.String functionNamePattern  
                       java.lang.String columnNamePattern)  
```  
  
#### <a name="parameters"></a>参数  
 *catalog*  
  
 一个包含目录名称的字符串  。 如果该名称为空字符串 ""，则结果将包括无目录的函数。 如果此字符串为“null”，目录名称则不可用于搜索  。  
  
 schemaPattern  
  
 一个包含架构名称模式的字符串  。 如果该名称为空字符串 ""，则结果将包括无架构的函数。 如果此字符串为“null”，架构名称则不可用于搜索  。  
  
 functionNamePattern  
  
 一个包含函数名称的字符串  。  
  
 columnNamePattern  
  
 一个包含参数名称的字符串  。  
  
## <a name="return-value"></a>返回值  
 一个 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getFunctionColumns 方法是由 java.sql.DatabaseMetaData 接口中的 getFunctionColumns 方法指定的。  
  
 此方法只返回与指定目录内的指定架构、函数名称和参数名称相匹配的函数和参数。  
  
 结果集中的各行均包括针对参数说明、列说明或返回类型的以下列：  
  
|名称|类型|说明|  
|----------|----------|-----------------|  
|FUNCTION_CAT|**字符串**|函数所在的数据库的名称。|  
|FUNCTION_SCHEM|**字符串**|函数的架构。|  
|FUNCTION_NAME|**字符串**|函数的名称。|  
|COLUMN_NAME|**字符串**|参数或列的名称。|  
|COLUMN_TYPE|**short**|**列的类型。可以为下列值之一：**<br /><br /> functionColumnUnknown (0):未知类型。<br /><br /> functionColumnIn (1):输入参数。<br /><br /> functionColumnInOut (2):输入/输出参数。<br /><br /> functionColumnOut (3):输出参数。<br /><br /> functionReturn (4):函数返回值。<br /><br /> functionColumnResult (5):参数或列是结果集中的列。|  
|DATA_TYPE|**smallint**|来自 Java.sql.Types 的 SQL 数据类型值。|  
|TYPE_NAME|**字符串**|数据类型的名称。|  
|PRECISION|**int**|有效数字总个数。|  
|LENGTH|**int**|数据的长度（字节）。|  
|SCALE|**short**|小数点右边的数字位数。|  
|RADIX|**short**|数值类型的基数。|  
|NULLABLE|**short**|指示参数或返回值是否可包括 null 值  。<br /><br /> **可以为下列值之一：**<br /><br /> functionNoNulls (0):不允许 NULL 值。<br /><br /> functionNullable (1):允许 NULL 值。<br /><br /> functionNullableUnknown (2):未知。|  
|REMARKS|**字符串**|有关列或参数的注释。|  
|COLUMN_DEF|**字符串**|列的默认值。<br /><br /> **注意：** 此信息可在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中使用，且特定于 JDBC 驱动程序。|  
|SQL_DATA_TYPE|**smallint**|此列与 DATA_TYPE 列相同，datetime 和 ISO interval 数据类型除外    。<br /><br /> **注意：** 此信息可在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中使用，且特定于 JDBC 驱动程序。|  
|SQL_DATETIME_SUB|**smallint**|如果 SQL_DATA_TYPE 的值为 SQL_DATETIME 或 SQL_INTERVAL，则为 datetime ISO interval 子代码      。 对于 datetime  和 ISO interval  以外的数据类型，该列为 NULL。<br /><br /> **注意：** 此信息可在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中使用，且特定于 JDBC 驱动程序。|  
|CHAR_OCTET_LENGTH|**int**|基于二进制和字符的最大长度的参数或列。 对于其他数据类型，该值为 NULL。|  
|ORDINAL_POSITION|**int**|对于输入和输出参数，它表示从 1 开始的位置。<br /><br /> 对于结果集列，它指从 1 开始的结果集的列的位置。<br /><br /> 对于返回值，该值为 0。|  
|IS_NULLABLE|**字符串**|确定参数或列的可为 Null 性。<br /><br /> 可以为下列值之一：<br /><br /> 是  ：参数或列可包括 NULL 值。<br /><br /> 否  ：参数或列不可包括 NULL 值。<br /><br /> 空字符串 ("")：未知。|  
|SS_TYPE_CATALOG_NAME|**字符串**|包含用户定义类型 (UDT) 的目录名称。|  
|SS_TYPE_SCHEMA_NAME|**字符串**|包含用户定义类型 (UDT) 的架构名称。|  
|SS_UDT_CATALOG_NAME|**字符串**|采用完全限定名称的用户定义类型 (UDT)。|  
|SS_UDT_SCHEMA_NAME|**字符串**|在其中定义 XML 架构集合名称的目录的名称。 如果找不到目录名称，此变量则会包含一个空字符串。|  
|SS_UDT_ASSEMBLY_TYPE_NAME|**字符串**|在其中定义 XML 架构集合名称的架构的名称。 如果找不到架构名称，则为空字符串。|  
|SS_XML_SCHEMACOLLECTION_CATALOG_NAME|**字符串**|XML 架构集合的名称。 如果找不到名称，则为空字符串。|  
|SS_XML_SCHEMACOLLECTION_SCHEMA_NAME|**字符串**|包含用户定义类型 (UDT) 的目录名称。|  
|SS_XML_SCHEMACOLLECTION_NAME|**字符串**|包含用户定义类型 (UDT) 的架构名称。|  
|SS_DATA_TYPE|**tinyint**|扩展存储过程使用的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。<br /><br /> **注意：** 有关 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 返回的数据类型的详细信息，请参阅 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书中的“数据类型 (Transact-SQL)”。|  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
