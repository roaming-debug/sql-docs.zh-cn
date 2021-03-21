---
title: 适用于 SQL Server 的 Microsoft OLE DB 驱动程序
description: Microsoft OLE DB Driver for SQL Server 支持通过标准 OLE DB API 连接到 SQL Server 和 Azure SQL 数据库。
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- MSOLEDBSQL, about OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server, about OLE DB Driver for SQL Server
- data access [OLE DB Driver for SQL Server], about OLE DB Driver for SQL Server
- data access [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server
- MSOLEDBSQL
- native data access [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5ed2642e679cb9822f98285c7dd13f1bdfeda6ef
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748303"
---
# <a name="microsoft-ole-db-driver-for-sql-server"></a>适用于 SQL Server 的 Microsoft OLE DB 驱动程序
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../includes/driver_oledb_download.md)]

适用于 SQL Server 的 OLE DB 驱动程序是独立的数据访问应用程序编程接口 (API)，用于 OLE DB，是在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中引入的。 适用于 SQL Server 的 OLE DB 驱动程序提供了一个动态链接库 (DLL) 中的 SQL OLE DB 驱动程序。 除 Windows 数据访问组件（Windows DAC，以前为 Microsoft 数据访问组件或 MDAC）提供的功能之外，它还提供新的功能。 适用于 SQL Server 的 OLE DB 驱动程序可用于创建新的应用程序或增强现有应用程序的性能，使其能够利用 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中引入的功能，例如多个活动结果集 (MARS)、用户定义数据类型 (UDT)、查询通知、快照隔离和 XML 数据类型支持。  
  
> [!NOTE]  
> 有关适用于 SQL Server 的 OLE DB 驱动程序与 Windows DAC 之间差异的列表，以及将 Windows DAC 应用程序更新到适用于 SQL Server 的 OLE DB 驱动程序之前要考虑问题的信息，请参阅[从 MDAC 将应用程序更新到适用于 SQL Server 的 OLE DB 驱动程序](../oledb/applications/updating-an-application-to-oledb-driver-for-sql-server-from-mdac.md)。  

> [!IMPORTANT]
> 以前的 [Microsoft OLE DB Provider for SQL Server (SQLOLEDB)](../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md) 和 [SQL Server Native Client OLEDB](../../relational-databases/native-client/sql-server-native-client.md) 提供程序 (SQLNCLI) 仍然不推荐使用，不建议在新的开发工作中使用它们。
  
 适用于 SQL Server 的 OLE DB 驱动程序可与 Windows DAC 提供的 OLE DB 核心服务一起使用，但这不是必要条件；是否选择使用核心服务取决于单个应用程序的要求（例如是否必需连接池）。  
  
 ActiveX 数据对象 (ADO) 应用程序可以使用适用于 SQL Server 的 OLE DB 驱动程序，但建议将 ADO 与 DataTypeCompatibility  连接字符串关键字（或其对应的 DataSource  属性）一起使用。 使用适用于 SQL Server 的 OLE DB 驱动程序时，ADO 应用程序可以通过连接字符串关键字、OLE DB 属性或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 利用在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中引入的通过适用于 SQL Server 的 OLE DB 驱动程序提供的那些新功能。 有关将这些功能与 ADO 一起使用的详细信息，请参阅[将 ADO 与适用于 SQL Server 的 OLE DB 驱动程序结合使用](../oledb/applications/using-ado-with-oledb-driver-for-sql-server.md)。  
  
 适用于 SQL Server 的 OLE DB 驱动程序旨在让用户更简单地使用 OLE DB 获取对 SQL Server 的本机数据访问。 它提供一种创新和开发新的数据访问功能而不更改当前 Windows DAC 组件（现在是 Microsoft Windows 平台的一部分）的方法。  
  
 尽管 OLE DB Driver for SQL Server 使用 Windows DAC 中的组件，但它并不显式依赖特定版本的 Windows DAC。 可以将适用于 SQL Server 的 OLE DB 驱动程序与随适用于 SQL Server 的 OLE DB 驱动程序支持的任一操作系统安装的 Windows DAC 版本一起使用。  

 ## <a name="different-generations-of-ole-db-drivers"></a>不同代的 OLE DB 驱动程序

有三个不同代的 Microsoft OLE DB Provider for SQL Server。

