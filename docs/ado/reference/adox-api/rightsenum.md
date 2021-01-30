---
description: RightsEnum
title: RightsEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RightsEnum
helpviewer_keywords:
- RightsEnum enumeration [ADOX]
ms.assetid: 55ee67c7-a583-42aa-849a-78264b4cb614
author: rothja
ms.author: jroth
ms.openlocfilehash: d89daeb8d85e9e59e60f6a1aa6b918364ab915fd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169222"
---
# <a name="rightsenum"></a>RightsEnum
指定组或用户对对象的权限。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adRightCreate**|16384 ( # B0 H4000) |用户或组具有创建此类型的新对象的权限。|  
|**adRightDelete**|65536 ( # B0 H10000) |用户或组有权删除对象中的数据。 对于 **表** 等对象，用户有权从记录中删除数据值。|  
|**adRightDrop**|256 ( # B0 H100) |用户或组有权从目录中删除对象。 例如，DROP TABLE SQL 命令可以删除 **表** 。|  
|**adRightExclusive**|512 ( # B0 H200) |用户或组有权仅访问该对象。|  
|**adRightExecute**|536870912 ( # B0 H20000000) |用户或组具有执行对象的权限。|  
|**adRightFull**|268435456 ( # B0 H10000000) |用户或组具有对对象的所有权限。|  
|**adRightInsert**|32768 ( # B0 H8000) |用户或组有权插入对象。 对于 **表** 等对象，用户有权将数据插入表中。|  
|**adRightMaximumAllowed**|33554432 ( # B0 H2000000) |用户或组具有提供程序允许的最大权限数。 特定权限与提供程序相关。|  
|**adRightNone**|0|用户或组对该对象没有任何权限。|  
|**adRightRead**|-2147483648 ( # B0 H80000000) |用户或组有权读取对象。 对于 [表](./table-object-adox.md)等对象，用户有权读取表中的数据。|  
|**adRightReadDesign**|1024 ( # B0 H400) |用户或组有权读取对象的设计。|  
|**adRightReadPermissions**|131072 ( # B0 H20000) |用户或组可以查看但不能更改目录中对象的特定权限。|  
|**adRightReference**|8192 ( # B0 H2000) |用户或组有权引用该对象。|  
|**adRightUpdate**|1073741824 ( # B0 H40000000) |用户或组具有更新该对象的权限。 对于 **表** 等对象，用户有权更新表中的数据。|  
|**adRightWithGrant**|4096 ( # B0 H1000) |用户或组有权授予对对象的权限。|  
|**adRightWriteDesign**|2048 ( # B0 H800) |用户或组有权修改对象的设计。|  
|**adRightWriteOwner**|524288 ( # B0 H80000) |用户或组有权修改对象的所有者。|  
|**adRightWritePermissions**|262144 ( # B0 H40000) |用户或组可以修改目录中对象的特定权限。|  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [GetPermissions 方法 (ADOX)](./getpermissions-method-adox.md)  
    :::column-end:::
    :::column:::
        [SetPermissions 方法 (ADOX)](./setpermissions-method-adox.md)  
    :::column-end:::
:::row-end:::