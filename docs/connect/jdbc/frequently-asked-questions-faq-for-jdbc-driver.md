---
title: JDBC 驱动程序常见问题解答 (FAQ)
description: 本页是关于 Microsoft JDBC Driver for SQL Server 的常见问题解答。
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cbc0e397-ecf2-4494-87b2-a492609bceae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 71c5e88db1bbc0c1b8d6698930c503a1134bac7d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837097"
---
# <a name="frequently-asked-questions-faq-for-jdbc-driver"></a>JDBC 驱动程序常见问题解答 (FAQ)

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

本页是关于 Microsoft JDBC Driver for SQL Server 的常见问题解答。

## <a name="frequently-asked-questions"></a>常见问题

**如何帮助改进 JDBC 驱动程序？**  
JDBC 驱动程序属于开放源代码，可以在 [GitHub](https://github.com/microsoft/mssql-jdbc) 上找到源代码。 可以通过归档问题和参与基本代码来帮助改进驱动程序。

**驱动程序支持哪些版本的 SQL Server 和 Java？**  
有关详细信息，请参阅 [Microsoft JDBC Driver for SQL Server 支持矩阵](../../connect/jdbc/microsoft-jdbc-driver-for-sql-server-support-matrix.md)页。

[Microsoft 下载页](download-microsoft-jdbc-driver-for-sql-server.md)提供的 JDBC 驱动程序包和 GitHub 提供的 JDBC 驱动程序之间有什么区别？   
Microsoft JDBC Driver 的 GitHub 存储库提供的 JDBC 驱动程序文件是 JDBC 驱动程序的核心，并获得存储库中列出的开放源代码许可证的许可。 [Microsoft 下载页](download-microsoft-jdbc-driver-for-sql-server.md)中的驱动程序包包括用于进行 Windows 集成身份验证和通过 JDBC 驱动程序启用 XA 事务的其他库。 这些其他库获得可下载包随附的许可证的许可。

**升级驱动程序时，我应该知道什么？**  
Microsoft JDBC Driver 9.2 支持 JDBC 4.2 和 4.3（部分）规范，并且安装包中包含以下三个 JAR 类库：

| JAR                        | JDBC 规范            | 添加版本 |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-9.2.1.jre15.jar | JDBC 4.3（部分）和 4.2 | JDK 15.0    |
| mssql-jdbc-9.2.1.jre11.jar | JDBC 4.3（部分）和 4.2 | JDK 11.0    |
| mssql-jdbc-9.2.1.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 8.4 支持 JDBC 4.2 和 4.3（部分）规范，并且安装包中包含以下三个 JAR 类库：

| JAR                        | JDBC 规范            | 添加版本 |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-8.4.1.jre14.jar | JDBC 4.3（部分）和 4.2 | JDK 14.0    |
| mssql-jdbc-8.4.1.jre11.jar | JDBC 4.3（部分）和 4.2 | JDK 11.0    |
| mssql-jdbc-8.4.1.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 8.2 支持 JDBC 4.2 和 4.3（部分）规范，并且安装包中包含以下三个 JAR 类库：

| JAR                        | JDBC 规范            | 添加版本 |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-8.2.2.jre13.jar | JDBC 4.3（部分）和 4.2 | JDK 13.0    |
| mssql-jdbc-8.2.2.jre11.jar | JDBC 4.3（部分）和 4.2 | JDK 11.0    |
| mssql-jdbc-8.2.2.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 7.4 支持 JDBC 4.2 和 4.3（部分）规范，并且其安装包中包含以下三个 JAR 类库：

| JAR                        | JDBC 规范            | 添加版本 |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-7.4.1.jre12.jar | JDBC 4.3（部分）和 4.2 | JDK 12.0    |
| mssql-jdbc-7.4.1.jre11.jar | JDBC 4.3（部分）和 4.2 | JDK 11.0    |
| mssql-jdbc-7.4.1.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 7.2 支持 JDBC 4.2 和 4.3（部分）规范，并且其安装包中包含以下两个 JAR 类库：

| JAR                        | JDBC 规范            | 添加版本 |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-7.2.2.jre11.jar | JDBC 4.3（部分）和 4.2 | JDK 11.0    |
| mssql-jdbc-7.2.2.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 7.0 支持 JDBC 4.2 和 4.3（部分）规范，并且其安装包中包含以下两个 JAR 类库：

| JAR                        | JDBC 规范            | 添加版本 |
| -------------------------- | ----------------------------- | ----------- |
| mssql-jdbc-7.0.0.jre10.jar | JDBC 4.3（部分）和 4.2 | JDK 10.0    |
| mssql-jdbc-7.0.0.jre8.jar  | JDBC 4.2                      | JDK 8.0     |
| &nbsp;                     | &nbsp;                        | &nbsp;      |

Microsoft JDBC Driver 6.4 支持 JDBC 4.1、4.2 和 4.3（部分）规范，并且其安装包中包含以下三个 JAR 类库：

| JAR                       | JDBC 规范                 | 添加版本 |
| ------------------------- | ---------------------------------- | ----------- |
| mssql-jdbc-6.4.0.jre9.jar | JDBC 4.3（部分）、4.2 和 4.1 | JDK 9.0     |
| mssql-jdbc-6.4.0.jre8.jar | JDBC 4.2 和 4.1                  | JDK 8.0     |
| mssql-jdbc-6.4.0.jre7.jar | JDBC 4.1                           | JDK 7.0     |
| &nbsp;                    | &nbsp;                             | &nbsp;      |

Microsoft JDBC Driver 6.2 支持 JDBC 4.0、4.1 和 4.2 规范，并且其安装包中包含以下两个 JAR 类库：

| JAR                       | JDBC 规范     | 添加版本 |
| ------------------------- | ---------------------- | ----------- |
| mssql-jdbc-6.2.2.jre8.jar | JDBC 4.2、4.1 和 4.0 | JDK 8.0     |
| mssql-jdbc-6.2.2.jre7.jar | JDBC 4.1 和 4.0       | JDK 7.0     |
| &nbsp;                    | &nbsp;                 | &nbsp;      |

Microsoft JDBC Driver 6.0 for SQL Server 和 Microsoft JDBC Driver 4.2 for SQL Server 支持 JDBC 4.0、4.1 和 4.2 规范，并且其安装包中包含以下两个 JAR 类库：

| JAR           | JDBC 规范     | 添加版本 |
| ------------- | ---------------------- | ----------- |
| sqljdbc42.jar | JDBC 4.2、4.1 和 4.0 | JDK 8.0     |
| sqljdbc41.jar | JDBC 4.1 和 4.0       | JDK 7.0     |
| &nbsp;        | &nbsp;                 | &nbsp;      |

Microsoft JDBC Driver 4.1 for SQL Server 支持 JDBC 4.0 规范，并且其安装包中包含以下一个 JAR 类库：

| JAR           | JDBC 规范 | 添加版本     |
| ------------- | ------------------ | --------------- |
| sqljdbc41.jar | JDBC 4.0           | JDK 7.0 和 6.0 |
| &nbsp;        | &nbsp;             | &nbsp;      |

**是否必须在应用程序中更改任何代码，才能结合使用最新驱动程序和我的现有版本 SQL Server？**  
一般而言，驱动程序都具有向后兼容性，因此升级驱动程序时，无需更改现有应用程序。 如果新的驱动程序版本引入了重大更改，请参阅 [JDBC 驱动程序的发行说明](../../connect/jdbc/release-notes-for-the-jdbc-driver.md)部分，了解有关更改的详细信息以及对现有应用程序的影响。 此外，还可以查看该驱动程序附带的发行说明，了解该版本中已修复的 bug 列表和已知问题。

**驱动程序的费用是多少？**  
Microsoft SQL Server JDBC 驱动程序是免费提供的，不需要额外付费。

**我能否再分发驱动程序？**  
JDBC 驱动程序 6.0、6.2、6.4 和 7.0 是可再发行的。 查看许可协议中的“可分发代码”子句。

**我能否使用驱动程序从 Linux 计算机访问 Microsoft SQL Server？**  
能！ 可以使用该驱动程序从 Linux、Unix 及其他非 Windows 平台访问 SQL Server。 有关详细信息，请参阅 [Microsoft JDBC Driver for SQL Server 支持矩阵](../../connect/jdbc/microsoft-jdbc-driver-for-sql-server-support-matrix.md)。

**驱动程序是否支持 TLS 加密？**  
自 1.2 版起，驱动程序支持传输层安全性 (TLS)（以前称为安全套接字层 (SSL)）加密。 有关详细信息，请参阅[使用加密](../../connect/jdbc/using-ssl-encryption.md)。

**Microsoft JDBC Driver for SQL Server 支持哪些类型的身份验证？**  
下表列出了可用的身份验证选项。 自 4.0 版起，驱动程序支持纯 Java Kerberos 身份验证。

| 平台    | 身份验证                        |
| ----------- | ------------------------------------- |
| 非 Windows | 纯 Java Kerberos                    |
| 非 Windows | SQL Server                            |
| 非 Windows | Azure Active Directory 身份验证 |
| 非 Windows | NTLM                                  |
| Windows     | 纯 Java Kerberos                    |
| Windows     | SQL Server                            |
| Windows     | 具有 NTLM 备份的 Kerberos             |
| Windows     | NTLM                                  |
| Windows     | Azure Active Directory 身份验证 |
| &nbsp;      | &nbsp;                                |

**驱动程序是否支持 Internet 协议版本 6 (IPv6) 地址？**  
是的。 驱动程序支持使用 IPv6 地址。 使用连接属性集合和 serverName 连接字符串属性。 有关详细信息，请参阅[生成连接 URL](../../connect/jdbc/building-the-connection-url.md)。

**什么是自适应缓冲？**  
自 Microsoft SQL Server 2005 JDBC Driver 1.2 版起，引入了自适应缓冲。 它旨在检索任何种类的大值数据，免去了服务器游标开销。 Microsoft SQL Server JDBC 驱动程序的自适应缓冲功能提供连接字符串属性 responseBuffering，该属性可以设置为“adaptive”或“full”。 在 1.2 版中，缓冲模式默认为“full”，应用程序必须显式设置自适应缓冲模式。 从 JDBC 驱动程序 2.0 版起，该驱动程序的默认行为就是“adaptive”。 因此，应用程序无需显式发出自适应行为请求，即可获取自适应缓冲行为。 有关详细信息，请参阅[使用自适应缓冲](../../connect/jdbc/using-adaptive-buffering.md)和[什么是自适应响应缓冲以及我为何应使用它？](/archive/blogs/jdbcteam/)博客文章。

**驱动程序是否支持连接池？**  
该驱动程序支持 Java 平台 Enterprise Edition 5 (Java EE 5) 连接池。 该驱动程序实现了 JDBC 3.0 所需的接口，从而参与到任何中间件应用程序供应商提供的任何连接池实现中。 该驱动程序将参与这些环境中的已池化连接。 有关详细信息，请参阅[使用连接池](../../connect/jdbc/using-connection-pooling.md)。 该驱动程序不提供自己的池实现，而是依赖第三方的 Java 应用程序服务器。

**能否获取驱动程序支持？**  
该驱动程序提供几种支持选项。 可以将疑问或问题发布到由 Microsoft 监视的 [GitHub 存储库](https://github.com/microsoft/mssql-jdbc)。 [论坛](https://go.microsoft.com/fwlink/?LinkID=246673)由 Microsoft、MVP 和社区监视。 还可以联系 Microsoft 客户支持服务部门。 开发团队可能会要求你在任何第三方应用程序服务器外重现问题。 如果无法在托管 Java 容器环境外重现问题，你需要联系相关第三方，这样团队才能继续为你提供帮助。 团队可能还会要求你在 Windows 等操作系统上重现问题，以便该问题获得最佳支持。

**驱动程序是否已经过认证，可用于任何第三方应用程序服务器？**  
已针对各种应用程序服务器（包括 IBM WebSphere 和 SAP NetWeaver）对该驱动程序进行了测试。

**我如何启用跟踪？**  
该驱动程序支持使用跟踪（或日志记录）来帮助解决在应用程序中使用 JDBC 驱动程序时遇到的问题。 为了启用客户端 JAR 跟踪的使用，JDBC 驱动程序将在 java.util.logging 中使用日志记录 API。 有关详细信息，请参阅[跟踪驱动程序操作](../../connect/jdbc/tracing-driver-operation.md)。 对于客户端 XA 跟踪，请参阅 [Data Access Tracing in SQL Server（SQL Server 中的数据访问跟踪）](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10))。

**我在哪里可以下载旧版驱动程序，如 SQL Server 2000 JDBC Driver、2005 Driver、1.0、1.1 或 1.2 Driver？**  
这些驱动程序版本已不再受到支持，因此不能下载。 我们在不断改善 Java 连接支持。 因此，强烈建议使用最新版 Microsoft JDBC Driver。

**我使用的是 JRE 1.4。哪个驱动程序与 JRE 1.4 兼容？**  
对于使用 SAP 产品且需要 JRE 1.4 支持的客户，可以联系 [SAP Service Marketplace](https://service.sap.com/) 获取 Microsoft JDBC 1.2 驱动程序。

**驱动程序能否使用 FIPS 验证算法进行通信？**  
Microsoft JDBC 驱动程序不包含任何加密算法。 如果客户使用美国联邦信息处理标准 (FIPS) 认为可接受的操作系统、应用程序和 JVM 算法，并将驱动程序配置为使用这些算法，那么驱动程序仅使用指定的算法进行通信。

## <a name="see-also"></a>另请参阅

[JDBC 驱动程序概述](../../connect/jdbc/overview-of-the-jdbc-driver.md)