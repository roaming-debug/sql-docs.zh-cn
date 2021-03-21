---
description: 会话属性 - SQL Server Native Client OLE DB 提供程序
title: 会话属性 OLE DB
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- sessions [OLE DB]
- SQL Server Native Client OLE DB provider, sessions
ms.assetid: 2498fbad-b3db-4bea-8fc6-fef5317d3eba
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4c914de9a6dcbc26444266365fce291d917e1370
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748487"
---
# <a name="session-properties---sql-server-native-client-ole-db-provider"></a>会话属性 - SQL Server Native Client OLE DB 提供程序
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序按如下方式解释 OLE DB 的会话属性。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|DBPROP_SESS_AUTOCOMMITISOLEVELS|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序支持所有的自动提交事务隔离级别，但 DBPROPVAL_TI_CHAOS 的混乱级别除外。|  
|||

 在特定于访问接口的属性集中 DBPROPSET_SQLSERVERSESSION， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序定义以下附加的会话属性。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|SSPROP_QUOTEDCATALOGNAMES|类型：VT_BOOL<br /><br /> R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE<br /><br /> 说明：在 CATALOG 限制中允许带引号的标识符。<br /><br /> VARIANT_TRUE：对提供分布式查询支持的架构行集的目录限制识别带引号的标识符。<br /><br /> VARIANT_FALSE：对提供分布式查询支持的架构行集的目录限制不识别带引号的标识符。<br /><br /> 有关提供分布式查询支持的架构行集的详细信息，请参阅[架构行集中的分布式查询支持](../../relational-databases/native-client/ole-db/schema-rowsets-distributed-query-support.md)。|  
|SSPROP_ALLOWNATIVEVARIANT|键入：VT_BOOL<br /><br /> R/W：读取/写入<br /><br /> 默认值：VARIANT_FALSE<br /><br /> 说明:确定提取的数据是作为 DBTYPE_VARIANT 还是作为 DBTYPE_SQLVARIANT。<br /><br /> VARIANT_TRUE：列类型作为 DBTYPE_SQLVARIANT 返回（这种情况下，缓冲区会保留 SSVARIANT 结构）。<br /><br /> VARIANT_FALSE：列类型作为 DBTYPE_VARIANT 返回，且缓冲区将具有 VARIANT 结构。|  
|SSPROP_ASYNCH_BULKCOPY|若要使用异步模式，请在调用 BCPExec 方法之前，将特定于提供程序的会话属性 SSPROP_ASYNCH_BULKCOPY 设置为 VARIANT_TRUE。 此属性位于 DBPROPSET_SQLSERVERSESSION 属性集中。|  
|||

## <a name="see-also"></a>另请参阅  
 [数据源对象 (OLE DB)](../../relational-databases/native-client-ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  
