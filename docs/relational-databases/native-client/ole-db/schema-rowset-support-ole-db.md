---
description: 'SQL Server Native Client (OLE DB 中支持架构行集) '
title: 架构行集支持 (OLE DB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- schema rowsets [OLE DB]
- OLE DB, schema rowsets
- OLE DB rowsets, schema
- SQL Server Native Client OLE DB provider, schema rowsets
- rowsets [OLE DB], schema
ms.assetid: a75b4b69-b095-4690-9b31-a2b32a67489e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 416ca535a2d38134a7c7d9be76006b096e8ae8be
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104743627"
---
# <a name="schema-rowset-support-in-sql-server-native-client-ole-db"></a>SQL Server Native Client (OLE DB 中支持架构行集) 
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]在处理分布式查询时，Native Client OLE DB 提供程序还支持从链接服务器返回架构信息 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 。  
  
> [!NOTE]  
>  尽管 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 支持同义词，但 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 不返回同义词的元数据。  
  
 下表列出了架构行集以及 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序支持的限制列。  
  
|架构行集|限制列|  
|-------------------|-------------------------|  
|DBSCHEMA_CATALOGS|CATALOG_NAME|  
|DBSCHEMA_COLUMN_PRIVILEGES|支持所有限制。<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME GRANTOR GRANTEE|  
|DBSCHEMA_COLUMNS|支持所有限制。<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME COLUMN_NAME<br /><br /> 以下附加列专用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]：<br /><br /> COLUMN_LCID，这是排序规则的区域设置 ID。 COLUMN_LCID 是与 Windows LCID 相同的值。<br /><br /> COLUMN_COMPFLAGS 定义对于排序规则支持哪些比较。 数据格式与 DBPROB_FINDCOMPAREOPS 相同。<br /><br /> COLUMN_SORTID，这是用于排序规则的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 排序样式。<br /><br /> COLUMN_TDSCOLLATION，这是用于列的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 排序规则。<br /><br /> IS_COMPUTED，如果列为计算列，则为 VARIANT_TRUE；否则为 VARIANT_FALSE。|  
|DBSCHEMA_FOREIGN_KEYS|支持所有限制。<br /><br /> PK_TABLE_CATALOG PK_TABLE_SCHEMA PK_TABLE_NAME FK_TABLE_CATALOG FK_TABLE_SCHEMA FK_TABLE_NAME|  
|DBSCHEMA_INDEXES|支持限制 1、2、3 和 5。<br /><br /> TABLE_CATALOG TABLE_SCHEMA INDEX_NAME TABLE_NAME|  
|DBSCHEMA_PRIMARY_KEYS|支持所有限制。<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|DBSCHEMA_PROCEDURE_PARAMETERS|支持所有限制。<br /><br /> PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME PARAMETER_NAME|  
|DBSCHEMA_PROCEDURES|支持限制 1、2 和 3。<br /><br /> PROCEDURE_CATALOG PROCEDURE_SCHEMA PROCEDURE_NAME<br /><br /> DBSCHEMA_PROCEDURES 只返回可由当前用户执行的过程，或返回已向当前用户授予 VIEW DEFINITION 权限的过程。|  
|DBSCHEMA_PROVIDER_TYPES|支持所有限制。<br /><br /> DATA_TYPE BEST_MATCH|  
|DBSCHEMA_SCHEMATA|支持所有限制。<br /><br /> CATALOG_NAME SCHEMA_NAME SCHEMA_OWNER|  
|DBSCHEMA_STATISTICS|支持所有限制。<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME|  
|DBSCHEMA_TABLE_CONSTRAINTS|支持所有限制。<br /><br /> CONSTRAINT_CATALOG CONSTRAINT_SCHEMA CONSTRAINT_NAME TABLE_CATALOG TABLE_SCHEMA TABLE_NAME CONSTRAINT_TYPE|  
|DBSCHEMA_TABLE_PRIVILEGES|支持所有限制。<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME GRANTOR GRANTEE|  
|DBSCHEMA_TABLES|支持所有限制。<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME TABLE_TYPE|  
|DBSCHEMA_TABLES_INFO|支持所有限制。<br /><br /> TABLE_CATALOG TABLE_SCHEMA TABLE_NAME TABLE_TYPE|  
  
## <a name="in-this-section"></a>本节内容  
 [架构行集中的分布式查询支持](../../../relational-databases/native-client/ole-db/schema-rowsets-distributed-query-support.md)  
  
 [LINKEDSERVERS 行集 &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/schema-rowsets-linkedservers-rowset.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)   
 [使用用户定义类型](../../../relational-databases/native-client/features/using-user-defined-types.md)  
  
  
