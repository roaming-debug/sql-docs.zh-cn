---
description: Handler 属性 (RDS)
title: 处理程序属性 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Handler property [ADO]
ms.assetid: fdc34362-6d47-4727-b171-8d033159408e
author: rothja
ms.author: jroth
ms.openlocfilehash: 4829eadfc91be74ae3f487d9f315162788b7ca0e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053118"
---
# <a name="handler-property-rds"></a>Handler 属性 (RDS)
指示 (处理程序) 的服务器端自定义项的名称，该程序扩展了 [DataFactory](./datafactory-object-rdsserver.md)的功能，以及 *处理程序* 使用的任何参数。  
  
 **适用于：** [DATACONTROL 对象 (RDS)](./datacontrol-object-rds.md)  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataControl.Handler = String  
```  
  
#### <a name="parameters"></a>参数  
 *DataControl*  
 表示 RDS 的对象变量 [。DataControl](./datacontrol-object-rds.md) 对象。  
  
 *字符串*  
 一个 **字符串** 值，其中包含处理程序的名称和任何参数（以逗号分隔） (例如， `"handlerName,parm1,parm2,...,parm` *N* `"`) 。  
  
## <a name="remarks"></a>备注  
 此属性支持 [自定义](../../guide/remote-data-service/datafactory-customization.md)功能，该功能需要将 [CursorLocation](../ado-api/cursorlocation-property-ado.md) 属性设置为 **adUseClient**。  
  
 处理程序的名称及其参数（如果有）用逗号分隔 ( "，" ) 。 如果分号 ( ";"，将导致不可预知的行为) 出现在 *字符串* 中的任何位置。 您可以编写自己的处理程序，前提是它支持 **IDataFactoryHandler** 接口。  
  
 默认处理程序的名称为 **MSDFMAP。处理程序**，其默认参数是名为 **MSDFMAP.INI** 的自定义文件。 使用此属性可调用由您的服务器管理员创建的备用自定义文件。  
  
 设置 **处理程序** 属性的替代方法是在 [ConnectionString](../ado-api/connectionstring-property-ado.md) 属性中指定处理程序和参数;也就是说，"**Handler =**_handlerName，parameter1，parameter2,...;_"。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [处理程序属性示例 (VB) ](./handler-property-example-vb.md)   
 [自定义 DataFactory](../../guide/remote-data-service/datafactory-customization.md)   
 [DataFactory 对象 (RDSServer)](./datafactory-object-rdsserver.md)