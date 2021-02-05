---
description: SQLServerResultSet 成员
title: SQLServerResultSet 成员 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2a438d5d-2d6a-46a0-a2ae-f35fbae4a472
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bf8e9b71b638a3836ebefb71e3d574183372267e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180110"
---
# <a name="sqlserverresultset-members"></a>SQLServerResultSet 成员
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  下表列出了由 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 类公开的成员。  
  
## <a name="constructors"></a>构造函数  
 无。  
  
## <a name="fields"></a>字段  
  
|名称|说明|  
|----------|-----------------|  
|[CONCUR_SS_OPTIMISTIC_CC](../../../connect/jdbc/reference/concur-ss-optimistic-cc-field-sqlserverresultset.md)|用于指定无行锁的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 读/写乐观并发类型。|  
|[CONCUR_SS_OPTIMISTIC_CCVAL](../../../connect/jdbc/reference/concur-ss-optimistic-ccval-field-sqlserverresultset.md)|用于指定无行锁的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 读/写乐观并发类型。|  
|[CONCUR_SS_SCROLL_LOCKS](../../../connect/jdbc/reference/concur-ss-scroll-locks-field-sqlserverresultset.md)|用于指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 有行锁的读/写乐观并发类型。|  
|[TYPE_SS_DIRECT_FORWARD_ONLY](../../../connect/jdbc/reference/type-ss-direct-forward-only-field-sqlserverresultset.md)|用于指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 快速只进只读游标类型。|  
|[TYPE_SS_SCROLL_DYNAMIC](../../../connect/jdbc/reference/type-ss-scroll-dynamic-field-sqlserverresultset.md)|用于指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 动态游标类型。|  
|[TYPE_SS_SCROLL_KEYSET](../../../connect/jdbc/reference/type-ss-scroll-keyset-field-sqlserverresultset.md)|用于指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 键集游标类型。|  
|[TYPE_SS_SCROLL_STATIC](../../../connect/jdbc/reference/type-ss-scroll-static-field-sqlserverresultset.md)|用于指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 静态游标类型。|  
|[TYPE_SS_SERVER_CURSOR_FORWARD_ONLY](../../../connect/jdbc/reference/type-ss-server-cursor-forward-only-field-sqlserverresultset.md)|用于指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 快速只进只读游标类型。|  
  
## <a name="inherited-fields"></a>继承的字段  
  
|类继承自：|说明|  
|---------------------------|-----------------|  
|java.sql.ResultSet|CLOSE_CURSORS_AT_COMMIT、CONCUR_READ_ONLY、CONCUR_UPDATABLE、FETCH_FORWARD、FETCH_REVERSE、FETCH_UNKNOWN、HOLD_CURSORS_OVER_COMMIT、TYPE_FORWARD_ONLY、TYPE_SCROLL_INSENSITIVE 和 TYPE_SCROLL_SENSITIVE|  
  
## <a name="methods"></a>方法  
  
