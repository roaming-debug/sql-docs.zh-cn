---
title: SQLXML 4.0 编程概念
description: 查看有关 SQLXML 4.0 中使用的编程概念的信息。
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, about SQLXML
- SQLXML
ms.assetid: 5a11cda2-b8a3-4453-848f-641afdaa7024
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2699c938d13750e954a4171da610c6a40b74f141
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97429559"
---
# <a name="sqlxml-40-programming-concepts"></a>SQLXML 4.0 编程概念
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  SQLXML 3.0 作为 Web 版本提供，以提供附加客户端 XML 功能和现有功能的增强功能，例如带批注的 XSD 架构、XML 大容量加载、Web 服务 (SOAP) 支持和 updategram。  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 引入了 SQLXML 4.0，它继续提供与 SQLXML 3.0 相同的功能，另外还提供附加的更新以接纳随 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 引入的新功能。  
  
 本部分提供有关 SQLXML 4.0 的信息。  
  
 [SQL Server 中未安装 SQLXML](../../relational-databases/sqlxml/sqlxml-is-not-installed-in-sql-server.md)  
 说明如何安装 SQLXML 4.0。  
  
 [SQLXML 4.0 SP1 中的新增功能](../../relational-databases/sqlxml/what-s-new-in-sqlxml-4-0-sp1.md)  
 介绍 SQLXML 4.0 中的更新和增强功能，并提供指向本文档中相关主题的链接。  
  
 [使用 ADO 执行 SQLXML 4.0 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
 介绍如何将 ADO 用于 SQLXML 查询。 相对以前版本而言，ADO 在 SQLXML 4.0 中更为重要。  
  
 [SQLXML 4.0 中的 xml 数据类型支持](../../relational-databases/sqlxml/xml-data-type-support-in-sqlxml-4-0.md)  
 介绍对 SQLXML 4.0 中新增的 xml 数据类型的支持。  
  
 [运行 SQLXML 示例的要求](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)  
 介绍从所提供的 SQLXML 示例创建工作示例的要求。  
  
 [&#40;SQLXML 4.0&#41;的客户端和服务器端格式设置 ](../../relational-databases/sqlxml/formatting/client-side-and-server-side-formatting-sqlxml-4-0.md)  
 提供有关客户端和服务器端格式设置的信息以及二者的比较，包括用于构造 XML 文档的 FOR XML 命令。  
  
 [SQLXML 4.0 中的带批注的 XSD 架构](../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xsd-schemas-in-sqlxml-4-0.md)  
 提供有关使用 SQLXML 4.0 中带批注的 XSD 架构的信息。 同时提供有关用于早期应用程序的带批注 XDR 架构的信息。  
  
 [在 SQLXML 4.0 中使用 XPath 查询](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/using-xpath-queries-in-sqlxml-4-0.md)  
 介绍如何使用 XPath 语言的子集查询由带批注的 XSD 架构创建的 XML 视图，并提供了示例。  
  
 [使用 Updategram 修改 SQLXML 4.0 中的数据](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/using-updategrams-to-modify-data-in-sqlxml-4-0.md)  
 提供有关 updategram 的信息，它通过处理由带批注的 XSD（或 XDR）架构提供的 XML 视图来修改数据库中的数据。  
  
 [对 XML 数据执行大容量加载 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)  
 介绍如何在 SQLXML 4.0 中大容量加载 XML。  
  
 [SQLXML 4.0 数据访问组件](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/sqlxml-4-0-data-access-components-sqlxmloledb-provider.md)  
 提供 SQLXMLOLEDB 访问接口的说明，并提供指向其他 SQLXML 4.0 数据访问组件的链接。  
  
 [SQLXML 4.0 .NET Framework 支持](../sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-4-0-net-framework-support-managed-classes.md)  
 介绍 SQLXML 4.0 对 .NET Framework 的支持。  
  
 [&#40;SQLXML 4.0&#41;缓存模板、XSL 和架构 ](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/caching-templates-xsl-and-schemas-sqlxml-4-0.md)  
 介绍在 SQLXML 中提供的用于提高性能的缓存功能。  
  
 [SQLXML 4.0 安全注意事项](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/sqlxml-4-0-security-considerations.md)  
 讨论与 SQLXML 4.0 相关的安全问题。  
  
 [SQLXML 4.0 的准则和限制](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/guidelines-and-limitations-of-sqlxml-4-0.md)  
 列出使用 SQLXML 4.0 时要记住的问题。  
  
## <a name="see-also"></a>另请参阅  
 [XML 数据 (SQL Server)](../../relational-databases/xml/xml-data-sql-server.md)  
  
