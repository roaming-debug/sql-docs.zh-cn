---
description: Cancel 方法 (RDS)
title: " (RDS) 的 Cancel 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Cancel method [RDS]
ms.assetid: 560b5b3d-fba9-4275-8920-9c3e186134f7
author: rothja
ms.author: jroth
ms.openlocfilehash: e87e8d173e1ab9aa8978ed9ab98c6a2f37a93e79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169072"
---
# <a name="cancel-method-rds"></a>Cancel 方法 (RDS)
取消执行挂起的异步方法调用。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
RDS.DataControl.Cancel  
```  
  
## <a name="remarks"></a>备注  
 调用 **Cancel** 时， [ReadyState](./readystate-property-rds.md) 会自动设置为 **adcReadyStateLoaded**，并且 [记录集](../ado-api/recordset-object-ado.md) 将为空。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [取消方法示例 (VBScript) ](./cancel-method-example-vbscript.md)   
 [ADO (取消方法) ](../ado-api/cancel-method-ado.md)   
 [CancelBatch 方法 (ADO) ](../ado-api/cancelbatch-method-ado.md)   
 [CancelUpdate 方法 (ADO) ](../ado-api/cancelupdate-method-ado.md)   
 [CancelUpdate 方法 (RDS) ](./cancelupdate-method-rds.md)   
 [ExecuteOptions 属性 (RDS)](./executeoptions-property-rds.md)