---
description: ReadyState 属性 (RDS)
title: ReadyState 属性 (RDS) |Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- ReadyState property [ADO]
ms.assetid: 5be75bc7-1171-4440-a37e-c8cc6b5cd865
author: rothja
ms.author: jroth
ms.openlocfilehash: a70a2f7fc26e83b21cdb85edb3ddf4a664785d30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049207"
---
# <a name="readystate-property-rds"></a>ReadyState 属性 (RDS)
指示 [DataControl](./datacontrol-object-rds.md) 对象在其 [Recordset](../ado-api/recordset-object-ado.md) 对象中检索数据时的进度。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**adcReadyStateLoaded**|当前查询仍在执行，未提取行。 **DataControl** 对象的 **记录集** 不可用。|  
|**adcReadyStateInteractive**|当前查询检索到的初始行集已存储在 **DataControl** 对象的 **记录集中** ，可供使用。 仍在提取剩余行。|  
|**adcReadyStateComplete**|当前查询检索到的所有行都存储在 **DataControl** 对象的 **记录集中** ，可供使用。<br /><br /> 如果操作由于错误而中止，或者 **记录集** 对象未初始化，则也会存在此状态。|  
  
> [!NOTE]
>  每个使用这些常量的客户端可执行文件都必须为其提供声明。 你可以从文件 Adcvbs 中剪切并粘贴所需的常量声明，该文件位于 RDS 库的默认安装文件夹中。  
  
## <a name="remarks"></a>备注  
 在异步查询操作期间，使用 [onReadyStateChange](./onreadystatechange-event-rds.md) 事件监视 **ReadyState** 属性中的更改。 这比定期检查属性值更有效。  
  
 如果在异步操作过程中发生错误，则 **ReadyState** 属性更改为 **adcReadyStateComplete**，[状态](../ado-api/state-property-ado.md)属性从 **adStateExecuting** 更改为 **adStateClosed**，而 **Recordset** 对象的 [值](../ado-api/value-property-ado.md)属性则不 *会。*  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [ReadyState 属性示例 (VBScript) ](./readystate-property-example-vbscript.md)   
 [ (RDS) 的 Cancel 方法 ](./cancel-method-rds.md)   
 [ExecuteOptions 属性 (RDS)](./executeoptions-property-rds.md)