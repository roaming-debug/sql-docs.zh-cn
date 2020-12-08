---
description: 从 JSON 中删除方括号 - WITHOUT_ARRAY_WRAPPER 选项
title: 从 JSON 中删除方括号 - WITHOUT_ARRAY_WRAPPER 选项
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- WITHOUT_ARRAY_WRAPPER
ms.assetid: aa86c2d1-458e-465f-abfa-75470137d054
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1140277ee7bb6c1375f7c1f19cbed92da9f19f3a
ms.sourcegitcommit: 28fecbf61ae7b53405ca378e2f5f90badb1a296a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96595183"
---
# <a name="remove-square-brackets-from-json---without_array_wrapper-option"></a>从 JSON 中删除方括号 - WITHOUT_ARRAY_WRAPPER 选项
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

若要删除默认括住 **FOR JSON** 子句的 JSON 输出的方括号，请指定 **WITHOUT_ARRAY_WRAPPER** 选项。 将此选项用于单行结果，生成单个 JSON 对象作为输出，而不是生成具有单个元素的数组。

如果将此选项用于多行结果，生成的输出将不会是有效的 JSON，因为存在多个元素并且缺少方括号。  
  
## <a name="example-single-row-result"></a>示例（单行结果）  
以下示例介绍了在指定和未指定 **WITHOUT_ARRAY_WRAPPER** 选项的情况下 **FOR JSON** 子句的输出。  
  
 **查询**  
  
```sql  
SELECT 2015 as year, 12 as month, 15 as day  
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER 
```  

 指定 **结果** with the **结果**  
  
```json  
{
    "year": 2015,
    "month": 12,
    "day": 15
} 
```  
  
 未指定 WITHOUT_ARRAY_WRAPPER 选项的结果（默认）  
  
```json  
[{
    "year": 2015,
    "month": 12,
    "day": 15
}]
```  

## <a name="example-multiple-row-result"></a>示例（多行结果）
下面又通过一个示例介绍了在指定 **FOR JSON** 选项的情况下 **WITHOUT_ARRAY_WRAPPER** 选项。 本示例生成多行结果。 输出不是有效的 JSON，因为存在多个元素并且缺少方括号。
  
 **查询**  
  
```sql  
SELECT TOP 3 SalesOrderNumber, OrderDate, Status  
FROM Sales.SalesOrderHeader  
ORDER BY ModifiedDate  
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER 
```  
  
 指定 **结果** with the **结果**  
  
```json  
{
    "SalesOrderNumber": "SO43662",
    "OrderDate": "2011-05-31T00:00:00",
    "Status": 5
}, {
    "SalesOrderNumber": "SO43661",
    "OrderDate": "2011-05-31T00:00:00",
    "Status": 5
}, {
    "SalesOrderNumber": "SO43660",
    "OrderDate": "2011-05-31T00:00:00",
    "Status": 5
} 
```  
  
 未指定 WITHOUT_ARRAY_WRAPPER 选项的结果（默认）  
  
```json  
[{
    "SalesOrderNumber": "SO43662",
    "OrderDate": "2011-05-31T00:00:00",
    "Status": 5
}, {
    "SalesOrderNumber": "SO43661",
    "OrderDate": "2011-05-31T00:00:00",
    "Status": 5
}, {
    "SalesOrderNumber": "SO43660",
    "OrderDate": "2011-05-31T00:00:00",
    "Status": 5
}]
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

-   [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>另请参阅  
 [FOR 子句 (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md)  
  
  
