---
description: 记录和提供程序提供的字段
title: 记录和 Provider-Supplied 字段 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- records-provided fields [ADO]
- provider-supplied fields [ADO]
ms.assetid: 77f95e0a-0cf2-411a-a792-593f77330fbd
author: rothja
ms.author: jroth
ms.openlocfilehash: a64168a8a1fec81d47c337978ea75664fec0a10c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037107"
---
# <a name="records-and-provider-supplied-fields"></a>记录和提供程序提供的字段
打开 [记录](../../../ado/reference/ado-api/record-object-ado.md) 对象时，它的源可以是打开的 [记录集](../../../ado/reference/ado-api/recordset-object-ado.md)的当前行、绝对 url 或与打开的 [连接](../../../ado/reference/ado-api/connection-object-ado.md) 对象关联的相对 url。  
  
 如果从记录 **集中** 打开 **记录**，则 "**记录** 对象 [字段](../../../ado/reference/ado-api/fields-collection-ado.md)" 集合将包含 **记录集中** 的所有字段，以及由基础提供程序添加的任何字段。  
  
 提供程序可以插入作为 **记录** 的补充特性的其他字段。 因此，**记录** 中可能包含不在 **记录集中** 的唯一字段，也可能是从 **记录集** 的其他行派生的任何 **记录**。  
  
 例如，从电子邮件数据源派生的 **记录集** 的所有行都可能包含列，如 From、To 和 Subject。 从该 **记录集** 派生的 **记录** 将具有相同的字段。 不过， **记录** 还可能具有该 **记录** 所表示的特定消息的其他字段，例如附件和抄送 (抄送) 。  
  
 尽管记录 **集** 的 **记录** 对象和当前行具有相同的字段，但它们是不同的，因为 **记录** 和 **记录集** 对象具有不同的方法和属性。  
  
 **记录** 和 **记录集** 共同保存的字段可以在任一对象上进行修改。 但是，该字段不能在 **Record** 对象上删除，不过，基础提供程序可能支持将字段设置为 null。  
  
 打开 **记录** 后，可以通过编程方式添加字段。 您还可以删除已添加的字段，但是不能从原始 **记录集中** 删除字段。  
  
 您还可以从 URL 直接打开 **记录** 对象。 在这种情况下，添加到 **记录** 的字段取决于基础提供程序。 目前，大多数提供程序会添加一组字段来描述由 **记录** 表示的实体。 如果实体包含字节流（如简单文件），通常可以从 **记录** 打开一个 [流](../../../ado/reference/ado-api/stream-object-ado.md)对象。  
  
## <a name="special-fields-for-document-source-providers"></a>文档源提供程序的特殊字段  
 称为 *文档源提供程序* 的一种特殊类提供程序管理文件夹和文档。 当 **Record** 对象表示文档或 **记录集** 对象表示文档的文件夹时，文档源提供程序将使用一组唯一的字段（描述文档的特征，而不是实际的文档本身）来填充这些对象。 通常，一个字段包含对表示文档的 **流** 的引用。  
  
 这些字段构成了资源 **记录** 或 **记录集** ，并为在 [附录 a：提供程序](../../../ado/guide/appendixes/appendix-a-providers.md)中支持这些字段的特定提供程序列出。  
  
 两个常量为资源 **记录** 或 **记录集** 的 **字段** 集合编制索引，以检索一对常用字段。 " **字段** 对象 [值](../../../ado/reference/ado-api/value-property-ado.md) " 属性返回所需的内容。  
  
-   使用 **adDefaultStream** 常量访问的字段包含与 **Record** 或 **Recordset** 对象关联的默认流。 提供程序将默认流分配给对象。  
  
