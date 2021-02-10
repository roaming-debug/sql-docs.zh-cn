---
description: DataMember 属性
title: DataMember 属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::DataMember
helpviewer_keywords:
- DataMember property
ms.assetid: 2c8fb09e-10ad-49b5-ab41-2603771780d9
author: rothja
ms.author: jroth
ms.openlocfilehash: 1026951122336e67760f74077044ff5fd9b9fd76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034427"
---
# <a name="datamember-property"></a>DataMember 属性
指示将从[DataSource](../../../ado/reference/ado-api/datasource-property-ado.md)属性所引用的[记录集中](../../../ado/reference/ado-api/recordset-object-ado.md)检索的数据成员的名称。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **字符串** 值。 该名称不区分大小写。  
  
## <a name="remarks"></a>备注  
 此属性用于通过数据环境创建数据绑定控件。 数据环境维护数据源 (数据源的集合) 包含 (数据成员) 的命名对象，这些成员将表示为 [记录集](../../../ado/reference/ado-api/recordset-object-ado.md) 对象。  
  
 **DataMember** 和 **DataSource** 属性必须一起使用。  
  
 **DataMember** 属性确定由 **DataSource** 属性指定的哪个对象将表示为 **记录集** 对象。 必须先关闭 **Recordset** 对象，然后才能设置此属性。 如果在 **datasource** 属性之前未设置 **datamember** 属性，或者 **数据源** 属性中指定的对象无法识别 **datamember** 名称，则会生成错误。  
  
## <a name="usage"></a>使用情况  
  
```  
Dim rs as New ADODB.Recordset  
rs.DataMember = "Command"     'Name of the rowset to bind to  
Set rs.DataSource = myDE      'Name of the object containing an IRowset  
```  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [DataSource 属性 (ADO)](../../../ado/reference/ado-api/datasource-property-ado.md)
