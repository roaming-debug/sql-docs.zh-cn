---
description: " (Native Client OLE DB 提供程序的数据源信息属性) "
title: 数据源信息属性 (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, data source properties
- properties [OLE DB]
- data source properties [OLE DB]
- information properties [OLE DB]
- OLE DB data source properties [SQL Server Native Client]
ms.assetid: 7fd80e47-5bd9-41e2-a3d3-091a7c8c5f2b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8adc170ab669bafb77b859aca3b36dbc93fae1f2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749047"
---
#  <a name="sql-server-native-client-data-source-information-properties"></a>SQL Server Native Client 数据源信息属性
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在特定于访问接口的属性集 DBPROPSET_SQLSERVERDATASOURCEINFO 中，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口定义了以下数据源信息属性。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|SSPROP_COLUMNLEVELCOLLATION|类型：VT_BOOL<br /><br /> 读取/写入：读取<br /><br /> 默认值：VARIANT_TRUE<br /><br /> 说明：用于确定是否支持列排序规则。<br /><br /> VARIANT_TRUE：支持列级别排序规则。<br /><br /> VARIANT_FALSE：不支持列级别排序规则。|  
|SSPROP_UNICODELCID|类型：VT_I4 读取/写入：读取<br /><br /> 说明：Unicode 区域设置 ID。<br /><br /> 这是用于 Unicode 数据排序的区域设置。|  
|SSPROP_UNICODECOMPARISONSTYLE|类型：VT_I4 读取/写入：读取<br /><br /> 说明：Unicode 比较样式。<br /><br /> 用于 Unicode 数据排序的排序选项。|  
  
 在特定于访问接口的属性集 DBPROPSET_SQLSERVERSTREAM 中，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口定义了以下附加属性。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|SSPROP_STREAM_XMLROOT|类型：VT_BSTR 读取/写入：读取/写入<br /><br /> 说明：FOR XML 查询的结果可能不是格式正确的文档。 如果此属性已指定，“select ... for XML”查询结果会被包装在此属性提供的根标记中，以返回格式正确的 XML 文档。 如果查询是在浏览器中执行的，在加载结果时它可能导致浏览器显示分析器错误。 为了避免错误，SQL ISAPI 支持 ROOT 关键字。 此关键字映射到 SSPROP_STREAM_XMLROOT 属性。|  
  
## <a name="see-also"></a>另请参阅  
 [数据源对象 (OLE DB)](../../relational-databases/native-client-ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  
