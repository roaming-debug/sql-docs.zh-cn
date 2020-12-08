---
description: FOR JSON 如何将 SQL Server 数据类型转换为 JSON 数据类型 (SQL Server)
title: FOR JSON 如何将 SQL Server 数据类型转换为 JSON 数据类型
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON, data type conversion
ms.assetid: da356f06-efd0-4ea3-8157-77395bf790d7
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7a9b6cb32af496b70a48ef4d32f3692b7b863b28
ms.sourcegitcommit: 28fecbf61ae7b53405ca378e2f5f90badb1a296a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96595101"
---
# <a name="how-for-json-converts-sql-server-data-types-to-json-data-types-sql-server"></a>FOR JSON 如何将 SQL Server 数据类型转换为 JSON 数据类型 (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

  **FOR JSON** 子句在 JSON 输出中使用以下规则将 SQL Server 数据类型转换为 JSON 类型。  
  
|Category|SQL Server 数据类型|JSON 数据类型|  
|--------------|--------------|---------------|  
|字符和字符串类型|char、nchar、varchar、nvarchar|字符串|  
|数字类型|int、bigint、float、decimal、numeric|number|  
|位类型|bit|布尔值（true 或 false）|  
|日期和时间类型|date、datetime、datetime2、time、datetimeoffset|字符串|  
|二进制类型|varbinary、binary、image、timestamp、rowversion|BASE64 编码的字符串|  
|CLR 类型|geometry、geography、其他 CLR 类型|不支持。 这些类型将返回错误。<br /><br /> 在 SELECT 语句中，使用 CAST 或 CONVERT，或使用 CLR 属性或方法，将源数据转换为可成功转换成 JSON 类型的 SQL Server 数据类型。 例如，对 geometry 类型使用 **STAsText()**，或对任何 CLR 类型使用 **ToString()**。 然后，JSON 输出值的类型将派生自 SELECT 语句中应用的转换的返回类型。|  
|其他类型|uniqueidentifier、money|字符串|  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

-   [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>另请参阅  
 [借助 FOR JSON 将查询结果的格式设置为 JSON (SQL Server)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)  
  
  
