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
ms.openlocfilehash: db02c56f55b577c9f74a42f43abc9a1957311c0d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040747"
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