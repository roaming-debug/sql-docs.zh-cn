---
title: xml (Transact-SQL)
description: xml (Transact-SQL)
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- XML_TSQL
- xml_TSQL
- xml
dev_langs:
- TSQL
helpviewer_keywords:
- xml data type [SQL Server], about xml data type
ms.assetid: 9198f671-8e61-4ca4-9c3a-859f84020e62
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: ''
ms.date: 07/26/2017
ms.openlocfilehash: a19e6cdb215e48965490c973f4daf0506d1cb805
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161050"
---
# <a name="xml-transact-sql"></a>xml (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  存储 XML 数据的数据类型。 可在列中或者 xml 类型的变量中存储 xml 实例 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```
xml ( [ CONTENT | DOCUMENT ] xml_schema_collection )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 CONTENT  
 将 xml 实例限制为格式正确的 XML 片段。 XML 数据的顶层可包含多个零或多个元素。 还允许在顶层使用文本节点。  
  
 此选项为默认行为。  
  
 DOCUMENT  
 将 xml 实例限制为格式正确的 XML 片段。 XML 数据必须且只能有一个根元素。 不允许在顶层使用文本节点。  
  
 xml_schema_collection  
 XML 架构集合的名称。 若要创建类型化的 xml 列或变量，可选择指定 XML 架构集合名称。 有关类型化和非类型化 XML 的详细信息，请参阅[类型化的 XML 与非类型化的 XML 的比较](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)。  
  
## <a name="remarks"></a>备注  
 存储的 xml 数据类型表示实例大小不能超过 2 GB。  
  
 CONTENT 和 DOCUMENT 方面仅应用于类型化的 XML。 有关详细信息，请参阅[类型化的 XML 与非类型化的 XML 的比较](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)。  
  
## <a name="examples"></a>示例  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @DemographicData XML (Person.IndividualSurveySchemaCollection);  
SET @DemographicData = (SELECT TOP 1 Demographics FROM Person.Person);  
SELECT @DemographicData;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据类型转换（数据库引擎）](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [xml 数据类型方法](../../t-sql/xml/xml-data-type-methods.md)   
 [XQuery 语言参考 (SQL Server)](../../xquery/xquery-language-reference-sql-server.md)  
  
  
