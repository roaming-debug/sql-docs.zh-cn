---
title: 查询表达式和统一资源名称
description: 了解用于枚举对象模型层次结构中一个或多个对象的查询表达式，以及用于唯一地标识单个对象的统一资源名称 (URN)。
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
helpviewer_keywords:
- query expressions
- unique resource names
- URN
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: e1955201f60f58c6513928c7185dbbdad16cf445
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839458"
---
# <a name="query-expressions-and-uniform-resource-names"></a>查询表达式和统一资源名称

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 管理对象 (SMO) 模型和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell 管理单元使用与 XPath 表达式相似的两种类型的表达式。 查询表达式是指定一组条件的字符串，用于枚举对象模型层次结构中的一个或多个对象。 统一资源名称 (URN) 是一种特定类型的查询表达式字符串，用于唯一标识单个对象。  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="syntax"></a>语法  

```powershell
  
Object1[<FilterExpression1>]/ ... /ObjectN[<FilterExpressionN>]  
  
<FilterExpression>::=  
<PropertyExpression> [and <PropertyExpression>][...n]  
  
<PropertyExpression>::=  
      @BooleanPropertyName=true()  
 | @BooleanPropertyName=false()  
 | contains(@StringPropertyName, 'PatternString')  
  | @StringPropertyName='String'  
 | @DatePropertyName=datetime('DateString')  
 | is_null(@PropertyName)  
 | not(<PropertyExpression>)  
```  

## <a name="arguments"></a>自变量

*Object*  
指定在表达式字符串的节点表示的对象的类型。 每个对象表示那些 SMO 对象模型名称空间中的一个集合类：  

<xref:Microsoft.SqlServer.Management.Smo>  

<xref:Microsoft.SqlServer.Management.Smo.Agent>  

<xref:Microsoft.SqlServer.Management.Smo.Broker>  

<xref:Microsoft.SqlServer.Management.Smo.Mail>  

<xref:Microsoft.SqlServer.Management.Dmf>  

<xref:Microsoft.SqlServer.Management.Facets>  

<xref:Microsoft.SqlServer.Management.RegisteredServers>  

<xref:Microsoft.SqlServer.Management.Smo.RegSvrEnum>  

例如，为 **ServerCollection** 类指定“服务器”，为 **DatabaseCollection** 类指定“数据库”。  

\@*PropertyName*  
指定与“对象”中指定的对象相关联的类的其中一个属性的名称。 属性名必须以字符 \@ 为前缀。 例如，对 Database 类的 IsAnsiNull 属性指定 \@IsAnsiNull。  
  
\@*BooleanPropertyName*=true()  
枚举指定的布尔属性设置为 TRUE 的所有对象。  
  
\@*BooleanPropertyName*=false()  
枚举指定的布尔属性设置为 FALSE 的所有对象。  
  
contains(\@StringPropertyName, 'PatternString')  
枚举指定的字符串属性中包含“*PatternString*”中指定的一组字符（至少出现一次）的所有对象。  
  
\@*StringPropertyName*='*PatternString*'  
枚举指定字符串属性的值与在“*PatternString*”中指定的字符模式完全相同的所有对象。  
  
\@*DatePropertyName*= datetime('*DateString*')  
枚举指定日期属性的值与在“*DateString*”中指定的日期相匹配的所有对象。 DateString 必须遵循格式 yyyy-mm-dd hh:mi:ss.mmm。  
  
|DateString 组件|说明|  
|-|-|  
|yyyy|四位数年份。|  
|mm|两位数月份（01 到 12）。|  
|dd|两位数日期（01 到 31）。|  
|hh|24 小时制的两位数小时（01 到 23）。|  
|mi|两位数分钟（01 到 59）。|  
|ss|两位数秒数（01 到 59）。|  
|mmm|毫秒数（001 到 999）。|  
  
 可以按照存储在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]中的任何日期格式，计算以此格式指定的日期。  
  
 is_null(\@PropertyName)  
 枚举指定属性的值为 NULL 的所有对象。  
  
 not(\<*PropertyExpression*>)  
 对 *PropertyExpression* 的计算值求反，枚举与 *PropertyExpression* 中指定的条件不匹配的所有对象。 例如，not(contains(\@Name, 'xyz')) 枚举名称中不含字符串 xyz 的所有对象。  
  
