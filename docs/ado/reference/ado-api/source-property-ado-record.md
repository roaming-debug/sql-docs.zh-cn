---
description: Source 属性（ADO 记录）
title: " (ADO 记录) 的源属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::Source
- _Record::PutRefSource
- _Record::GetSource
- _Record::put_Source
- _Record::putref_Source
- _Record::get_Source
helpviewer_keywords:
- Source property [ADO Record]
ms.assetid: 2c18279e-6f35-4af0-b12e-8f1543d9ed20
author: rothja
ms.author: jroth
ms.openlocfilehash: 850f3d493962db94671bb1f2e8ef65c1b2120f51
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051398"
---
# <a name="source-property-ado-record"></a>Source 属性（ADO 记录）
指示 [记录](./record-object-ado.md)所表示的数据源或对象。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **变量** 值，该值指示由 **记录** 表示的实体。  
  
## <a name="remarks"></a>备注  
 **Source** 属性返回 **Record** 对象 [Open](./open-method-ado-record.md)方法的 *source* 参数。 它可以包含绝对或相对的 URL 字符串。 可以使用绝对 URL，而无需将 [ActiveConnection](./activeconnection-property-ado.md) 属性设置为直接打开 **记录** 对象。 在这种情况下，将创建一个隐式 **连接** 对象。  
  
 **Source** 属性还可以包含对已打开的 **记录集** 的引用，该记录集打开表示 **记录集中** 的当前行的 **记录** 对象。  
  
 **Source** 属性还可以包含对 [命令](./command-object-ado.md)对象的引用，该对象将从提供程序返回一行数据。  
  
 如果同时设置了 **ActiveConnection** 属性，则 **源** 属性必须指向在该连接的作用域中存在的某个对象。 例如，在树结构命名空间中，如果 **源** 属性包含绝对 URL，则它必须指向在连接字符串中由 URL 标识的节点范围内的节点。 如果 **Source** 属性包含相对 URL，则会在 **ActiveConnection** 属性设置的上下文中对其进行验证。  
  
 当 **记录对象处于** 关闭状态时，**源** 属性是读/写的，并且在 **记录** 对象处于打开状态时为只读。  
  
> [!NOTE]
>  使用 http 方案的 Url 将自动调用 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [绝对和相对 url](../../guide/data/absolute-and-relative-urls.md)。  
  
## <a name="applies-to"></a>应用于  
 [记录对象 (ADO)](./record-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (ADO 错误的源属性) ](./source-property-ado-error.md)   
 [Source 属性（ADO 记录集）](./source-property-ado-recordset.md)