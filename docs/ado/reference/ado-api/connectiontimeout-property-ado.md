---
description: ConnectionTimeout 属性 (ADO)
title: " (ADO) 的 ConnectionTimeout 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::ConnectionTimeout
helpviewer_keywords:
- ConnectionTimeout property [ADO]
ms.assetid: 8904a403-1383-4b4b-b53d-5c01d6f5deac
author: rothja
ms.author: jroth
ms.openlocfilehash: 266d57defde6abaae34e558496092ec2df7caebb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164591"
---
# <a name="connectiontimeout-property-ado"></a>ConnectionTimeout 属性 (ADO)
指示在终止尝试并生成错误之前建立连接时等待的时间。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **长整型** 值，该值指示等待连接打开的时间（以秒为单位）。 默认为 15.  
  
## <a name="remarks"></a>备注  
 使用 [连接](./connection-object-ado.md)对象的 **ConnectionTimeout** 属性。如果从网络流量或繁重服务器使用延迟，则需要放弃连接尝试。 如果在打开连接之前， **ConnectionTimeout** 属性设置的时间已过，则会发生错误并且 ADO 会取消尝试。 如果将属性设置为零，则 ADO 将无限期地等待连接打开。 请确保编写代码的提供程序支持 **ConnectionTimeout** 功能。  
  
 当连接关闭时， **ConnectionTimeout** 属性是可读/写的，并且在打开时为只读。  
  
## <a name="applies-to"></a>应用于  
 [连接对象 (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ConnectionString、ConnectionTimeout 和 State 属性示例 (VB) ](./connectionstring-connectiontimeout-and-state-properties-example-vb.md)   
 [ConnectionString、ConnectionTimeout 和 State 属性示例 (VC + +) ](./connectionstring-connectiontimeout-and-state-properties-example-vc.md)   
 [CommandTimeout 属性 (ADO)](./commandtimeout-property-ado.md)