|名称|说明|  
|----------|-----------------|  
|[absolute](../../../connect/jdbc/reference/absolute-method-sqlserverresultset.md)|将游标移到此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中指定的行。|  
|[afterLast](../../../connect/jdbc/reference/afterlast-method-sqlserverresultset.md)|将光标移到此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的最后一行之后。|  
|[beforeFirst](../../../connect/jdbc/reference/beforefirst-method-sqlserverresultset.md)|将光标移到此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的第一行之前。|  
|[cancelRowUpdates](../../../connect/jdbc/reference/cancelrowupdates-method-sqlserverresultset.md)|取消对此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中的当前行所做的更新。|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverresultset.md)|清除此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象上所报告的所有警告。|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverresultset.md)|立即释放此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的数据库和 JDBC 资源，而不是在此对象自动关闭时等待上述操作发生。|  
|[deleteRow](../../../connect/jdbc/reference/deleterow-method-sqlserverresultset.md)|从此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象和基础数据库中删除当前行。|  
|[finalize](../../../connect/jdbc/reference/finalize-method-sqlserverresultset.md)|显式关闭此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象。|  
|[findColumn](../../../connect/jdbc/reference/findcolumn-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中指定列名称的首个匹配列的索引。|  
|[first](../../../connect/jdbc/reference/first-method-sqlserverresultset.md)|将游标移到此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的第一行。|  
|[getArray](../../../connect/jdbc/reference/getarray-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Array 对象。|  
|[getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 ASCII 字符流。|  
|[getBigDecimal](../../../connect/jdbc/reference/getbigdecimal-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列索引的值作为 java.math.BigDecimal。|  
|[getBinaryStream](../../../connect/jdbc/reference/getbinarystream-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为未解释字节的二进制流。|  
|[getBlob](../../../connect/jdbc/reference/getblob-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 Blob 对象。|  
|[getBoolean](../../../connect/jdbc/reference/getboolean-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 boolean。|  
|[getByte](../../../connect/jdbc/reference/getbyte-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 byte。|  
|[getBytes](../../../connect/jdbc/reference/getbytes-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的字节数组。|  
|[getCharacterStream](../../../connect/jdbc/reference/getcharacterstream-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 java.io.Reader 对象。|  
|[getClob](../../../connect/jdbc/reference/getclob-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 Clob 对象。|  
|[getConcurrency](../../../connect/jdbc/reference/getconcurrency-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的并发模式。|  
|[getCursorName](../../../connect/jdbc/reference/getcursorname-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象使用的 SQL 游标的名称。|  
|[getDate](../../../connect/jdbc/reference/getdate-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 java.sql.Date 对象。|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-sqlserverresultset.md)|将指定列的值作为 [DateTimeOffset 类](../../../connect/jdbc/reference/datetimeoffset-class.md)对象进行检索。|  
|[getDouble](../../../connect/jdbc/reference/getdouble-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 double。|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的提取方向。|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的提取大小。|  
|[getFloat](../../../connect/jdbc/reference/getfloat-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 float。|  
|[getHoldability](../../../connect/jdbc/reference/getholdability-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的可保持性。|  
|[getInt](../../../connect/jdbc/reference/getint-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 int。|  
|[getLong](../../../connect/jdbc/reference/getlong-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 long。|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的列数、列的类型和属性。|  
|[getNCharacterStream](../../../connect/jdbc/reference/getncharacterstream-method-sqlserverresultset.md)|检索 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Reader 对象。|  
|[getNClob](../../../connect/jdbc/reference/getnclob-method-sqlserverresultset.md)|检索 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 NClob 对象。|  
|[getNString](../../../connect/jdbc/reference/getnstring-method-sqlserverresultset.md)|检索 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的字符串。|  
|[getObject](../../../connect/jdbc/reference/getobject-method-sqlserverresultset.md)|获取此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的一个对象。|  
|[getRef](../../../connect/jdbc/reference/getref-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 Ref 对象。|  
|[getRow](../../../connect/jdbc/reference/getrow-method-sqlserverresultset.md)|检索当前行号。|  
|[getShort](../../../connect/jdbc/reference/getshort-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 short。|  
|[getStatement](../../../connect/jdbc/reference/getstatement-method-sqlserverresultset.md)|检索 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象，此对象生成了此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象。|  
|[getString](../../../connect/jdbc/reference/getstring-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的字符串。|  
|[getSQLXML](../../../connect/jdbc/reference/getsqlxml-method-sqlserverresultset.md)|检索 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 SQLXML 对象。|  
|[getTime](../../../connect/jdbc/reference/gettime-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 java.sql.Time 对象。|  
|[getTimestamp](../../../connect/jdbc/reference/gettimestamp-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Java 编程语言中的 java.sql.Timestamp 对象。|  
|[getType](../../../connect/jdbc/reference/gettype-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的游标类型。|  
|[getUnicodeStream](../../../connect/jdbc/reference/getunicodestream-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 Unicode 字符流。|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverresultset.md)|检索此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的当前行中指定列的值作为 URL 对象。|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverresultset.md)|检索在此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象上进行调用时所报告的第一个警告。|  
|[insertRow](../../../connect/jdbc/reference/insertrow-method-sqlserverresultset.md)|将插入行的内容插入到此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中，并进而插入到数据库中。|  
|[isAfterLast](../../../connect/jdbc/reference/isafterlast-method-sqlserverresultset.md)|检索游标是否位于此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中的最后一行之后。|  
|[isBeforeFirst](../../../connect/jdbc/reference/isbeforefirst-method-sqlserverresultset.md)|检索游标是否位于此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中的第一行之前。|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverresultset.md)|指示此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象是否已关闭。|  
|[isFirst](../../../connect/jdbc/reference/isfirst-method-sqlserverresultset.md)|检索游标是否位于此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的第一行上。|  
|[isLast](../../../connect/jdbc/reference/islast-method-sqlserverresultset.md)|检索游标是否位于此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象的最后一行上。|  
|[last](../../../connect/jdbc/reference/last-method-sqlserverresultset.md)|将游标移到此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中的最后一行。|  
|[moveToCurrentRow](../../../connect/jdbc/reference/movetocurrentrow-method-sqlserverresultset.md)|将光标移到所记住的光标位置，通常为当前行。|  
|[moveToInsertRow](../../../connect/jdbc/reference/movetoinsertrow-method-sqlserverresultset.md)|将游标移到插入行。|  
|[下一步](../../../connect/jdbc/reference/next-method-sqlserverresultset.md)|将游标从当前位置下移一行。|  
|[previous](../../../connect/jdbc/reference/previous-method-sqlserverresultset.md)|将游标移到此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中的上一行。|  
|[refreshRow](../../../connect/jdbc/reference/refreshrow-method-sqlserverresultset.md)|使用数据库中当前行的最新值刷新此行。|  
|[relative](../../../connect/jdbc/reference/relative-method-sqlserverresultset.md)|将光标沿正向或反向移动相对于当前行的指定行数。|  
|[rowDeleted](../../../connect/jdbc/reference/rowdeleted-method-sqlserverresultset.md)|检索是否已删除某行。|  
|[rowInserted](../../../connect/jdbc/reference/rowinserted-method-sqlserverresultset.md)|检索当前行是否曾插入内容。|  
|[rowUpdated](../../../connect/jdbc/reference/rowupdated-method-sqlserverresultset.md)|检索当前行是否已更新。|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverresultset.md)|提供提示以指明将用于处理此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象中的行的方向。|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverresultset.md)|为 JDBC 驱动程序提供提示，以指明在此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象需要更多行时应从数据库中提取的行数。|  
|[updateArray](../../../connect/jdbc/reference/updatearray-method-sqlserverresultset.md)|使用 Array 对象更新指定列。|  
|[updateAsciiStream](../../../connect/jdbc/reference/updateasciistream-method-sqlserverresultset.md)|使用 ASCII 流值更新指定列。|  
|[updateBigDecimal](../../../connect/jdbc/reference/updatebigdecimal-method-sqlserverresultset.md)|使用 BigDecimal 对象更新指定列。|  
|[updateBinaryStream](../../../connect/jdbc/reference/updatebinarystream-method-sqlserverresultset.md)|使用二进制流值更新指定列。|  
|[updateBlob](../../../connect/jdbc/reference/updateblob-method-sqlserverresultset.md)|使用 java.sql.Blob 值更新指定列。|  
|[updateBoolean](../../../connect/jdbc/reference/updateboolean-method-sqlserverresultset.md)|使用布尔值更新指定列。|  
|[updateByte](../../../connect/jdbc/reference/updatebyte-method-sqlserverresultset.md)|使用字节值更新指定列。|  
|[updateBytes](../../../connect/jdbc/reference/updatebytes-method-sqlserverresultset.md)|使用字节值构成的数组更新指定列。|  
|[updateCharacterStream](../../../connect/jdbc/reference/updatecharacterstream-method-sqlserverresultset.md)|使用字符流值更新指定列。|  
|[updateClob](../../../connect/jdbc/reference/updateclob-method-sqlserverresultset.md)|使用 java.sql.Clob 值更新指定列。|  
|[updateDate](../../../connect/jdbc/reference/updatedate-method-sqlserverresultset.md)|使用日期值更新指定列。|  
|[updateDateTimeOffset](../../../connect/jdbc/reference/updatedatetimeoffset-sqlserverresultset.md)|更新 [DateTimeOffset 类](../../../connect/jdbc/reference/datetimeoffset-class.md)列。|  
|[updateDouble](../../../connect/jdbc/reference/updatedouble-method-sqlserverresultset.md)|使用 double 值更新指定列。|  
|[updateFloat](../../../connect/jdbc/reference/updatefloat-method-sqlserverresultset.md)|使用 float 值更新指定列。|  
|[updateInt](../../../connect/jdbc/reference/updateint-method-sqlserverresultset.md)|使用 int 值更新指定列。|  
|[updateLong](../../../connect/jdbc/reference/updatelong-method-sqlserverresultset.md)|使用 long 值更新指定列。|  
|[updateNCharacterStream](../../../connect/jdbc/reference/updatencharacterstream-method-sqlserverresultset.md)|使用字符流值更新指定列。|  
|[updateNClob](../../../connect/jdbc/reference/updatenclob-method-sqlserverresultset.md)|使用指定对象值更新指定列。|  
|[updateNString](../../../connect/jdbc/reference/updatenstring-method-sqlserverresultset.md)|使用字符串值更新指定列。|  
|[updateNull](../../../connect/jdbc/reference/updatenull-method-sqlserverresultset.md)|使用 Null 值更新指定列。|  
|[updateObject](../../../connect/jdbc/reference/updateobject-method-sqlserverresultset.md)|使用 Object 值更新指定列。|  
|[updateRef](../../../connect/jdbc/reference/updateref-method-sqlserverresultset.md)|使用 java.sql.Ref 值更新指定列。|  
|[updateRow](../../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md)|使用此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象当前行的新内容更新基础数据库。|  
|[updateShort](../../../connect/jdbc/reference/updateshort-method-sqlserverresultset.md)|使用 short 值更新指定列。|  
|[updateString](../../../connect/jdbc/reference/updatestring-method-sqlserverresultset.md)|使用字符串值更新指定列。|  
|[updateSQLXML](../../../connect/jdbc/reference/updatesqlxml-method-sqlserverresultset.md)|使用 SQLXML 值更新指定列。|  
|[updateTime](../../../connect/jdbc/reference/updatetime-method-sqlserverresultset.md)|使用时间值更新指定列。|  
|[updateTimestamp](../../../connect/jdbc/reference/updatetimestamp-method-sqlserverresultset.md)|使用时间戳的值更新指定列。|  
|[wasNull](../../../connect/jdbc/reference/wasnull-method-sqlserverresultset.md)|验证所读取的最后一个值是否为 Null 值。|  
  
## <a name="inherited-methods"></a>继承的方法  
  
|类继承自：|方法|  
|---------------------------|-------------|  
|java.lang.Object|clone、equals、getClass、hashCode、notify、notifyAll、toString、wait|  
|java.sql.Wrapper|isWrapperFor、unwrap|  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
