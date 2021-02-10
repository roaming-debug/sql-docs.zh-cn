---
description: DataSource 属性 (ADO)
title: 数据源属性 (ADO) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::DataSource
helpviewer_keywords:
- DataSource property [ADO]
ms.assetid: 300a702a-3544-48c5-b759-83b511fe97e0
author: rothja
ms.author: jroth
ms.openlocfilehash: 73058aa92e594528ef214a9ffa93fa0f58d29b76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025664"
---
# <a name="datasource-property-ado"></a>DataSource 属性 (ADO)
指示包含要表示为 [记录集](../../../ado/reference/ado-api/recordset-object-ado.md) 对象的数据的对象。  
  
## <a name="remarks"></a>备注  
 此属性用于通过数据环境创建数据绑定控件。 数据环境维护数据源 (数据源的集合) 包含 (数据成员) 的命名对象，这些成员将表示为 **记录集** 对象。  
  
 [DataMember](../../../ado/reference/ado-api/datamember-property.md)和 **DataSource** 属性必须结合使用。  
  
 所引用的对象必须实现 **IDataSource** 接口，并且必须包含 **IRowset** 接口。  
  
## <a name="usage"></a>使用情况  
  
```  
Dim rs as New ADODB.Recordset  
rs.DataMember = "Command"     'Name of the rowset to bind to.  
Set rs.DataSource = myDE      'Name of the object containing an IRowset.  
```  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [DataMember 属性](../../../ado/reference/ado-api/datamember-property.md)
