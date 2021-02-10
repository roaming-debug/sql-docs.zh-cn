---
description: ParameterDirectionEnum
title: ParameterDirectionEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ParameterDirectionEnum
helpviewer_keywords:
- ParameterDirectionEnum enumeration [ADO]
ms.assetid: c66aa6e6-d4f0-4f0f-9640-e08ae6cfdef3
author: rothja
ms.author: jroth
ms.openlocfilehash: 7b352c5504571d50e8840e0d05e0d906a07d4b36
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041117"
---
# <a name="parameterdirectionenum"></a>ParameterDirectionEnum
指定 [参数](./parameter-object.md) 是否表示输入参数、输出参数、输入参数和输出参数，或存储过程的返回值。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adParamInput**|1|默认。 指示参数表示输入参数。|  
|**adParamInputOutput**|3|指示参数表示输入参数和输出参数。|  
|**adParamOutput**|2|指示参数表示输出参数。|  
|**adParamReturnValue**|4|指示参数表示返回值。|  
|**adParamUnknown**|0|指示参数方向未知。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums.ParameterDirection.INPUT|  
|AdoEnums.ParameterDirection.INPUTOUTPUT|  
|AdoEnums.ParameterDirection.OUTPUT|  
|AdoEnums.ParameterDirection.RETURNVALUE|  
|AdoEnums.ParameterDirection.UNKNOWN|  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [CreateParameter 方法 (ADO)](./createparameter-method-ado.md)  
    :::column-end:::
    :::column:::
        [Direction 属性](./direction-property.md)  
    :::column-end:::
:::row-end:::