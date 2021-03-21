---
description: 从 DB-Library 转换到 ODBC 大容量复制
title: 从 DB-Library 转换为 ODBC 大容量复制 |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- converting DB-Library to ODBC bulk copy
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [ODBC], DB-Library bulk copy
- ODBC, bulk copy operations
- DB-Library bulk copy
ms.assetid: 0bc15bdb-f19f-4537-ac6c-f249f42cf07f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bbaf14c98f20b5d070d1679da894d97429098dcc
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754737"
---
# <a name="converting-from-db-library-to-odbc-bulk-copy"></a>从 DB-Library 转换到 ODBC 大容量复制
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  将 DB-Library 大容量复制程序转换为 ODBC 非常简单，因为 Native Client ODBC 驱动程序支持的大容量复制函数 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 类似于 DB-Library 大容量复制函数，但以下情况例外：  
  
-   DB-Library 应用程序将 DBPROCESS 结构的指针作为大容量复制函数的第一个参数进行传递。 在 ODBC 应用程序中，DBPROCESS 指针由 ODBC 连接句柄取代。  
  
-   DB-Library 应用程序在连接之前调用 **BCP_SETL** ，以在 DBPROCESS 上启用大容量复制操作。 ODBC 应用程序在连接之前调用 [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) ，以对连接句柄启用批量操作：  
  
    ```  
    SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP,  
        (void *)SQL_BCP_ON, SQL_IS_INTEGER);  
    ```  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序不支持 DB-Library 消息和错误处理程序; 必须调用 **SQLGetDiagRec** 来获取 ODBC 大容量复制函数引发的错误和消息。 大容量复制函数的 ODBC 版本返回标准的大容量复制返回代码 SUCCEED 或 FAILED，而不是 ODBC 样式的返回代码，比如 SQL_SUCCESS 或 SQL_ERROR。  
  
-   为 DB-Library [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md)*varlen* 参数指定的值的解释方式与 ODBC **bcp_bind**_cbData_ 参数不同。  
  
    |指示条件|DB-Library *varlen* 值|ODBC *cbData* 值|  
    |-------------------------|--------------------------------|-------------------------|  
    |提供 Null 值|0|-1 (SQL_NULL_DATA)|  
    |提供变量数据|-1|-10 (SQL_VARLEN_DATA)|  
    |零长度字符或二进制字符串|NA|0|  
  
     在 DB-LIBRARY 中， *varlen* 值为-1 表示提供可变长度数据，ODBC *cbData* 中的数据将被解释为仅提供 NULL 值。 将-1 的任何 DB-Library *varlen* 规范更改为 SQL_VARLEN_DATA，并将任何 *varlen* 规范（0）更改为 SQL_NULL_DATA。  
  
-   _File_collen_ 和 ODBC [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md)*cbUserData* 的 DB-Library **bcp_colfmt** 与上面所述的 **bcp_bind**_varlen_ 和 *cbData* 参数相同。 将-1 的任何 DB-Library *file_collen* 规范更改为 SQL_VARLEN_DATA，并将任何 *file_collen* 规范（0）更改为 SQL_NULL_DATA。  
  
-   ODBC [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md)函数的 *iValue* 参数是一个 void 指针。 在 DB-LIBRARY 中， *iValue* 是一个整数。 将 ODBC *iValue* 的值强制转换为 void *。  
  
-   **Bcp_control** 选项 BCPMAXERRS 指定大容量复制操作失败之前有多少单独行出现错误。 BCPMAXERRS 的默认值为 0 (第一次错误时失败) 在 ODBC 版本的 **bcp_control** 的 DB-Library 版本中为10。 必须更改依赖于默认值0的 DB-Library 应用程序，以调用 ODBC **bcp_control** 将 BCPMAXERRS 设置为0。  
  
-   ODBC **bcp_control** 函数支持 **bcp_control** 的 DB-Library 版本不支持以下选项：  
  
    -   BCPODBC  
  
         如果设置为 TRUE，则指定以字符格式保存的 **datetime** 和 **smalldatetime** 值将具有 ODBC 时间戳转义序列前缀和后缀。 这仅适用于 BCP_OUT 操作。  
  
         将 BCPODBC 设置为 FALSE 时，转换为字符串的 **日期时间** 值将输出为：  
  
        ```  
        1997-01-01 00:00:00.000  
        ```  
  
         如果将 BCPODBC 设置为 TRUE，则相同的 **datetime** 值将输出为：  
  
        ```  
        {ts '1997-01-01 00:00:00.000' }  
        ```  
  
    -   BCPKEEPIDENTITY  
  
         设置为 TRUE 时，指定大容量复制函数插入为具有标识约束的列提供的数据值。 如果未进行设置，则为插入的行生成新标识值。  
  
    -   BCPHINTS  
  
         指定各种大容量复制优化措施。 该选项无法在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 6.5 或更早版本上使用。  
  
    -   BCPFILECP  
  
         指定大容量复制文件的代码页。  
  
    -   BCPUNICODEFILE  
  
         指定字符模式大容量复制文件是 Unicode 文件。  
  
-   ODBC **bcp_colfmt** 函数不支持 SQLCHAR 的 *file_type* 指示器，因为它与 ODBC SQLCHAR typedef 冲突。 使用 SQLCHARACTER 代替 **bcp_colfmt**。  
  
-   在大容量复制函数的 ODBC 版本中，在字符串中使用 **datetime** 和 **smalldatetime** 值的格式是以 yyyy-mm-dd hh： mm： SS 格式表示的 odbc 格式; **smalldatetime** 值使用 yyyy-mm-dd hh： mm： ss 格式。  
  
     大容量复制函数的 DB-Library 版本使用几种格式的字符串接受 **datetime** 和 **smalldatetime** 值：  
  
    -   默认格式为 *mmm dd yyyy hh： mmxx* ，其中 *xx* 是 AM 或 PM。  
  
    -   DB-Library **dbconvert** 函数支持的任何格式的 **datetime** 和 **smalldatetime** 字符字符串。  
  
    -   当在客户端网络实用工具的 "DB-Library **选项**" 选项卡上选中 "**使用国际设置**" 框时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，DB-Library 大容量复制函数也会接受为客户端计算机注册表的区域设置定义的区域日期格式的日期。  
  
     DB-Library 大容量复制函数不接受 ODBC **datetime** 和 **smalldatetime** 格式。  
  
     如果 SQL_SOPT_SS_REGIONALIZE 语句属性设置为 SQL_RE_ON，则 ODBC 大容量复制函数接受的日期格式可以是为客户端计算机注册表的区域设置定义的区域日期格式。  
  
-   当以字符格式输出 **money** 值时，ODBC 大容量复制函数提供四位数的精度，没有逗号分隔符;DB-Library 版本仅提供两位数的精度，并包含逗号分隔符。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;ODBC&#41;执行大容量复制操作 ](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)   
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
