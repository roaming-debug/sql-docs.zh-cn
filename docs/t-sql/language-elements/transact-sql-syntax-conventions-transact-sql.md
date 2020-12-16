---
description: Transact-SQL 语法约定 (Transact-SQL)
title: Transact-SQL 语法约定 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- sql13.TSQLExpandPortal.f1
dev_langs:
- TSQL
helpviewer_keywords:
- conventions [SQL Server]
- Applies to section in Transact-SQL topics
- code example conventions [SQL Server]
- objects [SQL Server], names
- code [SQL Server], conventions
- multipart names [SQL Server]
- Transact-SQL syntax conventions
- syntax conventions [SQL Server]
- code [SQL Server]
- Transact-SQL
- naming conventions [SQL Server]
- syntax [SQL Server], Transact-SQL
ms.assetid: 35fbcf7f-8b55-46cd-a957-9b8c7b311241
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cea868a3023b8b69ef69384f03983183d869531b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464248"
---
# <a name="transact-sql-syntax-conventions-transact-sql"></a>Transact-SQL 语法约定 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

下表列出了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 参考的语法关系图中使用的约定，并进行了说明。  
  
|约定|用于|  
|----------------|--------------|  
|大写|[!INCLUDE[tsql](../../includes/tsql-md.md)] 关键字。|  
|_斜体_|用户提供的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法的参数。|  
|**粗体**|完全按显示原样键入数据库名称、表名称、列名、索引名称、存储过程、实用工具、数据类型名称和文本。|  
|下划线|指示当语句中省略了包含带下划线的值的子句时应用的默认值。|  
|&#124; （垂直条）|分隔括号或大括号中的语法项。 只能使用其中一项。|  
|`[ ]`（方括号）|可选语法项。 不要键入方括号。|  
|{}（大括号）|必选语法项。 不要键入大括号。|  
|[  ..._n_]|指示前面的项可以重复 _n_ 次。 匹配项由逗号分隔。|  
|[...n  ]|指示前面的项可以重复 _n_ 次。 每一项由空格分隔。|  
|;|[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句终止符。 虽然此版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中大部分语句都不需要分号，但今后发布的版本需要分号。|  
|\<label> ::=|语法块的名称。 使用此约定，可以对能在一条语句中的多个位置使用的过长语法段或语法单元进行分组和标记。 可使用语法块的各个位置用括在尖括号内的标签指明：\<label>。<br /><br /> 集是表达式的集合，例如 \<grouping set>；列表是集的集合，例如 \<composite element list>。|  
  
## <a name="multipart-names"></a>多部分名称  
除非另外指定，否则，所有对数据库对象名的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 引用将是由四部分组成的名称，格式如下：  
  
server\_name  .[database\_name  ].[schema\_name  ].object\_name  
  
| database\_name  .[schema\_name  ].object\_name  
 
| schema\_name  .object\_name  
  
| _object\_name_  
  
_server\_name_  
指定链接的服务器名称或远程服务器名称。  
  
database\_name  
如果对象驻留在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地实例中，则指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的名称。 如果对象在链接服务器中，则 database_name 将指定 OLE DB 目录  。  
  
schema\_name  
如果对象在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中，则指定包含对象的架构的名称。 如果对象在链接服务器中，则 schema_name 将指定 OLE DB 架构名称  。  
  
object\_name  
对象的名称。  
  
引用某个特定对象时，不必总是指定服务器、数据库和架构，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 也能标识对象。 不过，如果找不到对象，便会返回错误。  
  
> [!NOTE]  
> 为了避免名称解析错误，建议只要指定了架构范围内的对象时就指定架构名称。  
  
若要省略中间节点，请使用句点来指示这些位置。 下表显示了对象名的有效格式。  
  
|对象引用格式|说明|  
|-----------------------------|-----------------|  
|server  .database  .schema  .object|四个部分的名称。|  
|server  .database  ..object|省略架构名称。|  
|server  ..schema  .object|省略数据库名称。|  
|server  ...object|省略数据库和架构名称。|  
|database  .schema  .object|省略服务器名。|  
|database  ..object|省略服务器和架构名称。|  
|schema  .object|省略服务器和数据库名称。|  
|对象|省略服务器、数据库和架构名称。|  
  
## <a name="code-example-conventions"></a>代码示例约定  
除非专门说明，否则，在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 参考中提供的示例都已使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 及其以下选项的默认设置进行了测试：  
  
-   ANSI_NULLS  
-   ANSI_NULL_DFLT_ON  
-   ANSI_PADDING  
-   ANSI_WARNINGS  
-   CONCAT_NULL_YIELDS_NULL  
-   QUOTED_IDENTIFIER  
  
[!INCLUDE[tsql](../../includes/tsql-md.md)] 参考中的大多数代码示例都已在运行区分大小写排序顺序的服务器上进行了测试。 测试服务器通常运行 ANSI/ISO 1252 代码页。  
  
许多代码示例用字母 N 作为 Unicode 字符串常量的前缀  。如果没有 N 前缀，则字符串被转换为数据库的默认代码页  。 此默认代码页可能不识别某些字符。  
  
## <a name="applies-to-references"></a>“适用于”引用  
[!INCLUDE[tsql](../../includes/tsql-md.md)] 引用包含的文章涉及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本）、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 和 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]。   

每篇文章的顶部附近有指明哪些产品支持文章主题的部分。 如果省略了某产品，文章描述的功能就不适用于此产品。 例如，在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]中介绍了可用性组。 “创建可用性组”一文指明它适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本），因为它不适用于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 或 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  。  
  
文章的常规主题可能用于某一产品，但在某些情况下，所有参数都不受支持。 例如，在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]中介绍了包含的数据库用户。 CREATE USER  语句可用于任何 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品，但 WITH PASSWORD  语法无法用于旧版本。 其他“适用于”  部分插入到文章正文中的相应参数说明中。  
  
## <a name="see-also"></a>另请参阅  
[Transact-SQL 参考（数据库引擎）](../language-reference.md)    
[保留关键字 (Transact SQL)](../../t-sql/language-elements/reserved-keywords-transact-sql.md)      
[Transact-SQL 设计问题](/previous-versions/visualstudio/visual-studio-2010/dd193411(v=vs.100))    
[Transact-SQL 命名问题](/previous-versions/visualstudio/visual-studio-2010/dd193246(v=vs.100))        
[Transact-SQL 性能问题](/previous-versions/visualstudio/visual-studio-2010/dd172117(v=vs.100))