---
title: SQLXML) 的 XSD 批注 (
description: 查看 SQL Server 2005 (1.x) 中引入的 (SQLXML 4.0) 的 XSD 批注列表，并对 SQL Server 2000 (3.x) 中引入的 XDR 批注进行比较。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- annotated XSD schemas, annotations listed
- XSD schemas [SQLXML], annotations
ms.assetid: c62a6785-8d66-40a2-9c5d-80c73d600a3b
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3bbf7245ec16a9946789eb28b444981822426e7b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462948"
---
# <a name="xsd-annotations-sqlxml-40"></a>XSD 批注 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  下表列出在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中引入的 XSD 批注，并且将这些批注与已在 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 中引入的 XDR 批注进行比较。  
  
|XSD 批注|说明|主题链接|XDR 批注|  
|--------------------|-----------------|----------------|--------------------|  
|**sql:encode**|在将某一 XML 元素或属性映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] BLOB 列时，允许请求某一引用 URI。 此 URI 可以在以后使用，以便返回 BLOB 数据。|[使用 sql：加码 &#40;SQLXML 4.0&#41;来请求对 BLOB 数据的 URL 引用 ](../../relational-databases/sqlxml-annotated-xsd-schemas-using/requesting-url-references-to-blob-data-using-sql-encode-sqlxml-4-0.md)|**url 编码**|  
|**sql： guid**|允许您指定是要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成的 GUID 值，还是使用在该列的 updategram 中提供的值。|[使用 sql:identity 和 sql:guid 批注](../../relational-databases/sqlxml-annotated-xsd-schemas-using/using-the-sql-identity-and-sql-guid-annotations.md)|不支持|  
|**sql:hide**|隐藏在最终 XML 文档的架构中指定的元素或属性。|[使用 sql:hide 隐藏元素和属性](../../relational-databases/sqlxml-annotated-xsd-schemas-using/hiding-elements-and-attributes-by-using-sql-hide.md)|不支持|  
|**sql:identity**|可对映射到 IDENTITY 类型数据库列的任何节点指定。 为此批注指定的值定义如何更新数据库中相应 IDENTITY 类型的列。|[使用 sql:identity 和 sql:guid 批注](../../relational-databases/sqlxml-annotated-xsd-schemas-using/using-the-sql-identity-and-sql-guid-annotations.md)|不支持|  
|**sql:inverse**|指示 updategram 逻辑反转使用指定的父子关系的解释 **\<sql:relationship>** 。|[在 sql： relationship 上指定 sql：反向特性 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-the-sql-inverse-attribute-on-sql-relationship-sqlxml-4-0.md)|不支持|  
|**sql:is-constant**|创建不映射到任何表的 XML 元素。 该元素出现在查询输出中。|[使用 sql 创建常量元素：是常量 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-constant-elements-using-sql-is-constant-sqlxml-4-0.md)|相同|  
|**sql:key-fields**|允许规定唯一标识表中的行的列。|[使用 sql：键字段 &#40;SQLXML 4.0&#41;标识键列 ](../../relational-databases/sqlxml-annotated-xsd-schemas-using/identifying-key-columns-using-sql-key-fields-sqlxml-4-0.md)|相同|  
|**sql:limit-field**<br /><br /> **sql:limit-value**|允许限制基于限制值返回的值。|[使用 sql： limit 字段和 sql： limit-value &#40;SQLXML 4.0&#41;筛选值 ](../../relational-databases/sqlxml-annotated-xsd-schemas-using/filtering-values-using-sql-limit-field-and-sql-limit-value-sqlxml-4-0.md)|相同|  
|**sql:mapped**|允许从结果中排除架构项。|[使用 sql：映射 &#40;SQLXML 4.0&#41;从生成的 XML 文档中排除架构元素 ](../../relational-databases/sqlxml-annotated-xsd-schemas-using/excluding-schema-elements-from-the-xml-document-using-sql-mapped.md)|**映射字段**|  
|**sql:max-depth**|允许您指定在架构中指定的递归关系的深度。|[使用 sql:max-depth 指定递归关系中的深度](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-depth-in-recursive-relationships-by-using-sql-max-depth.md)|不支持|  
|**sql:overflow-field**|标识包含溢出数据的数据库列。|[使用 sql：溢出字段检索未用完的数据 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/retrieving-unconsumed-data-using-the-sql-overflow-field-sqlxml-4-0.md)|相同|  
|**sql:prefix**|创建有效的 XML ID、IDREF 和 IDREFS。 将某一字符串置于 ID、IDREF 和 IDREFS 的值之前。|[使用 sql： prefix 创建有效的 ID、IDREF 和 IDREFS 类型属性 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-valid-id-idref-and-idrefs-type-attributes-using-sql-prefix-sqlxml-4-0.md)|相同|  
|**sql:relationship**|指定 XML 元素之间的关系。 **父**、**子**、**父键** 和 **子键** 特性用于建立关系。|[使用 sql： relationship 指定关系 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-relationships-using-sql-relationship-sqlxml-4-0.md)|属性名称不同：<br /><br /> **键关系**<br /><br /> **外部关系**<br /><br /> **key**<br /><br /> **外键**|  
|**sql:use-cdata**|允许指定要用于 XML 文档中的某些元素的 CDATA 部分。|[使用 sql： use-CDATA 创建 CDATA 节 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-cdata-sections-using-sql-use-cdata-sqlxml-4-0.md)|相同|  
  
> [!NOTE]  
>  XSD native **targetNamespace** 属性替换 XDR 映射架构中引入的 **目标命名空间** 批注 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 。  
  
## <a name="see-also"></a>另请参阅  
 [使用 targetNamespace 属性指定目标命名空间 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-a-target-namespace-using-the-targetnamespace-attribute-sqlxml-4-0.md)  
  
  
