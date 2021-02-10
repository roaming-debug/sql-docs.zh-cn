---
description: PersistFormatEnum
title: PersistFormatEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- PersistFormatEnum
helpviewer_keywords:
- PersistFormatEnum enumeration [ADO]
ms.assetid: ebe1a2ab-e9f1-43a2-8f94-b190c9613d70
author: rothja
ms.author: jroth
ms.openlocfilehash: 0d5968d9193f8e083982703664abf29ade2c864c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041017"
---
# <a name="persistformatenum"></a>PersistFormatEnum
指定保存 [记录集](./recordset-object-ado.md)的格式。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adPersistADTG**|0|指示 Microsoft Advanced Data TableGram (ADTG) 格式。|  
|**adPersistADO**|1|指示将使用 ADO 自己的可扩展标记语言 (XML) 格式。 此值与 adPersistXML 相同，包含它是为了向后兼容。|  
|**adPersistXML**|1|指示可扩展标记语言 (XML) 格式。|  
|**adPersistProviderSpecific**|2|指示提供程序将使用其自己的格式保存 **记录集** 。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums.PersistFormat.ADTG|  
|AdoEnums.PersistFormat.XML|  
  
## <a name="applies-to"></a>应用于  
 [Save 方法](./save-method.md)