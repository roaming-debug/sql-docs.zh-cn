---
title: 架构限制
description: 介绍使用 Microsoft SqlClient Data Provider for SQL Server 时可用于 GetSchema 的架构限制。
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8d72e2dd020f1345ceb0b68249cb3d3acd1024dc
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051259"
---
# <a name="schema-restrictions"></a>架构限制

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

GetSchema 方法的第二个可选参数是用于限制返回的架构信息量的限制，该参数以字符串数组的形式传递给 GetSchema 方法 。 在数组中的位置确定可以传递的值，这等效于限制数。  
  
例如，下表说明使用 Microsoft SqlClient Data Provider for SQL Server 时“Tables”架构集合支持的限制。 SQL Server 架构集合的其他限制在本主题的结尾处列出。  

|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|TABLE_CATALOG|1|  
|所有者|@Owner|TABLE_SCHEMA|2|  
|表|@Name|TABLE_NAME|3|  
|TableType|@TableType|TABLE_TYPE|4|  
 
## <a name="specifying-restriction-values"></a>指定限制值  

要使用“Tables”架构集合的一个限制，只需创建一个包含四个元素的字符串数组，然后在与限制数匹配的元素中填充值。 例如，要将 GetSchema 方法返回的表限制为仅返回“Sales”架构中的表，可将数组的第二个元素设置为“Sales”，然后再将其传递给 GetSchema 方法 。  
  
> [!NOTE]
> - `SqlClient` 的限制集合具有额外的 `ParameterName` 列。 为了向后兼容，仍提供限制默认列，但是目前忽略该列。 在指定限制值时，应使用参数化查询（而不是字符串替换）来最大程度地降低受到 SQL 注入式攻击的风险。  
> - 数组中的元素数必须小于或等于指定架构集合支持的限制数，否则，将引发 <xref:System.ArgumentException>。 可以小于最大限制数。 缺少的限制假定为空（无限制）。  
  
可以通过调用包含限制架构集合名称（即“Restrictions”）的 GetSchema 方法来查询 Microsoft SqlClient Data Provider for SQL Server，以确定支持的限制列表。 此时将返回 <xref:System.Data.DataTable>，包含集合名称、限制名称、默认限制值和限制数的列表。  
  
### <a name="example"></a>示例  

以下示例演示如何使用 Microsoft SqlClient Data Provider for SQL Server <xref:Microsoft.Data.SqlClient.SqlConnection> 类的 <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A> 方法来检索与 AdventureWorks 示例数据库中包含的所有表有关的架构信息，并将返回的信息仅限于“Sales”中的表：  

[!code-csharp[SqlClient GetSchema with restrictions#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Restriction.cs#1)]  

## <a name="sql-server-schema-restrictions"></a>SQL Server 架构限制  

 下表列出了 SQL Server 架构集合的限制。  
  
### <a name="users"></a>用户  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|User_Name|@Name|name|1|  
  
### <a name="databases"></a>数据库  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|名称|@Name|名称|1|  
  
### <a name="tables"></a>表  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|TABLE_CATALOG|1|  
|所有者|@Owner|TABLE_SCHEMA|2|  
|表|@Name|TABLE_NAME|3|  
|TableType|@TableType|TABLE_TYPE|4|  
  
### <a name="columns"></a>列  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|TABLE_CATALOG|1|  
|所有者|@Owner|TABLE_SCHEMA|2|  
|表|@Table|TABLE_NAME|3|  
|列|@Column|COLUMN_NAME|4|  
  
### <a name="structuredtypemembers"></a>StructuredTypeMembers  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|TABLE_CATALOG|1|  
|所有者|@Owner|TABLE_SCHEMA|2|  
|表|@Table|TABLE_NAME|3|  
|列|@Column|COLUMN_NAME|4|  
  
### <a name="views"></a>视图  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|TABLE_CATALOG|1|  
|所有者|@Owner|TABLE_SCHEMA|2|  
|表|@Table|TABLE_NAME|3|  
  
### <a name="viewcolumns"></a>ViewColumns  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|VIEW_CATALOG|1|  
|所有者|@Owner|VIEW_SCHEMA|2|  
|表|@Table|VIEW_NAME|3|  
|列|@Column|COLUMN_NAME|4|  
  
### <a name="procedureparameters"></a>ProcedureParameters  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|SPECIFIC_CATALOG|1|  
|所有者|@Owner|SPECIFIC_SCHEMA|2|  
|名称|@Name|SPECIFIC_NAME|3|  
|参数|@Parameter|PARAMETER_NAME|4|  
  
### <a name="procedures"></a>过程  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|SPECIFIC_CATALOG|1|  
|所有者|@Owner|SPECIFIC_SCHEMA|2|  
|名称|@Name|SPECIFIC_NAME|3|  
|类型|@Type|ROUTINE_TYPE|4|  
  
### <a name="indexcolumns"></a>IndexColumns  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|db_name()|1|  
|所有者|@Owner|user_name()|2|  
|表|@Table|o.name|3|  
|ConstraintName|@ConstraintName|x.name|4|  
|列|@Column|c.name|5|  
  
### <a name="indexes"></a>索引  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|db_name()|1|  
|所有者|@Owner|user_name()|2|  
|表|@Table|o.name|3|  
  
### <a name="userdefinedtypes"></a>UserDefinedTypes  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|assembly_name|@AssemblyName|assemblies.name|1|  
|udt_name|@UDTName|types.assembly_class|2|  
  
### <a name="foreignkeys"></a>ForeignKeys  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|CONSTRAINT_CATALOG|1|  
|所有者|@Owner|CONSTRAINT_SCHEMA|2|  
|表|@Table|TABLE_NAME|3|  
|名称|@Name|CONSTRAINT_NAME|4|  
  
## <a name="see-also"></a>另请参阅

- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
