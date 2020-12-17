---
description: Reshape Name 属性 - 动态 (ADO)
title: " (ADO) 改变名称 Property-DynamicMicrosoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- Reshape Name property [ADO]
ms.assetid: 690229d1-46cc-42e6-a57d-4438251fe248
author: rothja
ms.author: jroth
ms.openlocfilehash: 1678bcbae00c7d1022bfeffbb72a3e9b1ee63401
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637974"
---
# <a name="reshape-name-property-dynamic-ado"></a>Reshape Name 属性 - 动态 (ADO)
指定 [记录集](./recordset-object-ado.md) 对象的名称。  
  
## <a name="return-values"></a>返回值  
 返回一个 **字符串** 值，该值是 **记录集** 的名称。  
  
## <a name="remarks"></a>备注  
 名称在连接期间或在关闭 **记录集** 之前保持不变。  
  
 **整形名称** 属性主要用于 OLE DB 服务提供商的 [Microsoft 数据定形服务](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)的重新造型功能。 名称必须是唯一的，才能参与重新整理。  
  
 此属性是只读的，但可以在创建 **记录集** 时间接设置。 例如，如果 Shape 命令的子句创建了一个 **记录集** ，并通过使用 **AS** 关键字为其指定了别名，则该别名将分配给 " **改变形状名称** " 属性。 如果未声明别名，则会为 " **调整形状名称** " 属性分配由数据定形服务生成的唯一名称。 如果别名与现有 **记录集** 的名称相同，则在释放其中一个记录集之前，都不会改变 **记录集** 的名称。 可以通过在 ADO 连接的 " **改变名称** " 属性中设置一个唯一的名称来更改默认行为，使其设置为 **True**。 如果需要，设置此属性将为数据定形服务授予更改用户分配的名称的权限，以确保唯一性。 有关重构的详细信息，请参阅 [Microsoft 数据定形服务 OLE DB (ADO 服务提供程序) ](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)。  
  
 当您想要在 Shape 命令中引用 **记录集** 时，或者当您不知道该名称，因为它是由数据定形服务生成的，则可以使用 "重设形状 **名称**" 属性。 在这种情况下，可以通过将命令与 " **改变名称" 名称** 属性所返回的字符串连接在一起，生成一个 SHAPE 命令。  
  
 当 [CursorLocation](./cursorlocation-property-ado.md)属性设置为 **AdUseClient** 时，**改变名称** 是追加到 **Recordset** 对象的 [Properties](./properties-collection-ado.md) collection 的动态属性。  
  
## <a name="applies-to"></a>适用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [适用于 OLE DB (ADO 服务提供商的 Microsoft 数据定形服务) ](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md)   
 [整体形状命令](../../guide/data/shape-commands-in-general.md)   
 [记录集对象 (ADO)](./recordset-object-ado.md)