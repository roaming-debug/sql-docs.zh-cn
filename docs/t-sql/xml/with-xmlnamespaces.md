---
description: WITH XMLNAMESPACES
title: WITH XMLNAMESPACES (Transact-SQL)
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WITH_XMLNAMESPACES_TSQL
- WITH XMLNAMESPACES
dev_langs:
- TSQL
helpviewer_keywords:
- adding XML namespaces
- XML namespace declarations [SQL Server]
- clauses [SQL Server], WITH XMLNAMESPACES
- WITH XMLNAMESPACES clause
- declaring XML namespaces
ms.assetid: 3b32662b-566f-454d-b7ca-e247002a9a0b
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 6ec08509389c8b5aadd080245253e289cc23100c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181740"
---
# <a name="with-xmlnamespaces"></a>WITH XMLNAMESPACES
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  声明一个或多个 XML 命名空间。  
  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
WITH XMLNAMESPACES ( <XML namespace declaration item>  
[ { , <XML namespace declaration item> }...] )   
  
<XML namespace declaration item> ::=  
<xml_namespace_uri> AS <xml_namespace_prefix>  
| <XML default namespace declaration item>  
<xml_namespace_uri> ::= <character string literal>  
```  
  
```syntaxsql
  
<xml_namespace_prefix> ::= <identifier>  
```  
  
```syntaxsql
  
<XML default namespace declaration item> ::=  
DEFAULT <xml_namespace_uri>  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 xml_namespace_uri  
 统一资源标识符 (URI)，用于标识正在声明的 XML 命名空间。 xml_namespace_uri 是 SQL 字符串。  
  
 xml_namespace_prefix  
 指定一个要映射并与在 xml_namespace_uri 中指定的命名空间 URI 值关联的前缀。 xml_namespace_prefix 必须为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符。  
  
## <a name="remarks"></a>备注  
 在还包括公用表表达式的语句中使用 WITH XMLNAMESPACES 子句时，WITH XMLNAMESPACES 子句必须位于语句中的公用表表达式的前面。  
  
 下面是在使用 WITH XMLNAMESPACES 子句时所应用的常规语法规则：  
  
-   每个 XML 命名空间声明都必须包含至少一个 XML 默认命名空间声明项。  
  
-   所使用的每个 XML 命名空间前缀都必须是非移植名称 (NCName)，在这样的名称中冒号字符 (:) 不是名称的一部分。  
  
-   不能两次定义同一个命名空间前缀。  
  
-   XML 命名空间前缀和 URI 是区分大小写的。  
  
-   不能声明 XML 命名空间前缀 `xmlns`。  
  
-   除了命名空间 URI `xml` 以及不能为其分配不同前缀的此 URI 以外，不能用其他命名空间覆盖 XML 命名空间前缀 `'http://www.w3.org/XML/1998/namespace'`。  
  
-   当查询正在使用 ELEMENTS XSINIL 指令时，不能重新声明 XML 命名空间前缀 `xsi`。  

-   无需声明“http://www.w3.org/2001/XMLSchema-instance”即可使用 xsi 标准命名空间。 如果未指定，则它由 XML/XPATH 处理器隐式添加，并且只要 xml 文档中正确声明了“http://www.w3.org/2001/XMLSchema-instance”架构，xpath 表达式即可使用 xsi 前缀。

-   URI 字符串值按照当前数据库排序规则代码页进行编码，并且将内部转换为 Unicode。  
  
-   XML 命名空间 URI 将按照用于 xs:anyURI 的 XSD 空格折叠规则进行空格折叠。 另外，不会对 XML 命名空间 URI 值执行实体化和反实体化。  

-   系统将检查 XML 命名空间 URI 中是否有无效的 XML 1.0 字符，如果发现这样的字符（例如，U+0007），将引发错误。  
  
-   XML 命名空间 URI（折叠所有空格后）不能是零长度的字符串，否则会发生“无效的空命名空间 URI”错误。  
  
-   XMLNAMESPACES 关键字保留在 WITH 子句的上下文中。  
  
## <a name="examples"></a>示例  
 有关示例，请参阅[使用 WITH XMLNAMESPACES 将命名空间添加到查询](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XQuery 语言参考 (SQL Server)](../../xquery/xquery-language-reference-sql-server.md)  
  
  
