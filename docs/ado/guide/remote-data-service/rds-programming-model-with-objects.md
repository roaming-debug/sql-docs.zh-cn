---
description: RDS 编程模型和对象
title: 具有对象的 RDS 编程模型 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS programming model [ADO]
- RDS objects [ADO]
ms.assetid: 07ce0ef0-72f1-48f4-823d-1b65d28c0926
author: rothja
ms.author: jroth
ms.openlocfilehash: 282e272d1d830bfb20f407053a04a6d0b598c490
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036357"
---
# <a name="rds-programming-model-with-objects"></a>RDS 编程模型和对象
RDS 的目标是通过诸如 IIS 这样的媒介获取和更新数据源。 编程模型指定完成此目标所需的活动序列。 对象模型指定其方法和属性影响编程模型的对象。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 RDS 提供了执行以下一系列操作的方法：  
  
-   指定要在服务器上调用的程序，并获取 (proxy) 从客户端 (RDS 引用该程序的方法 [。空间](../../reference/rds-api/dataspace-object-rds.md)) 。  
  
-   调用服务器程序。 将参数传递给用于标识数据源的服务器程序，以及要 (proxy 或 RDS 发出的命令 [。DataControl](../../reference/rds-api/datacontrol-object-rds.md)) 。  
  
-   服务器程序从数据源中获取 [记录集](../../reference/ado-api/recordset-object-ado.md) 对象，通常使用 ADO。 （可选）在服务器 ([DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md)) 上处理 **Recordset** 对象。  
  
-   服务器程序将最终的 **记录集** 对象返回给客户端应用程序 (proxy) 。  
  
-   在客户端上， **记录集** 对象将被放入窗体，以便视觉对象和 RDS (视觉对象 **。DataControl**) 。  
  
-   对 **Recordset** 对象所做的更改会发送回服务器，并用于更新数据源 (**RDS。DataControl** 或 **DataFactory**) 。  
  
## <a name="see-also"></a>另请参阅  
 [RDS 对象模型摘要](./rds-object-model-summary.md)   
 [DataControl 对象 (RDS) ](../../reference/rds-api/datacontrol-object-rds.md)   
 [DataFactory 对象 (RDSServer) ](../../reference/rds-api/datafactory-object-rdsserver.md)   
 [ (RDS) 的空间对象 ](../../reference/rds-api/dataspace-object-rds.md)   
 [RDS 方案](./rds-scenario.md)   
 [RDS 教程](./rds-tutorial.md)   
 [ADO)  (Recordset 对象 ](../../reference/ado-api/recordset-object-ado.md)   
 [RDS 使用情况和安全性](./rds-usage-and-security.md)