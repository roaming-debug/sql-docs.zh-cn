---
description: EditModeEnum
title: EditModeEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- EditModeEnum
helpviewer_keywords:
- EditModeEnum enumeration [ADO]
ms.assetid: 45d54b6e-db2c-4553-9fd0-528147d6da2f
author: rothja
ms.author: jroth
ms.openlocfilehash: 0c8aafe9069f1ea6718713ddb2239e4e9699ee48
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167505"
---
# <a name="editmodeenum"></a>EditModeEnum
指定记录的编辑状态。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adEditNone**|0|指示没有正在进行的编辑操作。|  
|**adEditInProgress**|1|指示当前记录中的数据已修改但未保存。|  
|**adEditAdd**|2|指示已调用 [AddNew](../../../ado/reference/ado-api/addnew-method-ado.md) 方法，并且复制缓冲区中的当前记录是尚未保存到数据库中的新记录。|  
|**adEditDelete**|4|指示当前记录已被删除。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums. EditMode|  
|AdoEnums. EditMode|  
|AdoEnums. EditMode|  
|AdoEnums. EditMode. DELETE|  
  
## <a name="applies-to"></a>应用于  
 [EditMode 属性](../../../ado/reference/ado-api/editmode-property.md)
