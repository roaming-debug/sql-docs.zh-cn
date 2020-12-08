---
description: FOR JSON 如何转义特殊字符和控制字符 (SQL Server)
title: FOR JSON 如何转义特殊字符和控制字符
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON, special characters
ms.assetid: 4ba90025-5a09-4f0a-836a-54c886324530
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d7a99590fc36d246f19dbee3ebc227cfa10052d2
ms.sourcegitcommit: 28fecbf61ae7b53405ca378e2f5f90badb1a296a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96595166"
---
# <a name="how-for-json-escapes-special-characters-and-control-characters-sql-server"></a>FOR JSON 如何转义特殊字符和控制字符 (SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

  本主题介绍了 SQL Server **SELECT** 语句的 **FOR JSON** 子句在 JSON 输出中如何转义特殊字符以及如何表示控制字符。  

> [!IMPORTANT]
> 此页介绍了 Microsoft SQL Server 中对 JSON 的内置支持。 有关 JSON 中的转义和编码的常规信息，请参阅 JSON RFC - [https://www.ietf.org/rfc/rfc4627.txt](https://www.ietf.org/rfc/rfc4627.txt) 中的 2.5 部分。

## <a name="escaping-of-special-characters"></a>特殊字符转义  
如果源数据包含特殊字符，则 **FOR JSON** 子句在 JSON 输出中会使用 `\` 对其进行转义，如下表中所示。 在属性名称及其值中，均会发生这种转义。  
  
|**特殊字符**|**转义后的输出**|  
|---------------------------|--------------------------|  
|引号 (")|\\"|  
|反斜杠 (\\)|\\\\|  
|正斜杠 (/)|\\/|  
|Backspace|\b|  
|换页|\f|  
|换行|\n|  
|回车|\r|  
|水平制表符|\t|  
  
## <a name="control-characters"></a>控制字符  
如果源数据包含控制字符，则 **FOR JSON** 子句在 JSON 输出中会使用 `\u<code>` 格式对其进行转义，如下表中所示。  
  
|**控制字符**|**编码后的输出**|  
|---------------------------|--------------------------|  
|CHAR(0)|\u0000|  
|CHAR(1)|\u0001|  
|...|...|  
|CHAR(31)|\u001f|  
  
## <a name="example"></a>示例  
 下面是同时包含特殊字符和控制字符的源数据的 **FOR JSON** 输出的示例。  
  
 查询：  
  
```sql  
SELECT  
  'VALUE\    /  
  "' as [KEY\/"],  
  CHAR(0) as '0',  
  CHAR(1) as '1',  
  CHAR(31) as '31'  
FOR JSON PATH  
```  
  
 结果:  
  
```json  
{
    "KEY\\\t\/\"": "VALUE\\\t\/\r\n\"",
    "0": "\u0000",
    "1": "\u0001",
    "31": "\u001f"
}
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

-   [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>另请参阅  
 [借助 FOR JSON 将查询结果的格式设置为 JSON (SQL Server)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)  
[FOR 子句](../../t-sql/queries/select-for-clause-transact-sql.md)
