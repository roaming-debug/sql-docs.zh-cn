---
title: '使用 SQLXMLOLEDB 提供程序 (SQLXML) '
description: 查看有关在 ADO 应用程序中使用 SQLXMLOLEDB 提供程序特定属性的信息。
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- sample applications [SQLXML]
- SQLXMLOLEDB Provider, samples
- ClientSideXML property
ms.assetid: fbcefac5-29c9-478b-b0e0-d510b593f446
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bbecbe6507754412ffd4a95fc60b077a92fc669e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97414982"
---
# <a name="using-the-sqlxmloledb-provider-sqlxml-40"></a>使用 SQLXMLOLEDB 访问接口 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  本节中的主题提供 ADO 示例应用程序，这些应用程序说明 SQLXMLOLEDB 访问接口特有的属性的用法。  
  
## <a name="application-requirements-for-sqlxmloledb-40-provider"></a>SQLXMLOLEDB 4.0 访问接口的应用程序要求  
 若要创建使用 SQLXMLOLEDB 4.0 的工作示例，必须执行以下操作：  
  
1.  创建 Microsoft Visual Basic .exe 应用程序，并添加以下引用之一：  
  
    -   Microsoft ActiveX 数据对象2.6 库  
  
    -   Microsoft ActiveX 数据对象2.7 库  
  
    -   Microsoft ActiveX Data Objects 2.8 Library  
  
2.  部署和安装 SQLXML 4.0 和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client。  

     有关详细信息，请参阅 [SQLXML 4.0 编程概念](../../../relational-databases/sqlxml/sqlxml-4-0-programming-concepts.md) 和 [安装 SQL Server Native Client](../../../relational-databases/native-client/applications/installing-sql-server-native-client.md)。  
  
## <a name="in-this-section"></a>本节内容  
 [&#40;SQLXMLOLEDB 提供程序执行 SQL 查询&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-sql-queries-sqlxmloledb-provider.md)  
 说明如何使用 ClientSideXML 和 xml 根属性来执行 SQL 查询。  
  
 [&#40;SQLXMLOLEDB 提供程序执行包含 SQL 查询的模板&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-templates-that-contain-sql-queries-sqlxmloledb-provider.md)  
 说明如何使用 ClientSideXML 属性。  
  
 [&#40;SQLXMLOLEDB 提供程序执行 XPath 查询&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-sqlxmloledb-provider.md)  
 阐释 ClientSideXML、基路径和映射架构属性的用法。  
  
 [通过命名空间 &#40;SQLXMLOLEDB 提供程序执行 XPath 查询&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-with-namespaces-sqlxmloledb-provider.md)  
 说明如何针对已限定命名空间的架构进行查询。  
  
 [&#40;SQLXMLOLEDB 提供程序执行包含 XPath 查询的模板&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-templates-that-contain-xpath-queries-sqlxmloledb-provider.md)  
 说明如何使用 ClientSideXML、基路径和映射架构属性执行包含 SQL 查询的模板。  
  
 [&#40;SQLXMLOLEDB 提供程序应用 XSL 转换&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/applying-an-xsl-transformation-sqlxmloledb-provider.md)  
 说明如何使用 ClientSideXML 和 xsl 属性来应用 XSL 转换。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client 的系统要求](../../../relational-databases/native-client/system-requirements-for-sql-server-native-client.md)  
  
  
