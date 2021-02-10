---
description: SaveOptionsEnum
title: SaveOptionsEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- SaveOptionsEnum
helpviewer_keywords:
- SaveOptionsEnum enumeration [ADO]
ms.assetid: 59339100-6e29-48d1-aea3-6873796d186b
author: rothja
ms.author: jroth
ms.openlocfilehash: db94b409a799a24d0c74dfec5a3d388c55df20e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040667"
---
# <a name="saveoptionsenum"></a>SaveOptionsEnum
指定在从 [流](./stream-object-ado.md) 对象保存时是否应创建或覆盖文件。 这些值可以是 **adSaveCreateNotExist** 或 **adSaveCreateOverWrite**。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adSaveCreateNotExist**|1|默认。 如果 *FileName* 参数指定的文件尚不存在，则创建一个新文件。|  
|**adSaveCreateOverWrite**|2|如果 *Filename* 参数指定的文件已存在，则使用当前打开的 **流** 对象中的数据覆盖该文件。 如果 *Filename* 参数指定的文件不存在，则创建新的文件。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 这些常量没有 ADO/WFC 等效项。  
  
## <a name="applies-to"></a>应用于  
 [SaveToFile 方法](./savetofile-method.md)