---
description: RDS 对象
title: RDS 对象 |Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
helpviewer_keywords:
- objects [ADO], RDS
- RDS objects [ADO]
ms.assetid: f2ac8b3b-f968-41c4-a504-7aee3538b7c7
author: rothja
ms.author: jroth
ms.openlocfilehash: 1f53cd2ef72b6ff540a8663f6400c8dcdf2fb822
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052978"
---
# <a name="rds-objects"></a>RDS 对象
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
|对象|说明|  
|-|-|  
|[DataControl (RDS) ](./datacontrol-object-rds.md)|将数据查询 **记录集** 绑定到一个或多个控件 (例如，文本框、网格控件或组合框) ，用于在网页上显示 **记录集** 数据。<br /><br /> **DataControl** 对象可安全地用于脚本编写。|  
|[DataFactory (RDSServer) ](./datafactory-object-rdsserver.md)|实现为客户端应用程序提供对指定数据源的读/写数据访问的方法。<br /><br /> **DataFactory** 对象对于脚本编写是不安全的。|  
|[空间 (RDS) ](./dataspace-object-rds.md)|创建位于中间层上的自定义业务对象的客户端代理。<br /><br /> 对于脚本编写，" **空间** " 对象是安全的。|  
|[IRDSService 接口 (RDS)](./irdsservice-interface-rds.md)|公开 [InvokeService (RDS) ](./invokeservice-rds.md) 方法，该方法用于在对象的更强版本上返回指向所请求接口的指针。|  
  
## <a name="see-also"></a>另请参阅  
 [RDS API 参考](./rds-api-reference.md)