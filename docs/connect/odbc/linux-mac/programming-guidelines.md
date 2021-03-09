---
title: 编程指南（ODBC 驱动程序）
description: macOS 和 Linux 上的 Microsoft ODBC Driver for SQL Server 的编程功能基于 SQL Server Native Client (ODBC) 中的 ODBC。
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
author: v-makouz
ms.author: v-daenge
ms.openlocfilehash: 6d602db9c189b6e7fce8b767b60204253ccb7f67
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464388"
---
# <a name="programming-guidelines"></a>编程指南

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

macOS 和 Linux 上的 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的编程功能建立在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ([SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)) 的基础之上。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 建立在 Windows 数据访问组件中的 ODBC（[ODBC 程序员参考](../../../odbc/reference/odbc-programmer-s-reference.md)）的基础之上。  

通过在包含 unixODBC 标头（`sql.h`、`sqlext.h`、`sqltypes.h` 和 `sqlucode.h`）后包含 `/usr/local/include/msodbcsql.h`，ODBC 应用程序可以使用多重活动结果集 (MARS) 和其他 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 特定功能。 然后，使用与在 Windows ODBC 应用程序中将使用的相同的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 特定项符号名称。

## <a name="available-features"></a>可用功能  
在使用 macOS 和 Linux 上的 ODBC 驱动程序时，用于 ODBC ([ SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)) 的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 文档的以下各个部分均有效：  