-   通过 **adRecordURL** 常量访问的字段包含标识该文档的绝对 URL。  
  
 文档源提供程序不支持 **记录** 和 **字段** 对象的 [Properties](../../../ado/reference/ado-api/properties-collection-ado.md)集合。 此类对象的 **Properties** 集合的内容为 null。  
  
 文档源提供程序可以添加特定于提供程序的属性（如 **Datasource 类型** ），以确定它是否为文档源提供程序。 有关如何确定提供程序类型的详细信息，请参阅提供程序文档。  
  
## <a name="resource-recordset-columns"></a>资源记录集列  
 *资源记录集* 由以下列组成。  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|RESOURCE_PARSENAME|AdVarWChar|只读。 指示资源的 URL。|  
|RESOURCE_PARENTNAME|AdVarWChar|只读。 指示父记录的绝对 URL。|  
|RESOURCE_ABSOLUTEPARSENAME|AdVarWChar|只读。 指示资源的绝对 URL，该 URL 是 PARENTNAME 和 PARSENAME 的连接。|  
|RESOURCE_ISHIDDEN|AdBoolean|如果资源处于隐藏状态，则为 True。 除非创建行集的命令显式选择 RESOURCE_ISHIDDEN 为 True 的行，否则不会返回任何行。|  
|RESOURCE_ISREADONLY|AdBoolean|如果资源是只读的，则为 True。 尝试通过 DBBINDFLAG_WRITE 打开此资源，并将因 DB_E_READONLY 而失败。 即使仅打开了资源以进行读取，也可以编辑此属性。|  
|RESOURCE_CONTENTTYPE|AdVarWChar|指示可能使用文档的示例，例如律师的简短。 这可能对应于用于创建文档的 Office 模板。|  
|RESOURCE_CONTENTCLASS|AdVarWChar|指示文档的 MIME 类型，指示格式（如 " `text/html` "）。|  
|RESOURCE_CONTENTLANGUAGE|AdVarWChar|指示存储内容所用的语言。|  
|RESOURCE_CREATIONTIME|adFileTime|只读。 指示包含创建资源的时间的 FILETIME 结构。 以协调世界时 (UTC) 格式报告时间。|  
|RESOURCE_LASTACCESSTIME|AdFileTime|只读。 指示一个 FILETIME 结构，该结构包含上次访问该资源的时间。 时间采用 UTC 格式。 如果提供程序不支持此时间成员，则 FILETIME 成员为零。|  
|RESOURCE_LASTWRITETIME|AdFileTime|只读。 指示一个 FILETIME 结构，该结构包含上次写入资源的时间。 时间采用 UTC 格式。 如果提供程序不支持此时间成员，则 FILETIME 成员为零。|  
|RESOURCE_STREAMSIZE|asUnsignedBigInt|只读。 指示资源的默认流的大小（以字节为单位）。|  
|RESOURCE_ISCOLLECTION|AdBoolean|只读。 如果资源是集合（如目录），则为 True。 如果资源是一个简单文件，则为 False。|  
|RESOURCE_ISSTRUCTUREDDOCUMENT|AdBoolean|如果资源是结构化文档，则为 True。 如果资源不是结构化文档，则为 False。 它可以是一个集合或一个简单的文件。|  
|DEFAULT_DOCUMENT|AdVarWChar|只读。 指示此资源包含文件夹的默认简单文档的 URL 或结构化文档。 在从资源请求默认流时使用。 对于简单文件，此属性为空白。|  
|CHAPTERED_CHILDREN|AdChapter|只读。 可选。 指示包含资源子项的行集的章节。  (*用于 Internet 发布的 OLE DB 提供程序* 不使用此列。 ) |  
|RESOURCE_DISPLAYNAME|AdVarWChar|只读。 指示资源的显示名称。|  
|RESOURCE_ISROOT|AdBoolean|只读。 如果资源是集合或结构化文档的根，则为 True。|  
  
## <a name="see-also"></a>另请参阅  
 [ (ADO 记录对象) ](../../../ado/reference/ado-api/record-object-ado.md)   
 [附录 A：提供程序](../../../ado/guide/appendixes/appendix-a-providers.md)
