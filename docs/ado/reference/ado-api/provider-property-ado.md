---
description: Provider 属性 (ADO)
title: ADO)  (提供程序属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::get_Provider
- Connection15::PutProvider
- Connection15::put_Provider
- Connection15::GetProvider
- Connection15::Provider
helpviewer_keywords:
- Provider property [ADO]
ms.assetid: 0ff70e72-0061-4ffc-90fb-e3ea23129bb2
author: rothja
ms.author: jroth
ms.openlocfilehash: 3e022db374dc818529ed241f9d2ed5b525c8f374
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040827"
---
# <a name="provider-property-ado"></a>Provider 属性 (ADO)
指示 [连接](./connection-object-ado.md) 对象的访问接口的名称。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个指示提供程序名称的 **字符串** 值。  
  
## <a name="remarks"></a>备注  
 使用 **provider** 属性可设置或返回连接的访问接口的名称。 此属性还可以通过 [ConnectionString](./connectionstring-property-ado.md)属性的内容或 [Open](./open-method-ado-connection.md)方法的 *connectionstring* 参数设置;但是，在调用 **Open** 方法时在多个位置指定提供程序可能会产生不可预知的结果。 如果未指定提供程序，则该属性将默认为 MSDASQL ([Microsoft OLE DB provider FOR ODBC](../../guide/appendixes/microsoft-ole-db-provider-for-odbc.md)) 。  
  
 当连接关闭并且处于打开状态时， **提供程序** 属性是可读/写的。 只有打开 **连接** 对象或访问 **连接** 对象的 [Properties](./properties-collection-ado.md)集合后，此设置才会生效。 如果设置无效，则会发生错误。  
  
## <a name="applies-to"></a>应用于  
 [连接对象 (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Provider 和 DefaultDatabase Properties (VB) ](./provider-and-defaultdatabase-properties-example-vb.md)   
 [Provider 和 DefaultDatabase Properties (VB) ](./provider-and-defaultdatabase-properties-example-vb.md)   
 [适用于 ODBC 的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-odbc.md)   
 [附录 A：提供程序](../../guide/appendixes/appendix-a-providers.md)