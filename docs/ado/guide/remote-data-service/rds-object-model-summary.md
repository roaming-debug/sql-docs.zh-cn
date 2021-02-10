---
description: RDS 对象模型摘要
title: RDS 对象模型摘要 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS objects [ADO], object model summary
- RDS object model [ADO]
ms.assetid: 909f9af7-31db-4eec-ad52-650ce74dac2f
author: rothja
ms.author: jroth
ms.openlocfilehash: 67f6cdaeea36e5ed13e28a7674a0bb4b3d6ef3f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036457"
---
# <a name="rds-object-model-summary"></a>RDS 对象模型摘要
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
|对象|说明|  
|------------|-----------------|  
|[RDS.空间](../../reference/rds-api/dataspace-object-rds.md)|此对象包含用于获取服务器代理的方法。 代理可能是默认的或自定义的服务器程序 (业务对象) 。 服务器程序可以在 Internet、intranet、局域网或本地动态链接库上进行调用。<br /><br /> 对于脚本编写，" **空间** " 对象是安全的。|  
|[RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md)|此对象表示默认服务器程序。 它执行默认的 RDS 数据检索和更新行为。<br /><br /> **DataFactory** 对象对于脚本编写是不安全的。|  
|[RDS.DataControl](../../reference/rds-api/datacontrol-object-rds.md)|此对象可自动调用 **RDS。** RDSServer 和 **DataFactory** 对象。<br /><br /> 使用此对象可调用默认的 RDS 数据检索或更新行为。<br /><br /> 此对象还为视觉对象提供访问返回的 **Recordset** 对象的方法。<br /><br /> **DataControl** 对象可安全地用于脚本编写。|  
  
## <a name="see-also"></a>另请参阅  
 [RDS 基础知识](./rds-fundamentals.md)   
 [RDS 方案](./rds-scenario.md)   
 [RDS 教程](./rds-tutorial.md)   
 [RDS 使用情况和安全性](./rds-usage-and-security.md)