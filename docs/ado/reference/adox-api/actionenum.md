---
description: ActionEnum
title: ActionEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ActionEnum
helpviewer_keywords:
- ActionEnum enumeration [ADOX]
ms.assetid: f948febd-c885-4621-823b-421e116fec4e
author: rothja
ms.author: jroth
ms.openlocfilehash: 35f4ddd60d4056cebbf0383aa0afb240c3ecb722
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169665"
---
# <a name="actionenum"></a>ActionEnum
指定在调用 [SetPermissions](./setpermissions-method-adox.md) 时要执行的操作的类型。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adAccessDeny**|3|该组或用户将被拒绝指定的权限。|  
|**adAccessGrant**|1|该组或用户将至少具有所需的权限。|  
|**adAccessRevoke**|4|组或用户的所有显式访问权限将被吊销。|  
|**adAccessSet**|2|该组或用户将拥有完全请求的权限。|