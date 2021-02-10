---
description: onError 事件 (RDS)
title: onError 事件 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- onError event [ADO]
ms.assetid: b01cbc62-fbd7-4068-b16c-8b0f80a05887
author: rothja
ms.author: jroth
ms.openlocfilehash: 6912fe32c7d070b0f58c9c42fdd2217f91a72b39
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053058"
---
# <a name="onerror-event-rds"></a>onError 事件 (RDS)
只要在操作过程中发生错误，就会调用 **onError** 事件。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
onError SCode, Description, Source, CancelDisplay  
```  
  
#### <a name="parameters"></a>参数  
 *SCode*  
 一个整数，指示错误的状态代码。  
  
 *说明*  
 指示错误说明的 **字符串** 。  
  
 *Source*  
 一个 **字符串** ，指示导致错误的查询或命令。  
  
 *CancelDisplay*  
 一个 **布尔** 值，如果设置为 **True**，则将阻止错误显示在对话框中。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VC + + 的 ADO 事件模型示例) ](../ado-api/ado-events-model-example-vc.md)   
 [ADO 事件处理程序摘要](../../guide/data/ado-event-handler-summary.md)