### <a name="1-microsoft-ole-db-provider-for-sql-server-sqloledb"></a>1.Microsoft OLE DB Provider for SQL Server (SQLOLEDB)
[Microsoft OLE DB Provider for SQL Server](../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md) (SQLOLEDB) 仍作为 [Windows 数据访问组件](/previous-versions/windows/desktop/ms692897(v=vs.85))的一部分提供。 不再对其进行维护，且不建议在新开发中使用此驱动程序。

### <a name="2-sql-server-native-client-snac"></a>2.SQL Server Native Client (SNAC)
从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 开始，[SQL Server Native Client (SNAC)](../../relational-databases/native-client/sql-server-native-client.md) 将包含一个 OLE DB 提供程序接口 (SQLNCLI)，并且是通过 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 一起提供的 OLE DB 提供程序。

它[于 2011 年宣布弃用](/archive/blogs/sqlnativeclient/microsoft-is-aligning-with-odbc-for-native-relational-data-access)，且不建议在新开发中使用此驱动程序。 有关 SNAC 生命周期和可用下载的详细信息，请参阅[所述的 SNAC 生命周期](/archive/blogs/sqlreleaseservices/snac-lifecycle-explained)。

### <a name="3-microsoft-ole-db-driver-for-sql-server-msoledbsql"></a>3.适用于 SQL Server 的 Microsoft OLE DB 驱动程序 (MSOLEDBSQL)
OLE DB 已[取消弃用](/archive/blogs/sqlnativeclient/announcing-the-new-release-of-ole-db-driver-for-sql-server)并于 2018 年发布。

新的 OLEDB 提供程序被称为“Microsoft OLEDB Driver for SQL Server (MSOLEDBSQL)”。 随着最新服务器功能的演进，将对新提供程序进行更新。

> [!NOTE]
> 要在现有应用程序中使用新的 Microsoft OLE DB Driver for SQL Server，应计划将连接字符串从 SQLOLEDB 或 SQLNCLI 转换为 MSOLEDBSQL。
  
## <a name="in-this-section"></a>在本节中  
[何时使用适用于 SQL Server 的 OLE DB 驱动程序](../oledb/when-to-use-oledb-driver-for-sql-server.md)  
 讨论适用于 SQL Server 的 OLE DB 驱动程序与 Microsoft 数据访问技术的适应度如何，探讨它与 Windows DAC 和 ADO.NET 相比较如何，同时提供建议，帮助用户决定要采用哪种数据访问技术。  
  
 [适用于 SQL Server 的 OLE DB 驱动程序功能](../oledb/features/oledb-driver-for-sql-server-features.md )  
 介绍了适用于 SQL Server 的 OLE DB 驱动程序所支持的功能。  
  
 [使用适用于 SQL Server 的 OLE DB 驱动程序生成应用程序](../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
 简要介绍适用于 SQL Server 的 OLE DB 驱动程序，包括它与 Windows DAC 的差异、其使用的组件，以及如何将其与 ADO 结合。  
  
 本部分还讨论了适用于 SQL Server 的 OLE DB 驱动程序的安装和部署，包括如何重新发布适用于 SQL Server 的 OLE DB 驱动程序库。  
  
 [适用于 SQL Server 的 OLE DB 驱动程序的系统要求](../oledb/system-requirements-for-oledb-driver-for-sql-server.md)  
 讨论使用适用于 SQL Server 的 OLE DB 驱动程序所需的系统资源。  
  
 [适用于 SQL Server 的 OLE DB 驱动程序编程](../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
 提供有关使用适用于 SQL Server 的 OLE DB 驱动程序的信息。  
  
 [查找有关适用于 SQL Server 的 OLE DB 驱动程序的更多信息](../oledb/finding-more-oledb-driver-for-sql-server-information.md)  
 提供有关适用于 SQL Server 的 OLE DB 驱动程序的其他资源，包括指向外部资源和获取进一步帮助的链接。  
  
  
## <a name="see-also"></a>另请参阅  
 [从 SQL Server 2005 Native Client 更新应用程序](../oledb/applications/updating-an-application-from-sql-server-2005-native-client.md)    
 [OLE DB 操作指南主题](../oledb/ole-db-how-to/ole-db-how-to-topics.md)  
  
  
