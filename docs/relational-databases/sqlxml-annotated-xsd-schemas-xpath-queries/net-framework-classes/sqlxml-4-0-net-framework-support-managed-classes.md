---
title: SQLXML 托管类
description: 了解在 Microsoft .NET 框架内公开 SQLXML 4.0 功能的 Microsoft SQLXML 托管类。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- .NET Framework [SQLXML], Managed Classes
- SQL Server .NET Data Provider
- Managed Classes [SQLXML], about managed classes
- providers [SQLXML], SQL Server .NET Data Provider
- data providers [SQLXML], SQL Server .NET Data Provider
- Managed Classes [SQLXML]
- XML [SQLXML]
- SQLXML Managed Classes
- providers [SQLXML], SQLXML Managed Classes
- data providers [SQLXML], SQLXML Managed Classes
- SQLXML, Managed Classes
ms.assetid: 73a5faeb-dabf-4895-acb5-a9651b646065
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f63ac588cf9bdd48ff02343b39d81d250cb6d171
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482968"
---
# <a name="sqlxml-40-net-framework-support---managed-classes"></a>SQLXML 4.0 .NET Framework 支持 - 托管类
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 支持多种功能，例如可用于编写应用程序以便从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例访问 XML 数据、将数据置于 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework 环境中、处理数据以及将更新发送回 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 
  
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 托管类在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework 中公开 SQLXML 4.0 的功能。 利用 SQLXML 托管类，您可以编写 C# 应用程序，以便访问 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例中的 XML 数据，将该数据载入 .NET Framework 环境并进行处理，然后将更新作为 DiffGram 发回 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 以应用此更新。 使用 SQLXML 托管类向 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库应用更新时必须使用映射架构。 有关工作示例，请参阅 [在 .Net 环境中访问 SQLXML 功能](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md)。  
  
 若要配合使用 SQLXML 托管类和 SQLXML 4.0，必须安装 Microsoft Visual Studio。  
  
> [!NOTE]  
>  .NET Framework 包括 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .NET 数据访问接口。 该访问接口可用于从 .NET 环境访问 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]；但是，它只能处理传统 SQL 查询（即除 FOR XML 查询以外的关系数据库查询）。 您不能在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中执行 XML 模板或服务器端 XPath 查询。  

 有关访问和修改 .NET Framework 中的数据以及如何使用 Diffgram 更新表中的数据的信息 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ，请参阅 [在 .NET 环境中访问 SQLXML 功能](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md)。  
  
> [!NOTE]  
>  您还可以编写 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Studio 应用程序，以便使用 XML 大容量加载对 XML 文档执行大容量加载。 有关详细信息，请参阅 [&#40;SQLXML 4.0&#41;执行 XML 数据的大容量加载 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)。 必须在应用程序中添加指向 XML 大容量加载 DLL (Xblkld4.dll) 的引用。 Visual Studio .NET 将为此 COM DLL 自动创建包装库。  
  
  本部分提供了演示如何使用 SQLXML 托管类的示例应用程序 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] ：  
 [&#40;SQLXML 托管类执行 SQL 查询&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md)  
  [使用 ExecuteXMLReader 方法执行 SQL 查询](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-by-using-the-executexmlreader-method.md)  
  [&#40;SQLXML 托管类在客户端处理 XML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/processing-xml-on-the-client-side-sqlxml-managed-classes.md)  
  [&#40;SQLXML 托管类执行 XPath 查询&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-sqlxml-managed-classes.md)  
  [通过命名空间执行 XPath 查询 &#40;SQLXML 托管类&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-with-namespaces-sqlxml-managed-classes.md)  
  [使用 CommandText 属性执行模板文件](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandtext-property.md)  
  [使用 CommandStream 属性执行模板文件](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandstream-property.md)  
  [将 XSL 转换 &#40;SQLXML 托管类&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/applying-an-xsl-transformation-sqlxml-managed-classes.md)  
  

  
  
