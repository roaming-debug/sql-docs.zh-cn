---
description: SQLServerPreparedStatement 成员
title: SQLServerPreparedStatement 成员 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2363902f-d4c6-4cd4-a5fc-86079eb9e418
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 74ae525bdd1c05eb01f9bce98cba9ef987087a30
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167998"
---
# <a name="sqlserverpreparedstatement-members"></a>SQLServerPreparedStatement 成员
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  下表列出了由 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) 类公开的成员。  
  
## <a name="constructors"></a>构造函数  
 无。  
  
## <a name="fields"></a>字段  
 无。  
  
## <a name="inherited-fields"></a>继承的字段  
  
|类继承自：|方法|  
|---------------------------|-------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS、CLOSE_CURRENT_RESULT、EXECUTE_FAILED、KEEP_CURRENT_RESULT、NO_GENERATED_KEYS、RETURN_GENERATED_KEYS、SUCCESS_NO_INFO|  
  
## <a name="methods"></a>方法  
  
|名称|说明|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md)|为此 Statement 对象的一批命令添加一组参数。|  
|[cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）取消当前正由此 Statement 对象运行的 SQL 语句。|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverpreparedstatement.md)|为此 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象清空当前 SQL 命令列表。|  
|[clearParameters](../../../connect/jdbc/reference/clearparameters-method-sqlserverpreparedstatement.md)|立即清除当前参数值。|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）清除对此 Statement 对象报告的所有警告。|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverpreparedstatement.md)|立即释放此 Statement 对象的数据库和 JDBC 资源，而不是等待它们被自动释放。|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverpreparedstatement.md)|运行此 Statement 对象中的 SQL 语句，该语句可以是任何类型的 SQL 语句。|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverpreparedstatement.md)|向数据库提交要运行的命令批。 如果所有命令都成功运行，则返回一个更新计数数组。|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md)|运行此 Statement 对象中的 SQL 查询并返回由该查询生成的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象。|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverpreparedstatement.md)|运行此 Statement 对象中的 SQL 语句，该语句必须是 SQL INSERT、UPDATE、MERGE 或 DELETE 语句；或是不返回任何内容的 SQL 语句，如 DDL 语句。|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索生成了此 Statement 对象的 [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) 对象。|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索从数据库表中提取行的方向，该方向是由此 Statement 对象生成的结果集的默认值。|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索结果集行数，该行数是由此 Statement 对象生成的结果集对象的默认提取大小。|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索因运行此 Statement 对象而创建的任何自动生成的键。|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索可为由此 Statement 对象生成的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中的字符和二进制列值返回的最大字节数。|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索由此 Statement 对象生成的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象可包含的最大行数。|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverpreparedstatement.md)|检索 [SQLServerResultSetMetaData Class](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md) 对象，该对象包含有关执行此 Statement 对象时将返回的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的列信息。|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）移动到此 Statement 对象的下一个结果。|  
|[getParameterMetaData](../../../connect/jdbc/reference/getparametermetadata-method-sqlserverpreparedstatement.md)|检索此 Statement 对象的参数个数、类型和属性。|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索此 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象的响应缓冲模式。|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 等待此 Statement 对象运行的秒数。|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）作为 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象检索当前结果。|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的结果集并发机制，这些对象由此 Statement 对象生成。|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的结果集可保持性，这些对象由此 Statement 对象生成。|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的结果集类型，这些对象由此 Statement 对象生成。|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）作为更新计数检索当前结果。|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）检索调用此 Statement 对象时报告的第一个警告。|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）指示此 Statement 对象是否已关闭。|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）返回指示是否可以将语句添加到用户提供的语句池的一个值。|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverpreparedstatement.md)|指示此语句对象是否为指定接口的包装。|  
|[setArray](../../../connect/jdbc/reference/setarray-method-sqlserverpreparedstatement.md)|将指定的参数编号设置为给定的 Array 对象。|  
|[setAsciiStream](../../../connect/jdbc/reference/setasciistream-method-sqlserverpreparedstatement.md)|将指定的参数编号设置为给定的 InputStream 对象。|  
|[setBigDecimal](../../../connect/jdbc/reference/setbigdecimal-method-sqlserverpreparedstatement.md)|将指定参数编号设置为给定 BigDecimal 对象。|  
|[setBinaryStream](../../../connect/jdbc/reference/setbinarystream-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的输入流。|  
|[setBlob](../../../connect/jdbc/reference/setblob-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 Blob 对象。|  
|[setboolean](../../../connect/jdbc/reference/setboolean-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 Boolean 值。|  
|[setByte](../../../connect/jdbc/reference/setbyte-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的字节值。|  
|[setBytes](../../../connect/jdbc/reference/setbytes-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的字节数组。|  
|[setCharacterStream](../../../connect/jdbc/reference/setcharacterstream-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 Reader 对象。|  
|[setClob](../../../connect/jdbc/reference/setclob-method-sqlserverpreparedstatement.md)|将指定参数设置为给定 Clob 对象。|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）将 SQL 游标名称设置为指定字符串，以供后续执行方法使用。|  
|[setDate](../../../connect/jdbc/reference/setdate-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的日期值。|  
|[setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlserverpreparedstatement.md)|将指定列的值设置为 [DateTimeOffset 类](../../../connect/jdbc/reference/datetimeoffset-class.md)值。|  
|[setDouble](../../../connect/jdbc/reference/setdouble-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 Double 值。|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）设置转义处理模式。|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）为 JDBC 驱动程序提供提示以指明处理结果集行时应采用的方向。|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）为 JDBC 驱动程序提供提示以指明在需要更多行时应从数据库中提取的行数。|  
|[setFloat](../../../connect/jdbc/reference/setfloat-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 float 值。|  
|[setInt](../../../connect/jdbc/reference/setint-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 int 值。|  
|[setLong](../../../connect/jdbc/reference/setlong-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 long 值。|  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）将存储字符或二进制值的 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 列中的最大字节数限制设置为给定的字节数。|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）将任何 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象可包含的最大行数限制设置为给定的数目。|  
|[setNCharacterStream](../../../connect/jdbc/reference/setncharacterstream-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 Reader 对象。|  
|[setNClob](../../../connect/jdbc/reference/setnclob-method-sqlserverpreparedstatement.md)|将指定参数设置为指定对象。|  
|[setNull](../../../connect/jdbc/reference/setnull-method-sqlserverpreparedstatement.md)|根据要设置的参数的类型，将指定参数设置为 Null 值。|  
|[setNString](../../../connect/jdbc/reference/setnstring-method-int-java-lang-string.md)|将指定参数设置为指定的 String 对象。|  
|[setObject](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md)|使用给定对象设置指定参数的值。|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）请求语句入池或不入池。 默认情况下，创建时 SQLServerPreparedStatement 对象是可以合并的。|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）将驱动程序等待 Statement 对象运行的秒数设置为指定的秒数。|  
|[setRef](../../../connect/jdbc/reference/setref-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 Ref 对象。|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|（继承自 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)。）将此 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象的响应缓冲模式设置为不区分大小写的“String full”或“adaptive”。|  
|[setShort](../../../connect/jdbc/reference/setshort-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 Short 值。|  
|[setString](../../../connect/jdbc/reference/setstring-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 String 值。|  
|[setSQLXML](../../../connect/jdbc/reference/setsqlxml-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 SQLXML 对象。|  
|[setTime](../../../connect/jdbc/reference/settime-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 time 值。|  
|[setTimestamp](../../../connect/jdbc/reference/settimestamp-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 timestamp 值。|  
|[setUnicodeStream](../../../connect/jdbc/reference/setunicodestream-method-sqlserverpreparedstatement.md)|将指定参数编号设置为指定的输入流，而指定输入流将含有指定字节数。|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverpreparedstatement.md)|将指定参数设置为指定的 URL 值。|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverpreparedstatement.md)|返回一个实现指定接口的对象，从而允许访问特定于 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 的方法。|  
  
## <a name="inherited-methods"></a>继承的方法  
  
|类继承自：|方法|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerStatement|cancel、clearWarnings、execute、executeUpdate、getConnection、getFetchDirection、getFetchSize、getGeneratedKeys、getMaxFieldSize、getMaxRows、getMoreResults、getQueryTimeout、getResultSet、getResultSetConcurrency、getResultSetHoldability、getResultSetType、getUpdateCount、getWarnings、isPoolable、setCursorName、setEscapeProcessing、setFetchDirection、setFetchSize、setMaxFieldSize、setMaxRows、setPoolable、setQueryTimeout|  
|java.lang.Object|clone、equals、getClass、hashCode、notify、notifyAll、toString、wait|  
|java.sql.Statement|cancel、clearWarnings、execute、executeUpdate、getConnection、getFetchDirection、getFetchSize、getGeneratedKeys、getMaxFieldSize、getMaxRows、getMoreResults、getQueryTimeout、getResultSet、getResultSetConcurrency、getResultSetHoldability、getResultSetType、getUpdateCount、getWarnings、setCursorName、setEscapeProcessing、setFetchDirection、setFetchSize、setMaxFieldSize、setMaxRows、setQueryTimeout|  
|java.sql.Wrapper|isWrapperFor、unwrap|  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerPreparedStatement 类](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
