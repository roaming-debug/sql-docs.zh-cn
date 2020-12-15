---
title: " (SQLXML) 缓存模板、XSL 和架构"
description: 查看有关在 SQLXML 4.0 中缓存模板、XSL 和架构的信息。
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, caching
- cache [SQLXML]
- memory [SQLXML]
ms.assetid: 80b4fa79-243f-442c-9f22-74ad66186501
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 96dddb725447b8dfaecb41a85489e5071459faa0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97415050"
---
# <a name="caching-templates-xsl-and-schemas-sqlxml-40"></a>缓存模板、XSL 和架构 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  为了提高性能，[!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 支持对模板、XSL 和架构进行缓存。  
  
 除 https://或 ftp://位置) 的文件外，所有架构、模板和 XSL 文件 (。 在运行进程的过程中，缓存的文件保留在内存中。 在进程退出后，所有缓存内容都将丢失。 因此，如果是每个查询都运行一个进程，则缓存带来的好处可能并不明显。  
  
 本节中的主题提供有关缓存的详细信息。  
  
## <a name="in-this-section"></a>本节内容  
 [SQLXML 4.0 &#40;模板缓存&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/template-caching-sqlxml-4-0.md)  
 介绍模板缓存并且提供一个用于模板缓存的注册表项。  
  
 [XSL 缓存 &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/xsl-caching-sqlxml-4-0.md)  
 介绍 XSL 缓存并且提供一个用于 XSL 缓存的注册表项。  
  
 [&#40;SQLXML 4.0&#41;的架构缓存 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/schema-caching-sqlxml-4-0.md)  
 讨论与架构缓存相关的 SQLXML 并行安装问题，并且提供用于架构缓存的注册表项。  
  
  
