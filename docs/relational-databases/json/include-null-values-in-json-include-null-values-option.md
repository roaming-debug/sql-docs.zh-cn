---
description: 在 JSON 中添加 NULL 值 - INCLUDE_NULL_VALUES 选项
title: 在 JSON 中添加 NULL 值 - INCLUDE_NULL_VALUES 选项
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- INCLUDE_NULL_VALUES (FOR JSON)
ms.assetid: 06873768-3778-4ed8-a1db-61758726bda0
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 400b6f6198f44cb48c533c27c324fe78d13aa324
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483389"
---
# <a name="include-null-values-in-json---include_null_values-option"></a>在 JSON 中添加 NULL 值 - INCLUDE_NULL_VALUES 选项
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

  若要在 **FOR JSON** 子句的 JSON 输出中添加 NULL 值，请指定 **INCLUDE_NULL_VALUES** 选项。  
  
 如果你没有指定 **INCLUDE_NULL_VALUES** 选项，则 JSON 输出不会包括查询结果中 NULL 值所对应的属性。  
  
## <a name="examples"></a>示例  
 以下示例介绍了在指定和未指定 **INCLUDE_NULL_VALUES** 选项的情况下 **FOR JSON** 子句的输出。  
  
|未指定 **INCLUDE_NULL_VALUES** 选项|已指定 **INCLUDE_NULL_VALUES** 选项|  
|--------------------------------------------------|-----------------------------------------------|  
|`{    "name": "John",    "surname": "Doe" }`|`{    "name": "John",    "surname": "Doe",    "age": null,    "phone": null }`|  
  
 下面又通过一个示例介绍了在指定 **INCLUDE_NULL_VALUES** 选项的情况下 **FOR JSON** 子句的输出。  
  
 **查询**  
  
```sql  
SELECT name, surname  
FROM emp  
FOR JSON AUTO, INCLUDE_NULL_VALUES    
```  
  
 **结果**  
  
```json  
[{
    "name": "John",
    "surname": null
}, {
    "name": "Jane",
    "surname": "Doe"
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
  
  
