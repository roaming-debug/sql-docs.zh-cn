---
title: 适用于 SQL Server 的 OLE DB 驱动程序的系统要求
description: 了解在 OLE DB Driver for SQL Server 中使用 SQL Server 的数据访问功能（如 MARS）所需的软件必备项。
ms.custom: ''
ms.date: 03/18/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- system requirements [OLE DB Driver for SQL Server]
- data access [OLE DB Driver for SQL Server], system requirements
- OLE DB Driver for SQL Server, system requirements
- MSOLEDBSQL, system requirements
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: aa46b5a4db292b5ac4ee5df71aa18abeb74c5559
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744867"
---
# <a name="system-requirements-for-ole-db-driver-for-sql-server"></a>适用于 SQL Server 的 OLE DB 驱动程序的系统要求

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../includes/driver_oledb_download.md)]

若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的数据访问功能（如 MARS），必须安装以下软件：  

* 在客户端上安装 OLE DB Driver for SQL Server。  
* 服务器上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。

> [!NOTE]  
> 确保以管理员权限进行登录，然后再安装此软件。  

## <a name="operating-system-requirements"></a>操作系统要求  

有关支持 OLE DB Driver for SQL Server 的操作系统的列表，请参阅 [OLE DB Driver for SQL Server 的支持策略](../oledb/applications/support-policies-for-oledb-driver-for-sql-server.md)。  

## <a name="azure-active-directory-authentication-requirements"></a>Azure Active Directory 身份验证要求  

将 Active Directory 身份验证方法与低于 OLE DB Driver for SQL Server 18.3 的版本一起使用时，请确保已安装[适用于 SQL Server 的 Active Directory 身份验证库](https://go.microsoft.com/fwlink/?LinkID=513072)。 （版本 18.3 将依赖项作为其安装程序包的一部分包含在内。）其他身份验证方法或 OLE DB 操作不需要 ADAL。 有关详细信息，请参阅：[使用 Azure Active Directory](features/using-azure-active-directory.md)。

## <a name="sql-server-requirements"></a>SQL Server 要求  

若要使用 OLE DB Driver for SQL Server 访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中的数据，必须安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  

[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 支持与 MDAC 的所有版本、Windows 数据访问组件以及适用于 SQL Server 的 OLE DB 驱动程序的所有版本的连接。 如果客户端的较旧版本连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，则该客户端未知的服务器数据类型将映射到与客户端版本相兼容的类型。 有关详细信息，请参阅[客户端版本的数据类型兼容性](#data-type-compatibility-for-client-versions)。  

## <a name="cross-language-requirements"></a>跨语言要求  

英文版的适用于 SQL Server 的 OLE DB 驱动程序可以在所有受支持的本地化操作系统上使用。 与 OLE DB Driver for SQL Server 本地化版本使用同一语言的本地化操作系统支持 OLE DB Driver for SQL Server 的本地化版本。 只要安装了匹配的语言设置，则受支持的操作系统的英文版还支持适用于 SQL Server 的 OLE DB 驱动程序的本地化版本。  

对于升级：  

* 英文版本的 OLE DB Driver for SQL Server 可以升级到 OLE DB Driver for SQL Server 的任何本地化版本。  
* OLE DB Driver for SQL Server 的本地化版本可以升级到同一语言的 OLE DB Driver for SQL Server 的本地化版本。  
* OLE DB Driver for SQL Server 的本地化版本可以升级到英文版本的 OLE DB Driver for SQL Server。  
* OLE DB Driver for SQL Server 的本地化版本无法升级到不同本地化语言的 OLE DB Driver for SQL Server 的本地化版本。  

## <a name="data-type-compatibility-for-client-versions"></a>客户端版本的数据类型兼容性  

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和适用于 SQL Server 的 OLE DB 驱动程序将新数据类型映射到与下级客户端相兼容的较旧的数据类型，如下表所示。  

OLE DB 和 ADO 应用程序可将 DataTypeCompatibility  连接字符串关键字用于 OLE DB Driver for SQL Server，以便对较早的数据类型执行操作。 如果 DataTypeCompatibility=80，OLE DB 客户端将使用 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 表格格式数据流 (TDS) 版本而不是 TDS 版本进行连接  。 也就是说，对于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和较新的数据类型，将由服务器而不是适用于 SQL Server 的 OLE DB 驱动程序执行下级转换。 此外，这还意味着，连接中的可用功能将被限制为 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 功能集。 尽可能早地对 API 调用中使用新数据类型或功能的尝试进行检测，将错误返回给调用应用程序，而不是尝试将无效的请求传递给服务器。  

IDBInfo::GetKeywords 将始终返回与连接中的服务器版本相对应的关键字列表，并且不受 DataTypeCompatibility  的影响。  

|数据类型|SQL Server Native Client<br /><br />SQL Server 2005|SQL Server Native Client 11.0<br /><br /> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|适用于 SQL Server 的 OLE DB 驱动程序|Windows 数据访问组件、MDAC 及<br /><br /> OLE DB Driver for SQL Server OLE DB 应用程序（其中 DataTypeCompatibility=80）|  
|---------------|--------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|  
|CLR UDT (\<= 8 Kb)|udt|udt|udt|Varbinary|  
|varbinary(max)|varbinary|varbinary|varbinary|映像|  
|varchar(max)|varchar|varchar|varchar|文本|  
|nvarchar(max)|nvarchar|nvarchar|nvarchar|Ntext|  
|xml|xml|xml|xml|Ntext|  
|CLR UDT (> 8 Kb)|varbinary|udt|udt|映像|  
|date|varchar|date|date|Varchar|  
|datetime2|varchar|datetime2|datetime2|Varchar|  
|datetimeoffset|varchar|datetimeoffset|datetimeoffset|Varchar|  
|time|varchar|time|time|Varchar|  

## <a name="see-also"></a>另请参阅  

[适用于 SQL Server 的 OLE DB 驱动程序](../oledb/oledb-driver-for-sql-server.md)  
[安装适用于 SQL Server 的 OLE DB 驱动程序](../oledb/applications/installing-oledb-driver-for-sql-server.md)  
