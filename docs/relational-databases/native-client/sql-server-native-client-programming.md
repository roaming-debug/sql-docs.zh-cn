---
title: 编程
description: 使用这些资源来了解 SQL Server Native Client （一种独立的数据访问 API），用于 OLE DB 和 ODBC。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLNCLI, about SQL Server Native Client
- SQL Server Native Client, about SQL Server Native Client
- data access [SQL Server Native Client], about SQL Server Native Client
- data access [SQL Server Native Client]
- SQL Server Native Client
- SQLNCLI
- native data access [SQL Server Native Client]
ms.assetid: 14ba2cb1-a424-4e4d-b224-0bf1015ab801
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 918094d0bbc5739ed00827cc239723379abe3cf1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463288"
---
# <a name="sql-server-native-client-programming"></a>SQL Server Native Client 编程
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 是在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中引入的用于 OLE DB 和 ODBC 的独立数据访问应用程序编程接口 (API)。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 将 SQL OLE DB 访问接口和 SQL ODBC 驱动程序组合成一个本机动态链接库 (DLL)。 除 Windows 数据访问组件（Windows DAC，以前为 Microsoft 数据访问组件或 MDAC）提供的功能之外，它还提供新的功能。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 可用于创建新应用程序或增强现有应用程序，使这些应用程序能够利用在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中引入的功能，例如多个活动结果集 (MARS)、用户定义数据类型 (UDT)、查询通知、快照隔离和 XML 数据类型支持。  
  
> [!NOTE]  
>  有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native client 和 WINDOWS dac 之间的差异列表，以及有关将 WINDOWS DAC 应用程序更新到 Native Client 之前要考虑的问题的信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请参阅将 [应用程序更新为从 MDAC SQL Server Native Client](../../relational-databases/native-client/applications/updating-an-application-to-sql-server-native-client-from-mdac.md)。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序始终与 Windows DAC 提供的 ODBC 驱动程序管理器一起使用。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口可与 Windows DAC 提供的 OLE DB 核心服务一起使用，但这不是必要条件；是否选择使用核心服务取决于单个应用程序的要求（例如，是否必需连接池）。  
  
  (ADO) 的 ActiveX 数据对象可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序，但建议结合使用 ADO 和 **DataTypeCompatibility** 连接字符串关键字 (或其对应的 **数据源** 属性) 。 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口时，ADO 应用程序可以通过连接字符串关键字、OLE DB 属性或 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 利用在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中引入的通过 [!INCLUDE[tsql](../../includes/tsql-md.md)] Native Client 提供的那些新功能。 有关使用这些功能和 ADO 的详细信息，请参阅 [使用 ado 和 SQL Server Native Client](../../relational-databases/native-client/applications/using-ado-with-sql-server-native-client.md)。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 旨在提供一种使用 OLE DB 或 ODBC 获取对 SQL Server 的本机数据访问的简化方法。 简化之处在于它将 OLE DB 和 ODBC 技术组合到一个库中，并且它提供一种创新和开发新的数据访问功能而不更改当前 Windows DAC 组件（现在是 Microsoft Windows 平台的一部分）的方法。  
  
 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 使用 WINDOWS dac 中的组件时，它不会显式依赖于特定版本的 WINDOWS dac。 可以将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 与随 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 支持的任一操作系统安装的 Windows DAC 版本一起使用。  
  
## <a name="in-this-section"></a>本节内容  
 [SQL Server Native Client](../../relational-databases/native-client/sql-server-native-client.md)  
 列出重大的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 新功能。  
  
 [何时使用 SQL Server Native Client](../../relational-databases/native-client/when-to-use-sql-server-native-client.md)  
 讨论 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 如何适应 Microsoft 数据访问技术，它如何与 WINDOWS DAC 和 ADO.NET 进行比较，并提供用于决定要使用的数据访问技术的指针。  
  
 [SQL Server Native Client 功能](../../relational-databases/native-client/features/sql-server-native-client-features.md)  
 介绍 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 支持的功能。  
  
 [使用 SQL Server Native Client 生成应用程序](../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
 提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 开发概述，包括它与 Windows DAC 的不同之处、它使用的组件，以及如何将 ADO 与它一起使用。  
  
 本节还讨论 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的安装和部署，包括如何再分发 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 库。  
  
 [SQL Server Native Client 的系统要求](../../relational-databases/native-client/system-requirements-for-sql-server-native-client.md)  
 讨论使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 所需的系统资源。  
  
 [SQL Server Native Client (OLE DB)](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
 提供有关使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口的信息。  
  
 [SQL Server Native Client (ODBC)](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
 提供有关使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序的信息。  
  
 [查找更多 SQL Server Native Client 信息](../../relational-databases/native-client/finding-more-sql-server-native-client-information.md)  
 提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的其他资源，包括指向外部资源和获取进一步帮助的链接。  
  
 [SQL Server Native Client 错误](./sql-server-native-client-error-mssqlserver-50000.md)  
 包含有关与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 关联的运行时错误的主题。  
  
## <a name="see-also"></a>另请参阅  
 [从 SQL Server 2005 Native Client 更新应用程序](../../relational-databases/native-client/applications/updating-an-application-from-sql-server-2005-native-client.md)   
 [ODBC 操作指南主题](../../relational-databases/native-client-odbc-how-to/odbc-how-to-topics.md)   
 [OLE DB 操作指南主题](../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)  
  
