---
title: SQLXML)  (的 XSL 缓存
description: 了解如何缓存 XSL 样式表并设置 XSL 缓存大小以提高 SQLXML 4.0 中的查询性能。
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- registry keys [SQLXML]
- cache [SQLXML]
- XSL caching [SQLXML]
ms.assetid: 91994142-32f0-4d8d-a8cf-eb0d8b1f1999
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: da5e68eb863ebaa76b6b9901f77d38027c46e1f4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479268"
---
# <a name="xsl-caching-sqlxml-40"></a>XSL 缓存 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  缓存 XSL 样式表可提高性能。 如果 XSL 缓存设置为 ON，则在第一次执行之后，XSL 样式表将保留在内存中；这可以提高后续处理的性能。 默认设置为 ON。  
  
 通过在注册表中添加以下项可以设置 XSL 缓存大小：  
  
```  
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SQLXML4\XSLCacheSize  
```  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoteRegistry](../../../includes/ssnoteregistry-md.md)]  
  
 应根据可用内存和要使用的 XSL 样式表的个数来设置 XSL 缓存大小。 **XSLCacheSize** 大小的默认值为31。 如果 XSL 访问看起来较慢，您可以增加缓存大小；如果内存较少，则可以降低缓存大小。  
  
 为了获得更好的性能，建议您将 **XSLCacheSize** 设置为大于您通常使用的 XSL 样式表的数量。 如果 **XSLCacheSize** 小于您拥有的 xsl 样式表的数目，则会随着 xsl 样式表的数量的增加而降低性能。 **XSLCacheSize** 最大可以设置为128。  
  
 每次使用缓存的 XSL 样式表时，都将检查 XSL 文件的修改时间以确定是否需要刷新该样式表。 其原因在于磁盘副本新于缓存副本。  
  
## <a name="see-also"></a>另请参阅  
 [SQLXML 4.0 &#40;模板缓存&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/template-caching-sqlxml-4-0.md)   
 [&#40;SQLXML 4.0&#41;的架构缓存 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/schema-caching-sqlxml-4-0.md)  
  
  