-   [与 SQL Server 通信 (ODBC)](../../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
-   [连接和查询超时支持](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
-   [游标](../../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
-   [日期/时间的改进 (ODBC)](../../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
-   [执行查询 (ODBC)](../../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
-   [处理错误和消息](../../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
-   [Kerberos 身份验证](../../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)  
-   [大型 CLR 用户定义类型 (ODBC)](../../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)  
-   [执行事务 (ODBC)（分布式事务除外）](../../../relational-databases/native-client/odbc/performing-transactions-in-odbc.md)  
-   [处理结果 (ODBC)](../../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
-   [运行存储过程](../../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)
-   [稀疏列支持 (ODBC)](../../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md)
-   [使用不带验证的加密](../../../relational-databases/native-client/features/using-encryption-without-validation.md)
-   [表值参数](../../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)
-   [用于命令和数据 API 的 UTF-8 和 UTF-16](../../../relational-databases/native-client/features/utf-16-support-in-sql-server-native-client-11-0.md)
-   [使用目录函数](../../../relational-databases/native-client/odbc/using-catalog-functions.md)  

## <a name="unsupported-features"></a>不支持的功能

尚未验证以下功能是否能在此版本的 macOS 和 Linux 上的 ODBC 驱动程序中正常使用：

-   故障转移群集连接
-   [透明网络 IP 解析](../using-transparent-network-ip-resolution.md)（低于 ODBC Driver 17 版本）
-   [高级驱动程序跟踪](/archive/blogs/mattn/enabling-advanced-driver-tracing-for-the-sql-native-client-odbc-drivers)

以下功能在此版本的 macOS 和 Linux 上的 ODBC 驱动程序中不可用： 

-   分布式事务（不支持 SQL_ATTR_ENLIST_IN_DTC 属性）  
-   数据库镜像  
-   FILESTREAM  
-   在 [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) 中讨论了 ODBC 驱动程序性能事件分析，以及以下与性能相关的连接属性：  
    -   SQL_COPT_SS_PERF_DATA  
    -   SQL_COPT_SS_PERF_DATA_LOG  
    -   SQL_COPT_SS_PERF_DATA_LOG_NOW  
    -   SQL_COPT_SS_PERF_QUERY  
    -   SQL_COPT_SS_PERF_QUERY_INTERVAL  
    -   SQL_COPT_SS_PERF_QUERY_LOG  
-   SQLBrowseConnect（在 17.2 版以前）
-   诸如 SQL_C_INTERVAL_YEAR_TO_MONTH（记录在[数据类型标识符和描述符](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md)中）的 C 间隔类型
-   SQLSetConnectAttr 函数的 SQL_ATTR_ODBC_CURSORS 属性的 SQL_CUR_USE_ODBC 值。

## <a name="character-set-support"></a>字符集支持

对于 ODBC Driver 13 和 13.1，SQLCHAR 数据必须采用 UTF-8 编码。 不支持其他编码。

对于 ODBC Driver 17，支持以下一种字符集/编码中的 SQLCHAR 数据：

> [!NOTE]  
> 由于 `musl` 和 `glibc` 中存在 `iconv` 差异，许多区域设置在 Alpine Linux 上不受支持。
>
> 有关详细信息，请参阅[与 glibc 的功能差异](https://wiki.musl-libc.org/functional-differences-from-glibc.html)

|名称|说明|
|-|-|
|UTF-8|Unicode|
|CP437|MS-DOS 拉丁语(美国)|
|CP850|MS-DOS 拉丁语 1|
|CP874|拉丁语/泰语|
|CP932|日语 (Shift_JIS)|
|CP936|简体中文 (GBK)|
|CP949|韩语 (EUC-KR)|
|CP950|繁体中文 (Big5)|
|CP1251|西里尔语|
|CP1253|希腊语|
|CP1256|阿拉伯语|
|CP1257|波罗的语|
|CP1258|越南语|
|ISO-8859-1 / CP1252|拉丁语 - 1|
|ISO-8859-2 / CP1250|拉丁语 - 2|
|ISO-8859-3|拉丁语 - 3|
|ISO-8859-4|拉丁语 - 4|
|ISO-8859-5|拉丁语/西里尔语|
|ISO-8859-6|拉丁语/阿拉伯语|
|ISO-8859-7|拉丁语/希腊语|
|ISO-8859-8 / CP1255|希伯来语|
|ISO-8859-9 / CP1254|土耳其语|
|ISO-8859-13|拉丁语 - 7|
|ISO-8859-15|拉丁语 - 9|

连接后，驱动程序将检测加载它的进程的当前区域设置。 如果驱动程序使用上述一种编码，则驱动程序会将该编码用于 SQLCHAR（窄字符）数据；否则，默认使用 UTF-8。 默认情况下，由于所有进程都在“C”区域设置中启动（并因此导致驱动程序默认为 UTF-8），如果应用程序需要使用上述一种编码，则它应在连接前使用 setlocale 函数设置适当的区域设置：可显式指定所需的区域设置，或通过使用空字符串（例如，`setlocale(LC_ALL, "")`）来使用环境的区域设置  。

因此，在编码为 UTF-8 的典型 Linux 或 macOS 环境中，从 ODBC 驱动程序 13 或 13.1 升级的 ODBC 驱动程序 17 用户将不会察觉到任何差异。 但是，通过 `setlocale()` 使用上述列表中非 UTF-8 编码的应用程序需要对传入/传出驱动程序的数据使用该编码，而不是 UTF-8。

SQLWCHAR 数据必须是 UTF-16LE (Little Endian)。

使用 SQLBindParameter 绑定输入参数时，如果指定了 SQL_VARCHAR 等窄字符 SQL 类型，则驱动程序会将提供的数据从客户端编码转换为默认（通常为代码页 1252）[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 编码。 对于输出参数，驱动程序将从与数据关联的排序规则信息中指定的编码转换为客户端编码。 但是，数据可能会丢失 - 无法以目标编码表示的源编码字符将转换为问号（“?”）。

要在绑定输入参数时避免此类数据丢失，请指定一种 Unicode SQL 字符类型，例如 SQL_NVARCHAR。 在这种情况下，驱动程序将从客户端编码转换为 UTF-16，该编码可表示所有 Unicode 字符。 此外，服务器上的目标列或参数也必须是 Unicode 类型（nchar、nvarchar 和 ntext）或带有排序规则/编码的其他类型，这可以表示原始源数据的所有字符    。 为避免输出参数丢失数据，请指定 Unicode SQL 类型和 Unicode C 类型 (SQL_C_WCHAR)（使驱动程序以 UTF-16 形式返回数据）或窄 C 类型，并确保客户端编码可以表示源数据的所有字符（可始终使用 UTF-8 实现这一目的）。

有关排序规则和编码的详细信息，请参阅[排序规则和 Unicode 支持](../../../relational-databases/collations/collation-and-unicode-support.md)。

Windows 与 Linux 和 macOS 上的几个版本的 iconv 库之间存在一些编码转换差异。 代码页 1255（希伯来语）中的文本数据有一个码位 (0xCA) 在转换为 Unicode 时具有不同的行为。 在 Windows 中，此字符转换为 0x05BA 的 UTF-16 码位。 在具有 1.15 版本以前的 libiconv 的 macOS 和 Linux 上，它转换为 0x00CA。 在 iconv 库不支持 Big5/CP950 的 2003 修订（名为 `BIG5-2003`）的 Linux 上，使用该修订添加的字符将无法正确转换。 在代码页 932（日语(Shift-JIS)）中，最初未在编码标准中定义的字符的解码结果也存在差异。 例如，字节 0x80 在 Windows 上转换为 U+0080，但在 Linux 和 macOS 上可能会变为 U+30FB，具体取决于 iconv 版本。

在 ODBC 驱动程序 13 和 13.1 中，当在 SQLPutData 缓冲区上拆分 UTF-8 多字节字符或 UTF-16 代理项时，这会导致数据损坏。 使用用于流式传输不会在部分字符编码中结束的 SQLPutData 的缓冲区。 ODBC Driver 17 消除了这一限制。

## <a name="openssl"></a><a name="bkmk-openssl"></a>OpenSSL
从版本 17.4 开始，驱动程序将动态加载 OpenSSL，从而使其可以在具有版本 1.0 或 1.1 的系统上运行，而不需要单独的驱动程序文件。 当存在多个版本的 OpenSSL 时，驱动程序将尝试加载最新版本。 该驱动程序当前支持 OpenSSL 1.0.x 和 1.1.x

> [!NOTE]  
> 如果链接到使用驱动程序（或其组件之一）的应用程序或动态加载不同版本的 OpenSSL，则可能会发生潜在冲突。 如果系统上存在多个版本的 OpenSSL，并且应用程序使用 OpenSSL，则强烈建议你格外谨慎，以确保应用程序和驱动程序加载的版本不匹配，因为这些错误可能会破坏内存，从而损坏内存，因此，不一定会以明显或一致的方式表现出来。

## <a name="alpine-linux"></a><a name="bkmk-alpine"></a>Alpine Linux
在撰写本文时，MUSL 中的默认堆栈大小为 128K，足以满足基本的 ODBC 驱动程序功能，但是根据应用程序执行的操作，超过此限制并不困难，尤其是从多个线程调用驱动程序的情况。 建议使用 `-Wl,-z,stack-size=<VALUE IN BYTES>` 编译 Alpine Linux 上的 ODBC 应用程序，以增加堆栈大小。 作为参考，大多数 GLIBC 系统上的默认堆栈大小为 2MB。

## <a name="additional-notes"></a>其他说明  

1.  可以使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证和 主机,端口建立专用管理员连接 (DAC)  。 首先，Sysadmin 角色成员需要发现 DAC 端口。 请参阅[用于数据库管理员的诊断连接](../../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md#dac-port)来了解如何操作。 例如，如果 DAC 端口为 33000，可以使用 `sqlcmd` 连接它，如下所示：  

    ```
    sqlcmd -U <user> -P <pwd> -S <host>,33000
    ```

    > [!NOTE]  
    > DAC 连接必须使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证。  
    
2.  当所有语句属性均通过 SQLSetConnectAttr 传递时，UnixODBC 驱动程序管理器会为其返回“属性/选项标识符无效”。 在 Windows 上，当 SQLSetConnectAttr 接收某个语句属性值时，它会使驱动程序在属于连接句柄子级的所有活动语句上设置该值。  

3.  当驱动程序与高度多线程的应用程序一起使用时，unixODBC 的句柄验证可能会成为性能瓶颈。 在这种情况下，通过使用 `--enable-fastvalidate` 选项编译 unixODBC 可以显着提高性能。 但是，请注意，这可能导致将无效句柄传递给 ODBC API 的应用程序崩溃，而不是返回 `SQL_INVALID_HANDLE` 错误。

## <a name="see-also"></a>另请参阅  
[常见问题解答](frequently-asked-questions-faq-for-odbc-linux.yml)

[此版本驱动程序中的已知问题](known-issues-in-this-version-of-the-driver.md)

[发行说明](release-notes-odbc-sql-server-linux-mac.md)