## <a name="remarks"></a>备注  

查询表达式是一些字符串，用于枚举 SMO 模型层次结构中的节点。 每个节点有一个筛选表达式，指定用于确定要枚举该节点的哪些对象的条件。 查询表达式在 XPath 表达式语言上建模。 查询表达式实现了 XPath 支持的一小部分表达式，还具有 XPath 中不提供的一些扩展。 XPath 表达式是一些字符串，指定用于枚举 XML 文档中的一个或多个标记的一组条件。 有关 XPath 的详细信息，请参阅 [W3C XPath Language](http://www.w3.org/TR/xpath20/)（W3C Xpath 语言）。  

查询表达式必须以对服务器对象的绝对引用开头。 不允许使用以 / 开头的相对表达式。 查询表达式中指定的对象的顺序必须遵循相关对象模型中集合对象的层次结构。 例如，引用 Microsoft.SqlServer.Management.Smo 命令空间中对象的查询表达式必须从服务器节点开始，接下来才是数据库节点等。  

如果没有为对象指定 \<FilterExpression>，将枚举该节点的所有对象。  
  
## <a name="uniform-resource-names-urn"></a>统一资源名称 (URN)  

URN 是查询表达式的子集。 每个 URN 形成对单个对象的完全限定引用。 典型的 URN 使用 Name 属性来标识每个节点的单个对象。 例如，该 URN 引用一个特定列：  
  
```powershell
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012']/Table[@Name='SalesPerson' and @Schema='Sales']/Column[@Name='SalesPersonID']  
```
  
## <a name="examples"></a>示例  
  
### <a name="a-enumerating-objects-using-false"></a>A. 使用 false() 枚举对象  
 此查询表达式枚举 **MyComputer** 上默认实例中 **AutoClose** 属性设置为 False 的所有数据库。  
  
```  
Server[@Name='MYCOMPUTER']/Database[@AutoClose=false()]  
```  
  
### <a name="b-enumerating-objects-using-contains"></a>B. 使用 contains 枚举对象  
 此查询表达式枚举所有区分大小写、并且名称中包含“m”的数据库。  
  
```  
Server[@Name='MYCOMPUTER']/Database[@CaseSensitive=false() and contains(@Name, 'm')]   
```  
  
### <a name="c-enumerating-objects-using-not"></a>C. 使用 not 枚举对象  
 此查询表达式枚举不在 [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] Production **架构中、而且表名中包含“History”一词的所有** 表：  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012']/Table[not(@Schema='Production') and contains(@Name, 'History')]  
```  
  
### <a name="d-not-supplying-a-filter-expression-for-the-final-node"></a>D. 不为最终节点提供筛选表达式  
 此查询表达式枚举 **AdventureWorks2012.Sales.SalesPerson** 表中的所有列：  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012"]/Table[@Schema='Sales' and @Name='SalesPerson']/Columns  
```  
  
### <a name="e-enumerating-objects-using-datetime"></a>E. 使用 datetime 枚举对象  
 此查询表达式枚举 [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] 数据库中于特定时间创建的所有表：  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012"]/Table[@CreateDate=datetime('2008-03-21 19:49:32.647')]  
```  
  
### <a name="f-enumerating-objects-using-is_null"></a>F. 使用 is_null 枚举对象  
 此查询表达式枚举 [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] 数据库中上次修改日期属性不为 Null 的所有表：  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012"]/Table[Not(is_null(@DateLastModified))]  
```  
  
## <a name="see-also"></a>另请参阅

- [Invoke-PolicyEvaluation cmdlet](/powershell/module/sqlserver/Invoke-PolicyEvaluation)
- [SQL Server Audit（数据库引擎）](../relational-databases/security/auditing/sql-server-audit-database-engine.md)