---
description: ConnectOptionEnum
title: ConnectOptionEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ConnectOptionEnum
helpviewer_keywords:
- ConnectOptionEnum enumeration [ADO]
ms.assetid: bff07eeb-dee3-4e4e-9b2d-d56061ea744d
author: rothja
ms.author: jroth
ms.openlocfilehash: 257dd82d5740c89fd8ca9f8f8d1969d27b9301ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171431"
---
# <a name="connectoptionenum"></a>ConnectOptionEnum
指定在 (同步建立连接后，是否应返回[连接](./connection-object-ado.md)对象的[Open](./open-method-ado-connection.md)方法) 或 (异步) 之前返回。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adAsyncConnect**|16|以异步方式打开连接。 可以使用 [ConnectComplete](./connectcomplete-and-disconnect-events-ado.md) 事件来确定连接何时可用。|  
|**adConnectUnspecified**|-1|默认。 同步打开连接。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums.ConnectOption.ASYNCCONNECT|  
|AdoEnums.ConnectOption.CONNECTUNSPECIFIED|  
  
## <a name="applies-to"></a>应用于  
 [Open 方法（ADO 连接）](./open-method-ado-connection.md)