---
title: SQLXML 4.0 中的 xml 数据类型支持
description: 了解 SQLXML 4.0 如何识别 xml 数据类型的实例并实现对它的支持。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, xml data type support
- xml data type [SQL Server], SQLXML
ms.assetid: 9a6f5ad8-4a8f-4de7-ac17-81d5ccf78459
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0c2400239b5ad8d19733b1fa74f7750c6edc4382
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479178"
---
# <a name="xml-data-type-support-in-sqlxml-40"></a>SQLXML 4.0 中的 xml 数据类型支持
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  从开始 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持使用 **xml** 数据类型的 xml 类型化数据。 本主题提供有关 SQLXML 4.0 如何识别 **xml** 数据类型的实例并实现对这些实例的支持的信息。  
  
## <a name="working-with-xml-data-types"></a>使用 xml 数据类型  
 若要详细了解如何使用实现 **xml** 数据类型列的 SQL 表，提供了以下示例：  
  
|任务|示例|主题|  
|----------|-------------|-----------|  
|如何在 XML 视图中映射和包含 **xml** 列|“将 XML 元素映射到 XML 数据类型列”|[XSD 元素和属性到表和列的默认映射 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/default-mapping-of-xsd-elements-and-attributes-to-tables-and-columns-sqlxml-4-0.md)|  
|如何使用 updategram 将数据插入到 **xml** 列中|“将数据插入到 XML 数据类型列”|[使用 XML Updategram 插入数据 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md)|  
|将 XML 数据大容量加载到 **xml** 列中|“在 xml 数据类型列中执行大容量加载”|[XML 大容量加载示例 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/xml-bulk-load-examples-sqlxml-4-0.md)|  
  
## <a name="guidelines-and-limitations"></a>准则和限制  
  
-   **\<xsd:any>** 无法映射到包含 **xml** 数据类型的列。 对于此方案，SQLXML 中的支持通过 **sql：溢出字段** 批注提供。 另一种解决方法是将 **xml** 数据类型字段映射为 **xsd： anyType** 的元素。 上表中引用的“将 XML 元素映射到 XML 数据类型列”示例介绍了这种解决办法。  
  
-   不支持将 XPath 查询到 **xml** 数据类型列的内容。  
  
-   如果在不支持 xml 数据类型列的批注中使用 **xml** 数据类型列 (如 **sql： relationship** 和 **sql：**) 或允许，将导致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实现 SQLXML 4.0 的中间层组件不会捕获错误。 其原因在于 SQLXML 不要求 SQL 类型信息。 这类似于其他数据类型（如 BLOB 和二进制类型）的 SQLXML 的行为。  
  
-   仅 XSD 架构支持映射 **xml** 列。 XDR 架构不支持映射 **xml** 列。  
  
-   SQLXML 4.0 依赖于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中提供的 XML 分析支持。 **Xml** 列可以映射为类型化的 xml 或非类型化的 xml。 在任一种情况下，SQLXML 4.0 都不验证输入 XML。  如果输入 XML 无效或格式不正确，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将向 SQLXML 报告此情况，并将服务器返回的任何相关的错误信息传播给用户。  
  
-   SQLXML 4.0 依赖于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中提供的对于 DTD 的有限支持。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 允许在 **xml** 数据类型的数据中使用内部 DTD，该数据可用于提供默认值，并将实体引用替换为其扩展的内容。 SQLXML 将 XML 数据“按原样”（包括内部 DTD）传递到服务器。 可以通过使用第三方工具将 DTD 转换为 XML 架构 (XSD)，然后使用内联 XSD 架构将数据加载到数据库中。  
  
-   SQLXML 4.0 不保留 XML 声明处理指令 (例如，基于的行为 ) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 相反，XML 声明被视为对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] xml 分析器的指令，并且在将数据转换为 **xml** 数据类型后， (版本、编码和独立) 的属性将丢失。 XML 数据在内部存储为 UCS-2。 XML 实例中的所有其他处理指令都将保留;它们在 **xml** 列中是允许的，并且可以由 SQLXML 支持。  
  
## <a name="see-also"></a>另请参阅  
 [XML 数据 (SQL Server)](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
