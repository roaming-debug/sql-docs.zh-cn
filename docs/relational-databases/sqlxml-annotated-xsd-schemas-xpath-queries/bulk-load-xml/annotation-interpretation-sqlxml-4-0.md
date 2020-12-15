---
title: SQLXML) 的批注解释 (
description: 了解 SQLXML 4.0 中的 XML 大容量加载如何解释 XSD 和 XDR 架构中的批注。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- annotated XSD schemas, XML Bulk Load
- XML Bulk Load [SQLXML], annotation intepretations
- annotations [SQLXML]
- bulk load [SQLXML], annotation interpretations
- annotated XDR schemas, XML Bulk Load
ms.assetid: 1c46bdb6-2812-4a13-b60b-7101c04b299f
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9763f36a2a2d4678ec76f77cec65bc68d5cbef85
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462918"
---
# <a name="annotation-interpretation-sqlxml-40"></a>批注解释 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  本节中的主题介绍 XML 大容量加载如何解释 XSD 架构中的批注。 这里描述的行为也适用于 XDR 架构中的批注。  
  
> [!NOTE]  
>  这些主题中的信息仅说明了 XML 大容量加载在其处理过程中使用的批注。 有关 SQLXML 4.0 支持的 XSD 架构的完整批注列表，请参阅 [在 Xsd 架构中使用批注 &#40;sqlxml 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-using/using-annotations-in-xsd-schemas-sqlxml-4-0.md)。 有关 XDR 架构支持的批注的列表，请参阅 [SQLXML 4.0&#41;中 &#40;弃用的带批注 XDR 架构 ](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md)。  
  
## <a name="in-this-section"></a>本节内容  
 [sql：关系和键排序规则 &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-sql-relationship-and-key-ordering-rule.md)  
 说明如何在 XML 大容量加载中解释 **sql： relationship** 批注。  
  
 [sql：映射 &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-sql-mapped.md)  
 说明如何在 XML 大容量加载中解释 **sql：映射** 的批注。  
  
 [sql：限制字段和 sql： &#40;SQLXML 4.0&#41;的限制值 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-sql-limit-field-and-sql-limit-value.md)  
 说明如何在 XML 大容量加载中解释 **sql： limit 字段** 和 **sql： limit 值** 批注。  
  
 [sql：溢出-字段 &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-sql-overflow-field.md)  
 说明如何在 XML 大容量加载中解释 **sql：溢出** 批注。  
  
 [SQLXML 4.0&#41;的其他批注 &#40;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-other-annotations.md)  
 介绍 XML 大容量加载中如何解释以下批注： **sql： id-prefix**、 **sql： use-cdata**、 **sql： url 编码**、 **sql： is 映射-架构**、 **sql：键字段**。  
  
  
