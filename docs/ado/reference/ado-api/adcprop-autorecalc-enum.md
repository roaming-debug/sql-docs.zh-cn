---
description: ADCPROP_AUTORECALC_ENUM
title: ADCPROP_AUTORECALC_ENUM |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADCPROP_AUTORECALC_ENUM
helpviewer_keywords:
- ADCPROP_AUTORECALC_ENUM [ADO]
ms.assetid: ded4f087-87b9-4efa-8026-bde53d3e9e8a
author: rothja
ms.author: jroth
ms.openlocfilehash: 9c29d26af25a09c659fe141584b2e0200a674142
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035847"
---
# <a name="adcprop_autorecalc_enum"></a>ADCPROP_AUTORECALC_ENUM
指定 [MSDataShape](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) 提供程序何时重新计算分层记录集中的聚合和计算列。  
  
 这些常量仅用于 **MSDataShape** 提供程序和 **记录集** "**自动** 重新计算" 动态属性，该属性在 [ADO 动态属性索引](./ado-dynamic-property-index.md) 中进行了引用，并在 [Microsoft Cursor service for OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) 或 [microsoft 数据定形服务（用于 OLE DB](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) 文档）中进行了介绍。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adRecalcAlways**|1|默认。 每当 **MSDataShape** 提供程序确定计算列所依赖的值已更改时重新计算。|  
|**adRecalcUpFront**|0|仅当最初生成分层 **记录集** 时才进行计算。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 这些常量没有 ADO/WFC 等效项。