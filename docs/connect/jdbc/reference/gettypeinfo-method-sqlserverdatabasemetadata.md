---
description: getTypeInfo 方法 (SQLServerDatabaseMetaData)
title: getTypeInfo 方法 (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getTypeInfo
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 23208f01-c1bf-4235-b29c-9051d3df59a3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43145d029b4095ab05cd47526deface99dc81eb0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99155651"
---
# <a name="gettypeinfo-method-sqlserverdatabasemetadata"></a>getTypeInfo 方法 (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索当前数据库支持的所有标准 SQL 类型的说明。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.sql.ResultSet getTypeInfo()  
```  
  
## <a name="return-value"></a>返回值  
 一个 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getTypeInfo 方法是由 java.sql.DatabaseMetaData 接口中的 getTypeInfo 方法指定的。  
  
 由 getTypeInfo 方法返回的结果集将包含以下信息：  
  
|名称|类型|说明|  
|----------|----------|-----------------|  
|TYPE_NAME|**字符串**|数据类型的名称。|  
|DATA_TYPE|**short**|来自 java.sql.Types 的 SQL 数据类型。|  
|PRECISION|**int**|有效数字总个数。|  
|LITERAL_PREFIX|**字符串**|常量之前使用的一个或多个字符。|  
|LITERAL_SUFFIX|**字符串**|用于终止常量的一个或多个字符。|  
|CREATE_PARAMS|**字符串**|此数据类型的创建参数说明。|  
|NULLABLE|**short**|指示列能否包含 Null 值。 可以为下列值之一：<br /><br /> typeNoNulls (0)<br /><br /> typeNullable (1)<br /><br /> typeNullableUnknown (2)|  
|CASE_SENSITIVE|**boolean**|指示数据类型是否区分大小写。 如果类型区分大小写，则为“true”；否则为“false”。|  
|可搜索|**short**|指示是否可在 SQL WHERE 子句中使用此列。 可以为下列值之一：<br /><br /> typePredNone (0)<br /><br /> typePredChar (1)<br /><br /> typePredBasic (2)<br /><br /> typeSeachable (3)|  
|UNSIGNED_ATTRIBUTE|**boolean**|指示数据类型的符号。 如果类型未签名，则为“true”；否则为“false”。|  
|FIXED_PREC_SCALE|**boolean**|指示数据类型可以为 money 值。 如果数据类型为 money 类型，则为“true”；否则为“false”。|  
|AUTO_INCREMENT|**boolean**|指示数据类型可以自动递增。 如果类型可以自动递增，则为“true”；否则为“false”。|  
|LOCAL_TYPE_NAME|**字符串**|数据类型的本地化名称。|  
|MINIMUM_SCALE|**short**|小数点右边的最大位数。|  
|MAXIMUM_SCALE|**short**|小数点右边的最小位数。|  
|SQL_DATA_TYPE|**int**|JDBC 驱动程序不支持此类型。|  
|SQL_DATETIME_SUB|**int**|JDBC 驱动程序不支持此类型。|  
|NUM_PREC_RADIX|**int**|计算某列最大容纳数时所采用的位数或数字个数。|  
|INTERVAL_PRECISION|**smallint**|间隔起始精度的值。|  
|USERTYPE|**smallint**|来自 systypes 表的 usertype 值。 有关详细信息，请参阅 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书。|  
  
> [!NOTE]  
>  有关 getTypeInfo 方法返回的数据的详细信息，请参阅 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 联机丛书中的“sp_datatype_info (Transact-SQL)”。  
  
## <a name="example"></a>示例  
 以下示例演示了如何使用 getTypeInfo 方法返回有关 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]（或更高版本）数据库中使用的数据类型的信息。  
  
```  
public static void executeGetTypeInfo(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getTypeInfo();  
      ResultSetMetaData rsmd = rs.getMetaData();  
  
      // Display the result set data.  
      int cols = rsmd.getColumnCount();  
      while(rs.next()) {  
         for (int i = 1; i <= cols; i++) {  
            System.out.println(rs.getString(i));  
         }  
      }  
      rs.close();  
   }   
  
   catch (Exception e) {  
      e.printStackTrace();  
   }  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 方法](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
