---
title: 架构行集支持 (OLE DB) | Microsoft Docs
description: 处理 Transact-SQL 分布式查询时，OLE DB Driver for SQL Server 支持从链接的服务器返回架构信息。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- schema rowsets [OLE DB]
- OLE DB, schema rowsets
- OLE DB rowsets, schema
- OLE DB Driver for SQL Server, schema rowsets
- rowsets [OLE DB], schema
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 700d5b770176f9aefdab1ac7dfd43eee21f78d91
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754157"
---
# <a name="schema-rowset-support-ole-db"></a>架构行集支持 (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  此外，处理 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 分布式查询时，适用于 SQL Server 的 OLE DB 驱动程序支持从链接的服务器返回架构信息。  
  
> [!NOTE]  
>  尽管 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 支持同义词，但 OLE DB Driver for SQL Server 不返回同义词的元数据。  
  
 以下各表列出了适用于 SQL Server 的 OLE DB 驱动程序支持的架构行集和限制列。  
  
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
 [架构行集中的分布式查询支持](../../oledb/ole-db/schema-rowsets-distributed-query-support.md)  
  
 [LINKEDSERVERS 行集 &#40;OLE DB&#41;](../../oledb/ole-db/schema-rowsets-linkedservers-rowset.md)  
  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序编程](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)   
 [使用用户定义类型](../../oledb/features/using-user-defined-types.md)  
  
  
