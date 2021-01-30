---
description: ConnectPromptEnum
title: ConnectPromptEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ConnectPromptEnum
helpviewer_keywords:
- ConnectPromptEnum enumeration [ADO]
ms.assetid: 21026e24-62b7-4cc9-8aef-62c1fc6cba75
author: rothja
ms.author: jroth
ms.openlocfilehash: 7563179d70968cc98fbbc1055ec32c2c3a1948f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167693"
---
# <a name="connectpromptenum"></a>ConnectPromptEnum
指定在打开与数据源的连接时是否应显示一个对话框，以提示输入缺少的参数。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adPromptAlways**|1|始终提示。|  
|**adPromptComplete**|2|如果需要详细信息，请提示。|  
|**adPromptCompleteRequired**|3|如果需要详细信息但不允许使用可选参数，则会提示。|  
|**adPromptNever**|4|从不提示。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums. ConnectPrompt. ALWAYS|  
|AdoEnums. ConnectPrompt. 完成|  
|AdoEnums.ConnectPrompt.COMPLETEREQUIRED|  
|AdoEnums. ConnectPrompt. 从不|  
  
## <a name="applies-to"></a>应用于  
 [Prompt 属性 - 动态 (ADO)](./prompt-property-dynamic-ado.md)