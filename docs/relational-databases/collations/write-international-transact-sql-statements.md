---
description: 编写国际化 Transact-SQL 语句
title: 编写国际化 Transact-SQL 语句 | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- writing international statements
- Transact-SQL international considerations
- international considerations [SQL Server], Transact-SQL
- Database Engine international considerations [SQL Server], Transact-SQL
- statements [SQL Server], international
- database international considerations [SQL Server], Transact-SQL
- dates [SQL Server], international considerations
ms.assetid: f0b10fee-27f7-45fe-aece-ccc3f63bdcdb
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8872cf6e593279a282c34868a2f019ac82e5bf01
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "100339488"
---
# <a name="write-international-transact-sql-statements"></a>编写国际化 Transact-SQL 语句
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  如果遵循以下指导原则，则使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的数据库和数据库应用程序将变得更易于在语言之间移植，或者将支持多种语言：  

-   从 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 开始，在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]中，使用以下任一选项：
    -   具有已启用 [UTF-8](../../relational-databases/collations/collation-and-unicode-support.md#utf8) 的排序规则的 char、varchar 和 varchar(max) 数据类型，并使用 UTF-8 对数据进行编码。
    -   具有已启用[附属字符 (SC)](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters) 的排序规则的 nchar、nvarchar 和 nvarchar(max) 数据类型，并使用 UTF-16 对数据进行编码。 使用非 SC 排序规则会导致使用 UCS-2 对数据进行编码。      

    这避免了代码页转换问题。 有关其他注意事项，请参阅 [UTF-8 与 UTF-16 的存储差异](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences)。  

-   到 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 为止，将 char、varchar 和 varchar(max) 数据类型的所有使用替换为 nchar、nvarchar 和 nvarchar(max)。 如果使用已启用[增补字符 (SC)](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters) 的排序规则，则使用 UTF-16 对数据进行编码。 使用非 SC 排序规则会导致使用 UCS-2 对数据进行编码。 这避免了代码页转换问题。 有关详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。 

    > [!IMPORTANT]
    > text 数据类型已弃用，不应在新的开发工作中使用此数据类型。 计划将 text 数据转换为 varchar(max)。
  
-   当执行月份和星期的比较与操作时，请使用数值日期，而不要使用名称字符串。 不同的语言设置返回的月份和工作日的名称也不同。 例如，当语言设置为美国英语时，`DATENAME(MONTH,GETDATE())` 返回 `May`，当语言设置为德语时，返回 `Mai`，当语言设置为法语时，返回 `mai`。 应使用以数字而非名称表示月份的函数，如 [DATEPART](../../t-sql/functions/datepart-transact-sql.md)。 在生成显示给用户的结果集时，请使用 DATEPART 名称，因为日期名称通常比数值表示形式更有意义。 但是，编写逻辑代码时不要使用任何依赖于特定语言显示的名称。  
  
-   当指定用于比较的日期，或者指定用于 INSERT 或 UPDATE 语句的输入的日期时，请使用对于所有的语言设置解释方式都相同的常量：  
  
    -   ADO、OLE DB 和 ODBC 应用程序应使用下列 ODBC 时间戳、日期和时间转义子句：  
  
         { ts'yyyy - mm - dd hh : mm : ss [.fff] '}，例如：{ ts'1998-09-24 10:02:20'}        
  
         { d' yyyy - mm - dd '} 例如：{ d'1998-09-24'} 
  
         { t' hh : mm : ss '}，例如：{ t'10:02:20'}   
  
    -   使用其他 API 的应用程序或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本、存储过程和触发器都应使用未分隔数值字符串。 例如 *yyyymmdd* 为 19980924。  
  
    -   使用其他 API 的应用程序或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本、存储过程和触发器在进行 time、date、smalldate、datetime、datetime2 和 datetimeoffset 数据类型与字符串数据类型之间的所有转换时都应使用带有显式样式参数的 [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) 语句     。 例如，以下语句对于所有语言或日期格式连接设置的解释方式都相同：  
  
        ```sql  
        SELECT *  
        FROM AdventureWorks2012.Sales.SalesOrderHeader  
        WHERE OrderDate = CONVERT(DATETIME, '20060719', 101)  
        ```  
  
## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)     
[DATEPART (Transact-SQL)](../../t-sql/functions/datepart-transact-sql.md)        
[排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)      
