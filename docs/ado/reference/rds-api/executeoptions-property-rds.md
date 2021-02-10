---
description: ExecuteOptions 属性 (RDS)
title: ExecuteOptions 属性 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- ExecuteOptions property [ADO], VBScript example
ms.assetid: 62a4fd88-afc3-4f1f-b978-40710a30c4e9
author: rothja
ms.author: jroth
ms.openlocfilehash: a7ec4676ae96b1caf5e7561b90d5930a3b07d378
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053168"
---
# <a name="executeoptions-property-rds"></a>ExecuteOptions 属性 (RDS)
指示是否启用异步执行。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回以下值之一。  
  
|返回的常量|说明|  
|--------------|-----------------|  
|**adcExecSync**|同步执行 [记录集](../ado-api/recordset-object-ado.md) 的下一次刷新。|  
|**adcExecAsync**|默认。 异步执行 **记录集** 的下一次刷新。|  
  
> [!NOTE]
>  使用这些常量的每个可执行文件都必须为其提供声明。 你可以从文件 Adcvbs 中剪切并粘贴所需的常量声明，该文件位于 RDS 库的默认安装文件夹中。  
  
## <a name="remarks"></a>备注  
 如果将 **ExecuteOptions** 设置为 **adcExecAsync**，则这会异步执行对 RDS 的下一个 **刷新** 调用 [。DataControl](./datacontrol-object-rds.md) 对象的 **记录集**。  
  
 如果尝试调用 [Reset](./reset-method-rds.md)、 [Refresh](./refresh-method-rds.md)、 [SubmitChanges](./submitchanges-method-rds.md)、 [CancelUpdate](../ado-api/cancelupdate-method-ado.md)或 [Recordset](./recordset-sourcerecordset-properties-rds.md) ，而另一个可能更改 RDS 的异步操作 [。DataControl](./datacontrol-object-rds.md) 对象的 **记录集** 正在执行，发生错误。  
  
 如果在异步操作过程中发生错误，则将使用 **RDS。DataControl** 对象的 [ReadyState](./readystate-property-rds.md)值从 **adcReadyStateLoaded** 更改为 **adcReadyStateComplete**， **Recordset** 属性值不会更改 *。*  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [ExecuteOptions 和 FetchOptions 属性示例 (VBScript) ](./executeoptions-and-fetchoptions-properties-example-vbscript.md)   
 [Cancel 方法 (RDS)](./cancel-method-rds.md)