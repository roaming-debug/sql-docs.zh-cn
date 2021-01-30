---
description: InheritTypeEnum
title: InheritTypeEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- InheritTypeEnum
helpviewer_keywords:
- InheritTypeEnum enumeration [ADOX]
ms.assetid: c2f6ce79-c4b3-4d40-ac95-21025208f991
author: rothja
ms.author: jroth
ms.openlocfilehash: d758c78b8778d5a00c0105c8c9dcebfe40da22e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164184"
---
# <a name="inherittypeenum"></a>InheritTypeEnum
指定对象如何继承具有 [SetPermissions](./setpermissions-method-adox.md)的权限集。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adInheritBoth**|3|对象和主对象所包含的其他容器都继承该条目。|  
|**adInheritContainers**|2|主对象包含的其他容器继承该条目。|  
|**adInheritNone**|0|默认。 不发生继承。|  
|**adInheritNoPropagate**|4|**AdInheritObjects** 和 **adInheritContainers** 标志不会传播到继承的项。|  
|**adInheritObjects**|1|容器中的非容器对象将继承权限。|  
  
## <a name="applies-to"></a>应用于  
 [SetPermissions 方法 (ADOX)](./setpermissions-method-adox.md)