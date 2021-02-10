---
description: Server 属性 (RDS)
title: 服务器属性 (RDS) |Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
f1_keywords:
- RDS::IBindMgr21::Server
helpviewer_keywords:
- Server property [RDS]
ms.assetid: d2727ce7-da9f-4271-ae3c-9334ef477c14
author: rothja
ms.author: jroth
ms.openlocfilehash: cc6262e9748a4fd94fd7f4e0a75214c24def0acb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052928"
---
# <a name="server-property-rds"></a>Server 属性 (RDS)
指示 IIS) 名称和通信协议 (Internet Information Services。  
  
 您可以在设计时在 RDS 的对象标记中设置 **服务器** 属性 [。DataControl](./datacontrol-object-rds.md) 对象，或在运行时在脚本代码中运行。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
 **HTTP**  
  
 设计时语法  
  
```  
  
<PARAM NAME="Server" VALUE="http:  
//awebsrvr:port  
">  
  
```  
  
 运行时语法  
  
```  
  
DataControl  
.Server="https://  
awebsrvr:port  
"  
  
```  
  
 **HTTPS**  
  
 设计时语法  
  
```  
  
<PARAM NAME="Server" VALUE="https://awebsrvr:port">  
```  
  
 运行时语法  
  
```  
  
DataControl.Server="https://awebsrvr:port"  
```  
  
 **DCOM**  
  
 设计时语法  
  
```  
  
<PARAM NAME="Server" VALUE="  
computername  
">  
  
```  
  
 运行时语法  
  
```  
  
DataControl.Server="computername"  
```  
  
 **进程内**  
  
 设计时语法  
  
```  
  
<PARAM NAME="Server" VALUE="">  
  
```  
  
 运行时语法  
  
```  
  
DataControl.Server=""  
```  
  
## <a name="parameters"></a>参数  
 *awebsrvr* 或 *computername*  
 如果服务器位于远程计算机上，则为包含 Internet 或 intranet 路径或计算机名称的 **字符串** 值;或者，如果服务器在本地计算机上，则为空字符串。  
  
 *port*  
 可选。 用于连接到运行 IIS 的服务器的端口。 端口号是在 Internet Explorer 中设置的 (在 " **视图** " 菜单上，单击 " **选项**"，然后选择 " **连接** " 选项卡) 或 IIS 中。  
  
 *DataControl*  
 表示 RDS 的对象变量 **。DataControl** 对象。  
  
## <a name="remarks"></a>备注  
 服务器是 RDS 的位置 **。DataControl** 请求 (也就是说，处理) 的查询或更新。 默认情况下，所有请求均由 [RDSServer DataFactory](./datafactory-object-rdsserver.md) 对象 [MSDFMAP 处理。处理程序](../../guide/remote-data-service/datafactory-customization.md) 组件，并 [MSDFMAP.INI](../../guide/remote-data-service/understanding-the-customization-file.md) 指定服务器上的文件。 请记住，在将服务器更改为协调新旧 **MSDFMAP.INI** 文件中的设置时。 不兼容性可能会导致一台服务器上的成功请求在另一台服务器上失败。 如果服务器属性设置为空字符串 ""，则将在本地计算机上使用这些对象。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VBScript) 的服务器属性示例 ](./server-property-example-vbscript.md)   
 [ (RDS) 连接属性 ](./connect-property-rds.md)   
 [SQL 属性](./sql-property.md)   
 [SubmitChanges 方法 (RDS)](./submitchanges-method-rds.md)