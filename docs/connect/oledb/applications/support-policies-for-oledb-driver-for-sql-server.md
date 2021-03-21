---
title: 适用于 SQL Server 的 OLE DB 驱动程序的支持策略
description: 了解用于 OLE DB Driver for SQL Server 的支持策略，以及可以与每个驱动程序版本配合使用的操作系统和 SQL 数据库版本。
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.custom: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 97fc26029a7b74cc99b446ab42b96c50e022fed0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751227"
---
# <a name="support-policies-for-ole-db-driver-for-sql-server"></a>适用于 SQL Server 的 OLE DB 驱动程序的支持策略
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

本文介绍如何将各种数据访问组件与 OLE DB Driver for SQL Server 一起使用。  

## <a name="sql-version-support"></a>SQL 版本支持  

OLE DB Driver for SQL Server 已通过测试，并支持与以下版本的 SQL Server 建立连接。

| 数据库版本&nbsp;&#8594;<br />&#8595; 驱动程序版本 | Azure SQL Database | Azure Synapse Analytics | Azure SQL 托管实例 | SQL Server 2019 | SQL Server 2017 | SQL Server 2016 | SQL Server 2014 | SQL Server 2012 |
|----|---|---|---|---|---|---|---|---|
|18.5|是|是|是|是|是|是|是|是|
|18.4|是|是|是|是|是|是|是|是|
|18.3|是|是|是|是|是|是|是|是|
|18.2|是|是|是|是|是|是|是|是|
|18.1|是|是|是|   |是|是|是|是|
|18.0|是|是|是|   |是|是|是|是|
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

## <a name="supported-operating-system-versions"></a>支持的操作系统版本  

下表列出了 OLE DB Driver for SQL Server 支持的操作系统。  

| 操作系统&nbsp;&#8594;<br />&#8595; 驱动程序版本 | Windows Server 2019 | Windows Server 2016 | Windows Server 2012<sup>1</sup> | Windows Server 2012 R2<sup>2</sup> | Windows 10 | Windows 8.1<sup>3</sup> |
|----|---|---|---|---|---|---|
|18.5|是|是|是|是|是|是|
|18.4|是|是|是|是|是|是|
|18.3|是|是|是|是|是|是|
|18.2|是|是|是|是|是|是|
|18.1|   |是|是|是|是|是|
|18.0|   |是|是|是|是|是|
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup> 在带有 [KB2999226](https://go.microsoft.com/fwlink/?linkid=2074061) 的 Windows Server 2012 上受支持。  
<sup>2</sup> 在带有 [2014 年 4 月更新](https://go.microsoft.com/fwlink/?linkid=2073785)和 [KB2999226](https://go.microsoft.com/fwlink/?linkid=2074061) 的 Windows Server 2012 R2 上受支持。  
<sup>3</sup> 在带有 [2014 年 4 月更新](https://go.microsoft.com/fwlink/?linkid=2073785)和 [KB2999226](https://go.microsoft.com/fwlink/?linkid=2074061) 的 Windows 8.1 上受支持。  

## <a name="ado-support-policies"></a>ADO 支持策略  

如果 ADO 应用程序不需要 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 或更高版本的任何功能，可以使用 Windows 附带的 SQLOLEDB OLE DB 提供程序。  

ADO 应用程序可以使用 OLE DB Driver for SQL Server，但如果应用程序这样做，则必须在连接字符串中指定 `DataTypeCompatibility=80`。 如果连接字符串中包含 `DataTypeCompatibility=80`，则只能使用 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 的功能。  

## <a name="ole-db-support-policies"></a>OLE DB 支持策略  

应用程序可使用随 Windows 操作系统附带的 OLE DB 提供程序 (SQLOLEDB)。 但是，这处于维护模式下，且不再更新。 改为使用 OLE DB Driver for SQL Server (MSOLEDBSQL)。

## <a name="see-also"></a>另请参阅  

[使用适用于 SQL Server 的 OLE DB 驱动程序生成应用程序](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)
