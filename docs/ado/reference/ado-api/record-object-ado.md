---
description: 记录对象 (ADO)
title: ADO)  (记录对象 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Record
helpviewer_keywords:
- Record object [ADO]
ms.assetid: db83ed2c-a8e3-460c-8682-64667e4d5d01
author: rothja
ms.author: jroth
ms.openlocfilehash: b6064030bd26ab6abb0bb003b2fcf26e72c4f24d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166747"
---
# <a name="record-object-ado"></a>记录对象 (ADO)
表示 [记录集](./recordset-object-ado.md) 或数据提供程序中的行，或由半结构化数据访问接口（如文件或目录）返回的对象。  
  
## <a name="remarks"></a>备注  
 **Record** 对象表示一行数据，并与一行 **记录集** 有一些概念上的相似之处。 根据提供程序的功能，可以直接从提供程序（而不是单行 **记录集**）返回 **记录** 对象，例如，当执行仅选择一行的 SQL 查询时。 或者，可以直接从 **记录集** 对象获取 **记录** 对象。 或者， **记录** 可以直接从提供程序返回到半结构化数据，例如 Microsoft Exchange OLE DB 提供程序。  
  
 您可以通过 **record** 对象上的 [字段](./fields-collection-ado.md)集合来查看与 **record** 对象相关联的字段。 ADO 允许对象值列，包括 **记录** 对象的 **Fields** 集合中的 **记录集**、 **SafeArray** 和标量值。  
  
 如果 **Record** 对象表示 **记录集中** 的某行，则可以使用 [Source](./source-property-ado-record.md)属性返回到该原始 **记录集**。  
  
 **记录** 对象还可以由半结构化数据提供程序使用，例如用于 [Internet 发布的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)，以及为树状结构命名空间建模。 树中的每个节点都是具有关联列的 **记录** 对象。 列可以表示该节点的属性和其他相关信息。 **Record** 对象可以表示叶节点和树结构中的非叶节点。 非叶节点具有其他节点作为其内容，但叶节点没有此类内容。 叶节点通常包含数据的二进制流，而非叶节点还可能具有与其关联的默认二进制流。 **记录** 对象的属性标识节点的类型。  
  
 **Record** 对象还表示用于导航分层组织的数据的另一种方法。 可以创建 **记录** 对象来表示大型树结构中特定子树的根，并可以打开新的 **记录** 对象来表示子节点。  
  
 资源 (例如，文件或目录) 可以通过绝对 URL 来唯一标识。 当使用绝对 URL 打开 **记录** 时，将隐式创建 [连接](./connection-object-ado.md)对象并将其设置为 **record** 对象。 可以通过 [ActiveConnection](./activeconnection-property-ado.md)属性将 **连接** 对象显式设置为 **Record** 对象。 使用 **连接** 对象可以访问的文件和目录定义了 **记录** 操作可能发生的 *上下文*。  
  
 **记录** 对象上的数据修改和导航方法还接受一个相对 url，该 url 将使用绝对 Url 或 **连接** 对象上下文作为起始点查找资源。  
  
> [!NOTE]
>  使用 http 方案的 Url 将自动调用 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [绝对和相对 url](../../guide/data/absolute-and-relative-urls.md)。  
  
 **连接** 对象与每个 **Record** 对象相关联。 因此， **记录** 对象操作可以通过调用 **连接** 对象事务方法来成为事务的一部分。  
  
 **Record** 对象不支持 ADO 事件，因此将不会响应通知。  
  
 对于 **Record** 对象的方法和属性，您可以执行以下操作：  
  
-   设置或返回具有 [ActiveConnection](./activeconnection-property-ado.md)属性的关联 **连接** 对象。  
  
-   使用 [Mode](./mode-property-ado.md) 属性指示访问权限。  
  
-   返回目录的 URL （如果有），其中包含具有 [ParentURL](./parenturl-property-ado.md)属性的 **记录** 所表示的资源。  
  
-   指示用 [Source](./source-property-ado-record.md)属性派生 **记录** 的绝对 url、相对 url 或 **记录集**。  
  
-   用 [State](./state-property-ado.md)属性指示 **记录** 的当前状态。  
  
-   通过 RecordType 属性指示 **记录** 的  -  *简单*、*集合* 或 *结构化文档* 的类型 [](./recordtype-property-ado.md)。  
  
-   使用 [Cancel](./cancel-method-ado.md) 方法停止执行异步操作。  
  
-   使用 [Close](./close-method-ado.md)方法将 **记录** 与数据源解除关联。  
  
-   使用 [CopyRecord](./copyrecord-method-ado.md)方法将 **记录** 表示的文件或目录复制到另一个位置。  
  
-   删除使用 [DeleteRecord](./deleterecord-method-ado.md)方法由 a **记录** 表示的文件或目录和子目录。  
  
-   使用 [GetChildren](./getchildren-method-ado.md)方法打开一个 **记录集**，其中包含的行表示由 **记录** 表示的实体的子目录和文件。  
  
-   使用 [MoveRecord](./moverecord-method-ado.md)方法将 (重命名) 文件或目录和子目录中的一个 **记录** 表示为另一个位置。  
  
-   将 **记录** 与现有数据源关联，或使用 [Open](./open-method-ado-record.md) 方法创建新的文件或目录。  
  
 **记录** 对象对于脚本是安全的。  
  
 本部分包含以下主题。  
  
-   [记录对象属性、方法和事件](./record-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [字段集合 (ADO) ](./fields-collection-ado.md)   
 [ADO)  (属性集合 ](./properties-collection-ado.md)   
 [记录和流](../../guide/data/records-and-streams.md)   
 [记录集对象 (ADO)](./recordset-object-ado.md)