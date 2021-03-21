---
title: 会话属性 - 适用于 SQL Server 的 OLE DB 驱动程序 | Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 如何解释 OLE DB 会话属性，包括特定于提供程序的属性集。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- sessions [OLE DB]
- OLE DB Driver for SQL Server, sessions
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 326b0d09fbcf071d981db56440956393eda93e73
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755627"
---
# <a name="session-properties---ole-db-driver-for-sql-server"></a>会话属性 - 适用于 SQL Server 的 OLE DB 驱动程序
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server 按如下所示解释 OLE DB 会话属性。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|DBPROP_SESS_AUTOCOMMITISOLEVELS|适用于 SQL Server 的 OLE DB 驱动程序支持所有自动提交的事务隔离级别，混沌级别 DBPROPVAL_TI_CHAOS 除外。|  
  
 在特定于访问接口的属性集 DBPROPSET_SQLSERVERSESSION 中，适用于 SQL Server 的 OLE DB 驱动程序定义以下附加的会话属性。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|SSPROP_QUOTEDCATALOGNAMES|类型：VT_BOOL<br /><br /> R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE<br /><br /> 说明：在 CATALOG 限制中允许带引号的标识符。<br /><br /> VARIANT_TRUE：对提供分布式查询支持的架构行集的目录限制识别带引号的标识符。<br /><br /> VARIANT_FALSE：对提供分布式查询支持的架构行集的目录限制不识别带引号的标识符。<br /><br /> 有关提供分布式查询支持的架构行集的详细信息，请参阅[架构行集中的分布式查询支持](../../oledb/ole-db/schema-rowsets-distributed-query-support.md)。|  
|SSPROP_ALLOWNATIVEVARIANT|键入：VT_BOOL<br /><br /> R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE<br /><br /> 说明:确定提取的数据是作为 DBTYPE_VARIANT 还是作为 DBTYPE_SQLVARIANT。<br /><br /> VARIANT_TRUE：列类型作为 DBTYPE_SQLVARIANT 返回（这种情况下，缓冲区会保留 SSVARIANT 结构）。<br /><br /> VARIANT_FALSE：列类型作为 DBTYPE_VARIANT 返回，且缓冲区将具有 VARIANT 结构。|  
|SSPROP_ASYNCH_BULKCOPY|若要使用异步模式，请在调用 BCPExec 方法之前，将特定于提供程序的会话属性 SSPROP_ASYNCH_BULKCOPY 设置为 VARIANT_TRUE。 此属性位于 DBPROPSET_SQLSERVERSESSION 属性集中。|  
  
## <a name="see-also"></a>另请参阅  
 [数据源对象 (OLE DB)](../../oledb/ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  
