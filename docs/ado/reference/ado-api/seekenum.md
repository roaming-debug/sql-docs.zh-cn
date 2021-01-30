---
description: SeekEnum
title: SeekEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SeekEnum
helpviewer_keywords:
- SeekEnum enumeration [ADO]
ms.assetid: f0ec0c92-8253-47c6-9a14-e5dbccbad219
author: rothja
ms.author: jroth
ms.openlocfilehash: f61e05965b1750c4f8e6b48f092eeb059d1e19d3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166556"
---
# <a name="seekenum"></a>SeekEnum
指定要执行的 [搜索](./seek-method.md) 的类型。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adSeekFirstEQ**|1|查找等于 *架构* 的第一个键。|  
|**adSeekLastEQ**|2|查找等于 *架构* 的最后一个键。|  
|**adSeekAfterEQ**|4|查找等效于 *架构* 的键，或只查找在匹配项发生的位置。|  
|**adSeekAfter**|8|在出现与 *架构* 的匹配的位置之前查找密钥。|  
|**adSeekBeforeEQ**|16|查找等于 *架构* 的键，或刚好早于匹配的位置。|  
|**adSeekBefore**|32|在出现与 *架构* 匹配的位置之前查找密钥。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums.Seek.FIRSTEQ|  
|AdoEnums.Seek.LASTEQ|  
|AdoEnums.Seek.AFTEREQ|  
|AdoEnums.Seek.AFTER|  
|AdoEnums.Seek.BEFOREEQ|  
|AdoEnums.Seek.BEFORE|  
  
## <a name="applies-to"></a>应用于  
 [Seek 方法](./seek-method.md)