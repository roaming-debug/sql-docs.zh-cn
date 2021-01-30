---
description: ResyncEnum
title: ResyncEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ResyncEnum
helpviewer_keywords:
- ResyncEnum enumeration [ADO]
ms.assetid: d3df2c90-e570-4c40-a79a-25b3448a009c
author: rothja
ms.author: jroth
ms.openlocfilehash: c20965a348227583580bb75a807e792f74a9e967
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166631"
---
# <a name="resyncenum"></a>ResyncEnum
指定是否通过调用 [Resync](./resync-method.md)覆盖基础值。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adResyncAllValues**|2|默认。 覆盖数据，挂起的更新被取消。|  
|**adResyncUnderlyingValues**|1|不会覆盖数据，也不会取消挂起的更新。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums.Resync.ALLVALUES|  
|AdoEnums.Resync.UNDERLYINGVALUES|  
  
## <a name="applies-to"></a>应用于  
 [重新同步方法](./resync-method.md)