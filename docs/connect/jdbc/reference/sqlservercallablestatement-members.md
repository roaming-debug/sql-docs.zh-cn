---
description: SQLServerCallableStatement 成员
title: SQLServerCallableStatement 成员 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apitype: Assembly
ms.assetid: 5ebdc186-e50f-4d14-bbf4-95af5051e4a4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f134ee20c60dc45b216122f76482f6a331fa9a35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172864"
---
# <a name="sqlservercallablestatement-members"></a>SQLServerCallableStatement 成员
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  下表列出由 [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) 类公开的成员。  
  
## <a name="constructors"></a>构造函数  
 无。  
  
## <a name="fields"></a>字段  
  
|类继承自：|方法|  
|---------------------------|-------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS、CLOSE_CURRENT_RESULT、EXECUTE_FAILED、KEEP_CURRENT_RESULT、NO_GENERATED_KEYS、RETURN_GENERATED_KEYS、SUCCESS_NO_INFO|  
  
## <a name="inherited-fields"></a>继承的字段  
 无。  
  
## <a name="methods"></a>方法  
  
|名称|说明|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）向此 CallableStatement 对象的一批命令添加一组参数。|  
|[cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）取消当前正由此 CallableStatement 对象运行的 SQL 语句。|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）为此 CallableStatement 对象清空当前 SQL 命令列表。|  
|[clearParameters](../../../connect/jdbc/reference/clearparameters-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）立即清除当前参数值。|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）清除对此 CallableStatement 对象报告的所有警告。|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）立即释放此 CallableStatement 对象的数据库和 JDBC 资源，而不是等待它们被自动释放。|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）运行此 CallableStatement 对象中的 SQL 语句，该语句可以是任何类型的 SQL 语句。|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）向数据库提交要运行的命令批。 如果所有命令都成功运行，则返回一个更新计数数组。|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）运行此 CallableStatement 对象中的 SQL 查询并返回由该查询生成的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象。|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）运行此 CallableStatement 对象中的 SQL 语句，该语句必须是 SQL INSERT、UPDATE、MERGE 或 DELETE 语句；或是不返回任何内容的 SQL 语句，如 DDL 语句。|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索 [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) 对象，该对象生成了此 SQLServerStatement 对象。|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)|将指定列的值作为 [DateTimeOffset 类](../../../connect/jdbc/reference/datetimeoffset-class.md)对象进行检索。|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索从数据库表中提取行的方向，该方向是由此 CallableStatement 对象生成的结果集的默认值。|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索结果集行数，该行数是由此 CallableStatement 对象生成的结果集对象的默认提取大小。|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索因运行此 CallableStatement 对象而创建的任何自动生成的键。|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索可为由此 CallableStatement 对象生成的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中的字符和二进制列值返回的最大字节数。|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索由此 CallableStatement 对象生成的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象可包含的最大行数。|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）检索 [SQLServerResultSetMetaData Class](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md) 对象，该对象包含有关运行此 CallableStatement 对象时将返回的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的列信息。|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）移动到此 CallableStatement 对象的下一个结果。|  
|[getParameterMetaData](../../../connect/jdbc/reference/getparametermetadata-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）检索此 CallableStatement 对象的参数的个数、类型及属性。|  
|[getArray](../../../connect/jdbc/reference/getarray-method-sqlservercallablestatement.md)|检索指定参数作为 Array 对象的值。|  
|[getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlservercallablestatement.md)|检索指定参数作为 ASCII  字符流的值。|  
|[getBigDecimal](../../../connect/jdbc/reference/getbigdecimal-method-sqlservercallablestatement.md)|检索指定参数的值作为 java.math.BigDecimal。|  
|[getBinaryStream](../../../connect/jdbc/reference/getbinarystream-method-sqlservercallablestatement.md)|检索指定参数的值作为未解释字节二进制流。|  
|[getBlob](../../../connect/jdbc/reference/getblob-method-sqlservercallablestatement.md)|检索指定 JDBC Blob 参数作为 Java 编程语言中的 Blob 对象的值。|  
|[getboolean](../../../connect/jdbc/reference/getboolean-method-sqlservercallablestatement.md)|检索指定参数作为 Boolean 值的值。|  
|[getByte](../../../connect/jdbc/reference/getbyte-method-sqlservercallablestatement.md)|检索指定参数作为 byte 值的值。|  
|[getBytes](../../../connect/jdbc/reference/getbytes-method-sqlservercallablestatement.md)|检索指定参数的值作为字节数组。|  
|[getCharacterStream](../../../connect/jdbc/reference/getcharacterstream-method-sqlservercallablestatement.md)|检索指定参数的值作为 java.io.Reader 对象。|  
|[getClob](../../../connect/jdbc/reference/getclob-method-sqlservercallablestatement.md)|检索指定 JDBC Blob 参数作为 Java 编程语言中的 Clob 对象的值。|  
|[getDate](../../../connect/jdbc/reference/getdate-method-sqlservercallablestatement.md)|检索指定参数的值作为 Java 编程语言中的 java.sql.Date 对象。|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)|将指定列的值作为 [DateTimeOffset 类](../../../connect/jdbc/reference/datetimeoffset-class.md)对象进行检索。|  
|[getDouble](../../../connect/jdbc/reference/getdouble-method-sqlservercallablestatement.md)|检索指定参数作为 Java 编程语言中的 double 的值。|  
|[getFloat](../../../connect/jdbc/reference/getfloat-method-sqlservercallablestatement.md)|检索指定参数作为 Java 编程语言中的 float 的值。|  
|[getInt](../../../connect/jdbc/reference/getint-method-sqlservercallablestatement.md)|检索指定参数作为 Java 编程语言中的 int 的值。|  
|[getLong](../../../connect/jdbc/reference/getlong-method-sqlservercallablestatement.md)|检索指定参数作为 Java 编程语言中的 long 值。|  
|[getNCharacterStream](../../../connect/jdbc/reference/getncharacterstream-method-sqlservercallablestatement.md)|检索指定参数作为 Reader 对象的值。|  
|[getNClob](../../../connect/jdbc/reference/getnclob-method-sqlservercallablestatement.md)|检索指定 JDBC NCLOB 参数作为 Java 编程语言中的 NClob 对象的值。|  
|[getNString](../../../connect/jdbc/reference/getnstring-method-sqlservercallablestatement.md)|将指定 NCHAR、NVARCHAR 或 LONGNVARCHAR 参数的值作为 Java 编程语言中的 String 进行检索。|  
|[getObject](../../../connect/jdbc/reference/getobject-method-sqlservercallablestatement.md)|检索指定参数的值作为 Java 编程语言中的一个对象。|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 等待此 CallableStatement 对象运行的秒数。|  
|[getRef](../../../connect/jdbc/reference/getref-method-sqlservercallablestatement.md)|检索指定参数作为 Java 编程语言中 Ref 对象的值。|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索此 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象的响应缓冲模式。|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）作为 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象检索当前结果。|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的结果集并发机制，这些对象由此 CallableStatement 对象生成。|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的结果集可保持性，这些对象由此 CallableStatement 对象生成。|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的结果集类型，这些对象由此 CallableStatement 对象生成。|  
|[getShort](../../../connect/jdbc/reference/getshort-method-sqlservercallablestatement.md)|检索指定参数作为 Java 编程语言中的 short 的值。|  
|[getString](../../../connect/jdbc/reference/getstring-method-sqlservercallablestatement.md)|检索指定参数作为 Java 编程语言中的 String 的值。|  
|[getSQLXML](../../../connect/jdbc/reference/getsqlxml-method-sqlservercallablestatement.md)|检索指定参数的值作为 java.sql.SQLXML 对象。|  
|[getTime](../../../connect/jdbc/reference/gettime-method-sqlservercallablestatement.md)|检索指定参数的值作为 Java 编程语言中的 java.sql.Time 对象。|  
|[getTimestamp](../../../connect/jdbc/reference/gettimestamp-method-sqlservercallablestatement.md)|检索指定参数的值作为 Java 编程语言中的 java.sql.Timestamp 对象。|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索当前结果作为更新计数。|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlservercallablestatement.md)|检索指定参数作为 Java 编程语言中的 URL 对象的值。|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索调用此 CallableStatement 对象时报告的第一个警告。|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）指示此 Statement 对象是否已关闭。|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）返回指示是否可以将语句添加到用户提供的语句池的一个值。|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlservercallablestatement.md)|指示此语句对象是否为指定接口的包装。|  
|[registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md)|注册 OUT 参数。|  
|[setArray](../../../connect/jdbc/reference/setarray-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）将指定的参数编号设置为给定的 Array 对象。|  
|[setAsciiStream](../../../connect/jdbc/reference/setasciistream-sqlservercallablestatement.md)|将指定参数设置为给定的输入流。|  
|[setBigDecimal](../../../connect/jdbc/reference/setbigdecimal-method-sqlservercallablestatement.md)|将指定参数编号设置为给定 BigDecimal 对象。|  
|[setBinaryStream](../../../connect/jdbc/reference/setbinarystream-sqlservercallablestatement.md)|将指定参数设置为指定的输入流。|  
|[setBlob](../../../connect/jdbc/reference/setblob-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）将指定参数设置为给定的 Blob 对象。|  
|[setboolean](../../../connect/jdbc/reference/setboolean-method-sqlservercallablestatement.md)|将指定参数设置为给定的 Boolean 值。|  
|[setByte](../../../connect/jdbc/reference/setbyte-method-sqlservercallablestatement.md)|将指定参数设置为给定的 byte 值。|  
|[setBytes](../../../connect/jdbc/reference/setbytes-method-sqlservercallablestatement.md)|将指定参数设置为给定的字节值数组。|  
|[setCharacterStream](../../../connect/jdbc/reference/setcharacterstream-method-sqlservercallablestatement.md)|将指定参数设置为给定的 Reader 对象。|  
|[setClob](../../../connect/jdbc/reference/setclob-method-sqlservercallablestatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）将指定参数设置为指定对象。|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）将 SQL 游标名称设置为给定字符串，该字符串将由随后的执行方法使用。|  
|[setDate](../../../connect/jdbc/reference/setdate-method-sqlservercallablestatement.md)|将指定参数设置为给定的日期值。|  
|[setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlservercallablestatement.md)|将指定列的值设置为 [DateTimeOffset 类](../../../connect/jdbc/reference/datetimeoffset-class.md)值。|  
|[setDouble](../../../connect/jdbc/reference/setdouble-method-sqlservercallablestatement.md)|将指定参数设置为给定 double 值。|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）设置转义处理模式。|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）为 JDBC 驱动程序提供提示以指明处理结果集行时应采用的方向。|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）为 JDBC 驱动程序提供提示以指明在需要更多行时应从数据库中提取的行数。|  
|[setFloat](../../../connect/jdbc/reference/setfloat-method-sqlservercallablestatement.md)|将指定参数设置为指定的 float 值。|  
|[setInt](../../../connect/jdbc/reference/setint-method-sqlservercallablestatement.md)|将指定参数设置为指定的 int 值。|  
|[setLong](../../../connect/jdbc/reference/setlong-method-sqlservercallablestatement.md)|将指定参数设置为指定的 long 值。|  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）将存储字符或二进制值的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 列中的最大字节数限制设置为指定的字节数。|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）将任何 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象可包含的最大行数限制设置为指定的数目。|  
|[setNCharacterStream](../../../connect/jdbc/reference/setncharacterstream-method-sqlservercallablestatement.md)|将指定参数设置为指定的 Reader 对象。|  
|[setNClob](../../../connect/jdbc/reference/setnclob-method-sqlservercallablestatement.md)|将指定参数设置为指定对象。|  
|[setNString](../../../connect/jdbc/reference/setnstring-method-sqlservercallablestatement.md)|将指定参数设置为指定的 String 对象。|  
|[setNull](../../../connect/jdbc/reference/setnull-method-sqlservercallablestatement.md)|根据要设置的参数的类型，将指定参数设置为 Null 值。|  
|[setObject](../../../connect/jdbc/reference/setobject-method-sqlservercallablestatement.md)|使用给定对象设置指定参数的值。|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）请求语句入池或不入池。 默认情况下，创建时 SQLServerCallableStatement 对象是可以合并的。|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）将驱动程序等待 CallableStatement 对象运行的秒数设置为指定的秒数。|  
|[setRef](../../../connect/jdbc/reference/setref-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）将指定参数设置为指定的 Ref 对象。|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）将此 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象的响应缓冲模式设置为不区分大小写的“String full”或“adaptive”。|  
|[setShort](../../../connect/jdbc/reference/setshort-method-sqlservercallablestatement.md)|将指定参数设置为指定的 Short 值。|  
|[setString](../../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md)|将指定参数设置为指定的 String 值。|  
|[setSQLXML](../../../connect/jdbc/reference/setsqlxml-method-sqlservercallablestatement.md)|将指定参数设置为指定的 SQLXML 对象。|  
|[setTime](../../../connect/jdbc/reference/settime-method-sqlservercallablestatement.md)|将指定参数设置为指定的 time 值。|  
|[setTimestamp](../../../connect/jdbc/reference/settimestamp-method-sqlservercallablestatement.md)|将指定参数设置为指定的 timestamp 值。|  
|[setUnicodeStream](../../../connect/jdbc/reference/setunicodestream-method-sqlserverpreparedstatement.md)|（继承自 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)。）将指定参数编号设置为将有指定字节数的给定输入流。|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlservercallablestatement.md)|将指定参数设置为指定的 URL 值。|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md)|返回一个实现指定接口的对象，从而允许访问特定于 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 的方法。|  
|[wasNull](../../../connect/jdbc/reference/wasnull-method-sqlservercallablestatement.md)|检索读取的最后一个 OUT 参数是否具有 SQL NULL 值。|  
  
## <a name="inherited-methods"></a>继承的方法  
  
|类继承自：|方法|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerPreparedStatement|addBatch、clearBatch、clearParameters、close、execute、executeBatch、executeQuery、executeUpdate、getMetaData、getParameterMetaData、setArray、setAsciiStream、setBigDecimal、setBinaryStream、setBlob、setboolean、setByte、setBytes、setCharacterStream、setClob、setDate、setDouble、setFloat、setInt、setLong、setNull、setObject、setRef、setShort、setString、setTime、setTimestamp、setUnicodeStream、setURL|  
|com.microsoft.sqlserver.jdbc.SQLServerStatement|cancel、clearWarnings、execute、executeUpdate、getConnection、getFetchDirection、getFetchSize、getGeneratedKeys、getMaxFieldSize、getMaxRows、getMoreResults、getQueryTimeout、getResultSet、getResultSetConcurrency、getResultSetHoldability、getResultSetType、getUpdateCount、getWarnings、isPoolable、setCursorName、setEscapeProcessing、setFetchDirection、setFetchSize、setMaxFieldSize、setMaxRows、setPoolable、setQueryTimeout|  
|class java.lang.Object|clone、equals、finalize、getClass、hashCode、notify、notifyAll、toString、wait|  
|java.sql.PreparedStatement|addBatch、clearParameters、execute、executeQuery、executeUpdate、getMetaData、getParameterMetaData、getSQLXML、setArray、setAsciiStream、setBigDecimal、setBinaryStream、setBlob、setboolean、setByte、setBytes、setCharacterStream、setClob、setDate、setDate、setDouble、setFloat、setInt、setLong、setNull、setObject、setRef、setShort、setString、setSQLXML、setTime、setTimestamp、setUnicodeStream、setURL|  
|java.sql.Statement|addBatch、cancel、clearBatch、clearWarnings、close、execute、executeBatch、executeQuery、executeUpdate、getConnection、getFetchDirection、getFetchSize、getGeneratedKeys、getMaxFieldSize、getMaxRows、getMoreResults、getQueryTimeout、getResultSet、getResultSetConcurrency、getResultSetHoldability、getResultSetType、getUpdateCount、getWarnings、setCursorName、setEscapeProcessing、setFetchDirection、setFetchSize、setMaxFieldSize、setMaxRows、setQueryTimeout|  
|java.sql.Wrapper|isWrapperFor、unwrap|  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerCallableStatement 类](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
