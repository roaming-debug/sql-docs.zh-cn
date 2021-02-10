---
description: FetchOptions 属性 (RDS)
title: FetchOptions 属性 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FetchOptions property [ADO]
ms.assetid: 7b2e254a-9354-4541-bc98-bb185276388f
author: rothja
ms.author: jroth
ms.openlocfilehash: 2299e0b918206a1eadfa3d3352a01ec0d7320152
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049407"
---
# <a name="fetchoptions-property-rds"></a>FetchOptions 属性 (RDS)
指示异步提取的类型。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="setting-and-return-values"></a>设置和返回值  
 设置或返回以下值之一。  
  
|返回的常量|说明|  
|--------------|-----------------|  
|**adcFetchUpFront**|在将控制权返回到应用程序之前，将提取 [记录集](../ado-api/recordset-object-ado.md) 的所有记录。 在允许应用程序执行任何操作之前，将提取完整的 **记录集** 。|  
|**adcFetchBackground**|提取第一批记录后，控件即可返回到应用程序。 对于尝试访问第一批中未提取的记录的记录 **集** ，后面的记录将被延迟，直到实际提取所查找的记录为止，此时控件返回到应用程序。|  
|**adcFetchAsync**|默认。 当在后台提取记录时，控件立即返回到应用程序。 如果应用程序尝试读取尚未提取的记录，则将读取与所查找记录最接近的记录，并且控制将立即返回，这表示已到达 **记录集** 的当前端。 例如，对 [MoveLast](./movefirst-movelast-movenext-and-moveprevious-methods-rds.md) 的调用会将当前记录位置移动到实际提取的最后一条记录，即使更多记录将继续填充 **记录集**。|  
  
> [!NOTE]
>  每个使用这些常量的客户端可执行文件都必须为其提供声明。 你可以从文件 Adcvbs 中剪切并粘贴所需的常量声明，该文件位于 RDS 库的默认安装文件夹中。  
  
## <a name="remarks"></a>备注  
 在 Web 应用程序中，你通常会希望使用 **adcFetchAsync** (默认值) ，因为它提供更好的性能。 在已编译的客户端应用程序中，通常需要使用 **adcFetchBackground**。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [ExecuteOptions 和 FetchOptions 属性示例 (VBScript) ](./executeoptions-and-fetchoptions-properties-example-vbscript.md)   
 [Cancel 方法 (RDS)](./cancel-method-rds.md)