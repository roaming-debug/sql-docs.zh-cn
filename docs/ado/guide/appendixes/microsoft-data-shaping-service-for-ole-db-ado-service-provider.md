---
description: '适用于 OLE DB (ADO 服务提供商的 Microsoft 数据定形服务) '
title: 适用于 OLE DB (ADO 服务提供商的 Microsoft 数据定形服务) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], data shaping service for OLE DB
- data shaping service for OLE DB [ADO]
ms.assetid: 523009ce-e01b-4e2d-a7df-816d7688aff0
author: rothja
ms.author: jroth
ms.openlocfilehash: a83d37c301d34c273514e4d6eb0551bc86429bd4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029371"
---
# <a name="microsoft-data-shaping-service-for-ole-db-overview"></a>适用于 OLE DB 的 Microsoft 数据定形服务概述
> [!IMPORTANT]
>  此功能将在 Windows 的将来版本中删除。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 相反，应用程序应使用 XML。

 用于 OLE DB 服务提供商的 Microsoft 数据定形服务支持从数据提供程序构造分层 (形状) [Recordset](../../reference/ado-api/recordset-object-ado.md) 对象。

## <a name="provider-keyword"></a>Provider 关键字
 若要为 OLE DB 调用数据定形服务，请在连接字符串中指定以下关键字和值。

```vb
"Provider=MSDataShape"
```

## <a name="dynamic-properties"></a>动态属性
 调用此服务提供程序时，会将以下动态属性添加到[连接](../../reference/ado-api/connection-object-ado.md)对象的[properties](../../reference/ado-api/properties-collection-ado.md)集合。

|动态属性名称|说明|
|---------------------------|-----------------|
|**唯一改变名称**|指示是否允许为其重设其 **名称** 属性的重复值的 **记录集** 对象。 如果此动态属性为 **True** ，并且使用与现有 **记录集** 相同的用户指定的重设名称来创建新的 **记录集**，则会修改新的 **记录集** 对象的重设名称，使其唯一。 如果此属性为 **False** ，并且使用与现有 **记录集** 相同的用户指定的重设名称创建新的 **记录集**，则这两个 **记录集** 对象将具有相同的改变名称。 因此，只要两个记录集都存在，就不能改变 **记录集** 。<br /><br /> 此属性的默认值为 **False**。|
|**数据提供程序**|指示提供程序的名称，该提供程序将提供要整形的行。 如果提供程序将不用于提供行，则此值可以为 "无"。|

 还可以通过在连接字符串中将其名称指定为关键字，来设置可写的动态属性。 例如，在 Microsoft Visual Basic 中，通过指定以下内容将 **数据访问接口** 动态属性设置为 "MSDASQL"：

```vb
Dim cn as New ADODB.Connection
cn.Open "Provider=MSDataShape;Data Provider=MSDASQL"
```

 还可以通过将动态属性的名称指定为 [Properties](../../reference/ado-api/properties-collection-ado.md) 属性的索引来设置或检索该属性。 例如，下面的代码示例获取并输出 **数据提供程序** 动态属性的当前值，并在 cn 中设置一个新值。DataProvider 已设置为 "MSDataShape" (直接或间接地通过连接字符串) 并且尚未打开该连接：

```vb
Debug.Print cn.Properties("Data Provider")
cn.Properties("Data Provider") = "MSDASQL"
```

> [!NOTE]
>  只能对未打开的 **连接** 对象设置动态属性 **数据提供程序**。 打开连接后， **数据访问接口** 属性将变为只读。

 有关数据定形的详细信息，请参阅 [数据定形](../data/data-shaping-overview.md)。

## <a name="see-also"></a>另请参阅
 [附录 A：提供程序](./appendix-a-providers.md)