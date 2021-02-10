---
description: Open 方法 (ADO MD)
title: " (ADO MD) 打开方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Open
- Cellset::Open
helpviewer_keywords:
- Open method [ADO MD]
ms.assetid: a87d8080-a238-45e5-bc80-9a8625b3810f
author: rothja
ms.author: jroth
ms.openlocfilehash: 119a2394fba0b7558ee5a2975e07fccd76bd8ee9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051048"
---
# <a name="open-method-ado-md"></a>Open 方法 (ADO MD)
检索多维查询的结果，并将结果返回到 [单元集](./cellset-object-ado-md.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
Cellset.Open Source, ActiveConnection  
```  
  
#### <a name="parameters"></a>参数  
 *Source*  
 可选。 计算结果为有效多维查询（如 (MDX) 查询的多维表达式）的 **变量** 。 *Source* 参数对应于 [source](./source-property-ado-md.md)属性。 有关 MDX 的详细信息，请参阅 Microsoft 数据访问组件 SDK 中的 [联机分析处理 (OLAP) 文档 OLE DB ](/previous-versions/windows/desktop/ms717005(v=vs.85)) 。  
  
 *ActiveConnection*  
 可选。 一个 **变量** ，其计算结果为字符串，该字符串指定有效的 ADO [连接](../ado-api/connection-object-ado.md) 对象变量名称或连接定义。 *ActiveConnection* 参数指定要在其中打开 [单元集](./cellset-object-ado-md.md)对象的连接。 如果传递此参数的连接定义，ADO 将使用指定的参数打开新连接。 *ActiveConnection* 参数对应于 [ActiveConnection](./activeconnection-property-ado-md.md)属性。  
  
## <a name="remarks"></a>备注  
 如果在尝试打开 **单元集** 之前省略了其参数的任何一个参数并且尚未设置其相应的属性值，则 **Open** 方法会生成错误。  
  
## <a name="applies-to"></a>应用于  
 [单元集对象 (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB) 单元集示例 ](./cellset-example-vb.md)   
 [ActiveConnection 属性 (ADO MD) ](./activeconnection-property-ado-md.md)   
 [Close 方法 (ADO MD) ](./close-method-ado-md.md)   
 [ADO) 的连接对象 (](../ado-api/connection-object-ado.md)   
 [源属性 (ADO MD) ](./source-property-ado-md.md)   
 [State 属性 (ADO MD)](./state-property-ado-md.md)