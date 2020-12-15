---
title: Microsoft SQL Server 的驱动程序历史记录 | Microsoft Docs
description: 本页面介绍 Microsoft 用于连接到 SQL Server 的历史数据连接技术。
ms.custom: ''
ms.date: 12/08/2020
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dee1514230f3e0ce0f0ba4c0d3af904cc90c9720
ms.sourcegitcommit: d983ad60779d90bb1c89a34d7b3d6da18447fdd8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/09/2020
ms.locfileid: "96933806"
---
# <a name="driver-history-for-microsoft-sql-server"></a>Microsoft SQL Server 的驱动程序历史记录

本页面介绍 Microsoft 用于连接到 SQL Server 的历史数据连接技术。

## <a name="odbc"></a>ODBC

有三代不同的 Microsoft ODBC Driver for SQL Server。 第一代“SQL Server”ODBC 驱动程序仍作为 [Windows 数据访问组件](#microsoft-or-windows-data-access-components)的一部分提供。 对于新开发，不建议使用此驱动程序。 从 SQL Server 2005 开始，[SQL Server Native Client](#sql-server-native-client) 包含一个 ODBC 接口，并且它是 SQL Server 2005 至 SQL Server 2012 中随附的 ODBC 驱动程序。 对于新开发，也不建议使用此驱动程序。 在 SQL Server 2012 之后，[Microsoft ODBC Driver for SQL Server](#microsoft-odbc-driver-for-sql-server) 驱动程序随最新的服务器功能进行更新。

### <a name="sql-server-native-client"></a>SQL Server Native Client

SQL Server Native Client 是一个用于 OLE DB 和 ODBC 的独立库。 SQL Server 2005 至 SQL Server 2012 中已包含 SQL Server Native Client（通常缩写为 SNAC）。 SQL Server Native Client 可用于需要利用 SQL Server 2005 至 SQL Server 2012 中引入的新功能的应用程序。 （对于 SQL Server 中的这些新功能，不会更新 Microsoft/Windows 数据访问组件。）对于 SQL Server 2012 之后的新功能，将不会更新 SQL Server Native Client。 如果要继续使用新的 SQL Server 功能，请切换到 Microsoft ODBC Driver for SQL Server 或 Microsoft OLE DB Driver for SQL Server。

有关 SQL Server Native Client 的完整文档，请参阅 [SQL Server Native Client 文档](../relational-databases/native-client/sql-server-native-client-programming.md)。

### <a name="microsoft-odbc-driver-for-sql-server"></a>Microsoft ODBC Driver for SQL Server

在 SQL Server 2012 之后，已经开发了主要的 ODBC Driver for SQL Server，并作为 Microsoft ODBC Driver for SQL Server 发布。 有关详细信息，请参阅 [Microsoft ODBC Driver for SQL Server 文档](./odbc/microsoft-odbc-driver-for-sql-server.md)。

## <a name="ole-db"></a>OLE DB

有三个不同代的 Microsoft OLE DB Provider for SQL Server。 第一代“Microsoft OLE DB Provider for SQL Server”(SQLOLEDB) 仍作为 [Windows 数据访问组件](#microsoft-or-windows-data-access-components)的一部分提供。 将不会对此提供程序更新新功能，因此不建议将该驱动程序用于新开发。 从 SQL Server 2005 开始，[SQL Server Native Client](#sql-server-native-client) 将包含一个 OLE DB 提供程序接口 (SQLNCLI)，并且它是 SQL Server 2005 至 SQL Server 2017 中随附的 OLE DB 提供程序。 它[于 2011 年宣布弃用](/archive/blogs/sqlnativeclient/microsoft-is-aligning-with-odbc-for-native-relational-data-access)，且不建议在新开发中使用此驱动程序。 在 2017 年，OLE DB 数据访问技术随后被[取消弃用，并宣布发布 2018 年新版的计划](/archive/blogs/sqlnativeclient/announcing-the-new-release-of-ole-db-driver-for-sql-server)。 新的 OLE DB 提供程序被称为“Microsoft OLE DB Driver for SQL Server”(MSOLEDBSQL)，目前对其提供维护和支持。

## <a name="adonet"></a>ADO.NET

ADO.NET 是一组类，用于定义用于访问任何类型的数据源（关系数据源和非关系数据源）的接口。 ADO.NET 是随 Microsoft .NET Framework 引入的，并将在 .NET 中继续进行改进和维护。 SqlClient 库是一个 ADO.NET 数据提供程序，它提供与 SQL Server 和 Azure SQL 数据源的连接。

### <a name="systemdatasqlclient"></a>System.Data.SqlClient

System.Data.SqlClient 包含在 .NET Framework 和 .NET Core 中。 直到 2019 年，它都会接收定期功能更新。 随着宣布[将来的 .NET Core、.NET Framework](https://devblogs.microsoft.com/dotnet/net-core-is-the-future-of-net/) 和[常规 .NET](https://devblogs.microsoft.com/dotnet/introducing-net-5/)，SqlClient 的开发需要移动到 .NET 以外的包。 System.Data.SqlClient 仍受支持，但不接收功能更新，因此不建议用于新开发。

### <a name="microsoftdatasqlclient"></a>Microsoft.Data.SqlClient

[于 2019 年引入](https://devblogs.microsoft.com/dotnet/introducing-the-new-microsoftdatasqlclient/)的 Microsoft SqlClient Data Provider for SQL Server 是一个 ADO.NET 数据提供程序，支持面向 .NET Framework、.NET Core 和 .NET Standard 的应用程序。 有关 Microsoft.Data.SqlClient 命名空间的详细信息，请参阅[用于 SQL Server 的 Microsoft ADO.NET](ado-net/microsoft-ado-net-sql-server.md)。

## <a name="jdbc"></a>JDBC

### <a name="microsoft-jdbc-driver-for-sql-server"></a>Microsoft JDBC Driver for SQL Server

Microsoft JDBC Driver for SQL Server 于 2000 年引入，将继续对其进行改进和维护。 它在 2016 年已开放源代码。 有关最新信息（包括如何下载驱动程序），请参阅 [JDBC 驱动程序概述](./jdbc/overview-of-the-jdbc-driver.md)。

## <a name="php"></a>PHP

### <a name="microsoft-drivers-for-php-for-sql-server"></a>Microsoft Drivers for PHP for SQL Server

Microsoft Drivers for PHP for SQL Server 在 2009 年作为一个开源项目引入，将继续对其进行改进和维护。 有关最新信息（包括如何下载 PHP 驱动程序），请参阅 [Microsoft Drivers for PHP for SQL Server](./php/microsoft-php-driver-for-sql-server.md)。

## <a name="nodejs"></a>Node.js

### <a name="microsoft-driver-for-nodejs-for-sql-server"></a>适用于 SQL Server 的 Microsoft Node.JS 驱动程序

Microsoft Driver for Node.js for SQL Server 允许 Microsoft Windows 和 Microsoft Azure 上的 Node.js 应用程序访问 Microsoft SQL Server 和 Microsoft Azure SQL 数据库。 开发工作不再专注于此驱动程序。 不建议使用 Microsoft Driver for Node.js for SQL Server 创建新应用程序。

有关 Microsoft Driver for Node.js for SQL Server 的详细信息，请参阅 [WindowsAzure / node-sqlserver](https://github.com/Azure/node-sqlserver)。

### <a name="tedious"></a>Tedious

Microsoft 目前致力于开发 Node.js 中的开源 Tedious 模块并提供支持，以便使用 JavaScript 连接到 SQL Server。 有关详细信息，请参阅 [Node.js Driver for SQL Server](./node-js/node-js-driver-for-sql-server.md)。

## <a name="microsoft-or-windows-data-access-components"></a>Microsoft 或 Windows 数据访问组件

Microsoft/Windows 数据访问组件 (MDAC/WDAC) 随 Windows 一起提供并受其支持，以实现应用程序向后兼容性，它们不属于当前 SQL Server 技术堆栈。 不会向 MDAC/WDAC 中的组件添加任何新功能，不建议将它们用于新的应用程序开发。

就本文档而言，可以根据技术和产品将 MDAC/WDAC 堆栈划分为以下组件：

* **ADO**（包括 ADOMD 和 ADOX）
* **OLE DB**（包括 OLE DB 核心服务、SQL Server OLE DB 提供程序、Oracle OLE DB 提供程序、ODBC 驱动程序的 OLE DB 提供程序、数据形状提供程序以及远程数据提供程序）
* **ODBC**（包括 ODBC 驱动程序管理器、SQL ODBC 驱动程序和 Oracle ODBC 驱动程序）

### <a name="mdacwdac-components"></a>MDAC/WDAC 组件

MDAC/WDAC 包含以下组件：

* **ODBC：** Microsoft 开放式数据库连接 (ODBC) 接口是一种 C 编程语言接口，它允许应用程序访问来自不同类型的数据库管理系统 (DBMS) 的数据。 使用此 API 的应用程序只能访问关系数据源。
* **OLE DB：** OLE DB 是一组 COM 接口，用于访问不同类型的数据存储中的数据。 OLE DB 提供程序用于访问数据库、文件系统、消息存储、目录服务、工作流和文档存储中的数据。
* **ADO：** ActiveX 数据对象 (ADO) 提供了一个高级编程模型。 尽管与直接编码到 OLE DB 或 ODBC 相比，ADO 的性能稍差，但它非常易于学习和使用。 可以从脚本语言（如 Microsoft Visual Basic Scripting Edition (VBScript) 或 Microsoft JScript）中使用它。
* **ADOMD：** ADO Multi-Dimensional (ADOMD) 将与多维数据提供程序（如 Microsoft OLAP 提供程序，也称为 Microsoft Analysis Services 提供程序）一起使用。 自 MDAC 2.0 开始，未对其主要功能进行增强。
* **ADOX：** 使用 ADO Extensions for DDL and Security (ADOX)，可以创建和修改数据库、表、索引或存储过程的定义。 可以将 ADOX 与任何提供程序一起使用。 Microsoft Jet OLE DB Provider 完全支持 ADOX，而 Microsoft SQL Server OLE DB Provider 则提供有限的支持。
* **Microsoft SQL Server 网络库：** SQL Server 网络库允许 SQLOLEDB 和 SQLODBC 与 SQL Server 数据库通信。 以下 SQL Server 网络库在 MDAC/WDAC 版本中已弃用：Banyan Vines、AppleTalk、ServerNet、IPX/SPX、Giganet 和 RPC。 TCP/IP 和命名管道将继续受支持，并且在 64 位 Windows 操作系统上可用。
* **MSDASQL：** Microsoft OLE DB Provider for ODBC (MSDASQL) 允许基于 OLE DB 和 ADO（在内部使用 OLEDB）生成的应用程序通过 ODBC 驱动程序访问数据源。 MSDASQL 是一个 OLEDB 提供程序，它连接到 ODBC 而不是数据库。 在没有直接的 OLE DB 提供程序用于数据源时，它将作为 OLE DB 与 ODBC 驱动程序之间的桥梁。 MSDASQL 随 Windows 操作系统一起提供，Windows Server 2008 和 Vista SP1 是首批包含该技术的 64 位版本的 Windows 版本。

### <a name="deprecated-mdacwdac-components"></a>已弃用的 MDAC/WDAC 组件

MDAC/WDAC 的当前版本仍支持这些组件，但在将来的版本中可能会将其删除。 在开发新应用程序时，Microsoft 建议避免使用这些组件。 此外，在升级或修改现有应用程序时，请删除这些组件的任何依赖项。

* **SQLOLEDB：** 已弃用 Microsoft OLE DB Provider for SQL Server (SQLOLEDB)，它支持对 Microsoft SQL Server 的访问。 可能不支持它与将来版本的 SQL Server 建立连接。 在 Windows 7 之后，将从操作系统中删除连接到 SQL Server 7 之前版本的功能。 新应用程序应使用 Microsoft OLE DB Driver for SQL Server (MSOLEDBSQL)，该驱动程序支持新的 SQL Server 功能。 现有应用程序也应迁移到 Microsoft OLE DB Driver for SQL Server，以提高性能、可靠性和可支持性。 有关详细信息，请参阅[将应用程序从 MDAC 更新到 OLE DB Driver for SQL Server](oledb/applications/updating-an-application-to-oledb-driver-for-sql-server-from-mdac.md)。
* **SQLODBC：** 已弃用 Microsoft SQL Server ODBC Driver (SQLODBC)，它支持对 Microsoft SQL Server 的访问。 可能不支持它与将来版本的 SQL Server 建立连接。 在 Windows 7 之后，将从操作系统中删除连接到 SQL Server 7 之前版本的功能。 新应用程序应使用 Windows 上的 Microsoft ODBC Driver for SQL Server，它支持新的 SQL Server 功能。 现有应用程序也应迁移到 Microsoft ODBC Driver for SQL Server，以提高性能、可靠性和可支持性。 若要了解相关信息，请参阅[将应用程序从 MDAC 更新到 SQL Server Native Client](../relational-databases/native-client/applications/updating-an-application-to-sql-server-native-client-from-mdac.md)。
* **Microsoft Jet 数据库引擎 4.0：** 从版本 2.6 开始，MDAC 不再包含 Jet 组件。 换句话说，MDAC 2.6、2.7 和 2.8 不包含 Microsoft Jet、Microsoft Jet OLE DB 提供程序、ODBC 桌面数据库驱动程序或 Jet 数据访问对象 (DAO)。 

  不提供 64 位版本的 Jet 数据库引擎、Jet OLEDB 驱动程序、Jet ODBC 驱动程序或 Jet DAO。 有关详细信息，请参阅[知识库文章 957570](https://support.microsoft.com/kb/957570)。 在 64 位版本的 Windows 上，32 位 Jet 在 Windows WOW64 子系统下运行。 有关 WOW64 的详细信息，请参阅 [MSDN WOW64 文档](/windows/desktop/WinProg64/wow64-implementation-details)。 本机 64 位应用程序不能与在 WOW64 中运行的 32 位 Jet 驱动程序通信。

  在开发需要关系数据存储的新的非 Microsoft Access 应用程序时，Microsoft 建议使用 [Microsoft SQL Server Express Edition](https://www.microsoft.com/sql-server/sql-server-editions-express) 来替代 Microsoft Jet。 这些新的或转换后的 Jet 应用程序可以继续使用 Jet，目的是将 Microsoft Office 2003 文件及更低版本的文件（.mdb 和 .xls）用于非主要数据存储。 但是，对于这些应用程序，应计划从 Jet 迁移到 Microsoft Access 数据库引擎。 可以[下载 Microsoft Access 数据库引擎](https://www.microsoft.com/download/details.aspx?id=54920)，它可便于对已有 Office 2003（.mdb 和.xls）或 Office 2007（*.accdb、*.xlsm、*.xlsx 和 *.xlsb）文件执行读取和写入操作。

  > [!IMPORTANT]
  > 请阅读“2007 Office System 最终用户许可协议”，了解具体的使用限制。

  > [!NOTE]
  > 通过 2007 Office System 驱动程序，SQL Server 应用程序还可以访问来自 SQL Server 异类数据连接和 Integration Services 功能的 2007 Office System 以及更早版本的文件。 另外，通过在 64 位 Windows 上使用 32 位 SQL Server Integration Services (SSIS)，64 位 SQL Server 应用程序可以访问 32 位 Jet 和 2007 Office System 文件。

* **Microsoft OLE DB Provider for Data Shaping (MSDADS)：** 使用 MSDADS，可以在应用程序中的键、字段或行集之间创建层次结构关系。 自 MDAC 2.1 开始，未对主要功能进行增强。 已弃用此提供程序。 Microsoft 建议使用 XML 替代 MSDADS。
* **Oracle ODBC 和 Oracle OLE DB：** Microsoft Oracle ODBC 驱动程序 (Oracle ODBC) 和 Oracle 的 Microsoft OLE DB 提供程序 (Oracle OLE DB) 提供对 Oracle 数据库服务器的访问。 它们是使用 Oracle Call Interface (OCI) 版本 7 生成的，并为 Oracle 7 提供完全支持。 此外，它还使用 Oracle 7 仿真为 Oracle 8 数据库提供有限的支持。 Oracle 不再支持使用 OCI 版本 7 调用的应用程序。 这些技术已弃用。 如果你使用的是 Oracle 数据源，应迁移到 Oracle 提供的驱动程序和提供程序。
* **远程数据服务 (RDS)：** RDS 是一种 Microsoft 专有机制，用于通过 Internet 或 Intranet 访问远程 ADO 记录集对象。 RDS 已弃用；自 MDAC 2.1 开始，未对 RDS 的主要功能进行增强。 Microsoft 已发布 .NET Framework，它具有广泛的 SOAP 功能并替换了 RDS 组件。 在 Windows 7 之后，所有 RDS 服务器组件都将从操作系统中删除。
* **Jet 复制对象 (JRO)：** JRO 已弃用。 JRO 在 ADO 中与 Jet ( *.mdb) 数据库结合使用，创建和压缩 Jet 数据库 (.mdb) 并执行 Jet 复制管理。MDAC 2.7 将是它的最后一个版本。JRO 在 64 位 Windows 操作系统上不可用。Microsoft Access 2007 文件格式 (* .accdb) 不支持 JRO。
* **16 位 ODBC 支持：** 如果你使用的是 16 位应用程序，应迁移到 32 位应用程序。 16 位功能已弃用，将从 64 位操作系统中删除。 有关详细信息，请参阅[知识库文章 896458](https://support.microsoft.com/kb/896458)。
* **OLEDB 简单提供程序 (MSDAOSP)：** OLEDB 简单提供程序提供了一个框架，用于基于简单数据快速生成 OLE DB 提供程序。 MSDAOSP 已弃用。
* **ODBC 游标库：** ODBC 游标库 (ODBCCR32.dll) 提供有限的客户端数据游标。 ODBC 游标库已弃用；应用程序可以使用服务器端游标实现作为替代方法。
* **OLE DB 进程外接口远程处理：** OLEDB 接口远程处理 (msdaps.dll) 用于尝试允许 OLE DB 提供程序在进程外运行。 OLEDB 进程外接口远程处理已弃用。
* **AppleTalk 和 Banyan Vines SQL 网络库：** Banyan Vines、AppleTalk、ServerNet、IPX/SPX、Giganet 和 RPC SQL 网络库已弃用。 如果你正在使用其中任何一项技术，应修改应用程序，使其使用其他网络库，如 TCP/IP 和命名管道。

### <a name="mdacwdac-releases"></a>MDAC/WDAC 版本

下面是过去 MDAC/WDAC 版本的可支持性方案列表，从最早版本开始。

* **MDAC 1.5、MDAC 2.0 和 MDAC 2.1：** 这些版本的 MDAC 为独立版本，通过 Microsoft Windows NT 可选包、Microsoft Windows 平台 SDK 或 MDAC 网站发布。 不再支持这些 MDAC 版本。
* **MDAC 2.5：** 此版本的 MDAC 随 Windows 2000 操作系统一起提供。 MDAC 2.5 的服务包随相应的 Windows 2000 服务包一起提供。
* **MDAC 2.6：** MDAC 2.6 RTM、SP1 和 SP2 分别随 Microsoft SQL Server 2000 RTM、SP1 和 SP2 一起提供。 此外，这些 MDAC 服务包已按照 Microsoft SQL Server 2000 服务包发布计划发布到 MDAC 网站。 可以在 Windows 2000、Windows Millennium Edition、Windows NT、Windows 95 和 Windows 98 平台上安装此版本的 MDAC 及其服务包。 不再支持此版本的 MDAC。
* **MDAC 2.7：** 此版本的 MDAC 随 Microsoft Windows XP RTM 和 SP1 操作系统一起提供。 可以在 Windows 2000、Windows Millennium、Windows NT 和 Windows 98 平台上安装此版本的 MDAC 及其服务包。 只能通过操作系统或其服务包在 Windows XP 平台上安装此版本。 不再支持此版本的 MDAC。
* **MDAC 2.8：** 此版本的 MDAC 随 Windows Server 2003 和 Windows XP SP2 及更高版本一起提供。 也可以在 Windows 2000 上安装此版本的 MDAC 及其服务包。

  * 向客户发布 Windows Server 2003 的同时，也会在 MDAC 网站上发布 32 位版本的 MDAC 2.8。
  * 64 位版本的 MDAC 2.8 随 64 位版本的 Windows Server 2003 和 Windows XP 一起发布。

* **Windows 数据访问组件 (WDAC)：** 从 Windows Vista 和 Windows Server 2008 开始，MDAC 将其名称更改为 WDAC -“Windows 数据访问组件”。 WDAC 是操作系统的一部分，不能单独用于重新分发。 WDAC 的可维护性取决于操作系统的生命周期。

  32 位和 64 位版本的 WDAC 分别随 32 位和 64 位版本的 Windows 操作系统一起发布。

## <a name="obsolete-data-access-technologies"></a>过时的数据访问技术

过时的技术是指一些产品版本中未进行增强或更新的那些技术，这些技术将不包含在以后的产品版本中。 编写新应用程序时，请勿使用这些技术。 修改使用这些技术编写的现有应用程序时，请考虑将这些应用程序迁移到 ADO.NET 或其他当前技术。

以下组件被认为过时组件：

* **DB-Library：** DB-Library 是一种特定于 SQL Server 的编程模型，其中包含 C API。 自 SQL Server 6.5 起，未增强 DB-LIBRARY 的任何功能。 它的最终版本是与 SQL Server 2000 一起发布的，不会将其移植到 64 位 Windows 操作系统。
* **嵌入式 SQL (E-SQL)：** E-SQL 是一种特定于 SQL Server 的编程模型，使用它可以在 Visual C 代码中嵌入 Transact-SQL 语句。 自 SQL Server 6.5 起，未增强 E-SQL 的任何功能。 它的最终版本是与 SQL Server 2000 一起发布的，不会将其移植到 64 位 Windows 操作系统。
* **数据访问对象 (DAO)：** DAO 提供对 JET (Access) 数据库的访问。 可以从 Microsoft Visual Basic、Microsoft Visual C++ 和脚本语言中使用此 API。 它随 Microsoft Office 2000 和 Office XP 一起提供。 DAO 3.6 是此技术的最后一个版本。 它在 64 位 Windows 操作系统上不可用。
* **远程数据对象 (RDO)：** RDO 专门用于访问远程 ODBC 关系数据源，无需复杂的应用程序代码即可更轻松使用 ODBC。 它随 Microsoft Visual Basic 版本 4、5 和 6 一起提供。 RDO 版本 2.0 是此技术的最后一个版本。

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
