---
description: 在组件服务中运行业务对象
title: 在组件服务中运行业务对象 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- component services in RDS [ADO]
ms.assetid: 3077d0b6-42d6-4f10-8e5d-42e6204f1109
author: rothja
ms.author: jroth
ms.openlocfilehash: 7129254d609e1bd80853ccf5008527bfbf938c30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036377"
---
# <a name="running-business-objects-in-component-services"></a>在组件服务中运行业务对象
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 业务对象可以是可执行文件 ( .exe) 或动态链接库 () 。 用于运行业务对象的配置取决于对象是否为 .dll 或 .exe 文件：  
  
-   可以通过 DCOM 调用创建为 .exe 文件的业务对象。 如果这些业务对象通过 (IIS) Internet Information Services 使用，则它们可能会受到其他数据封送处理，从而降低客户端性能。  
  
-   作为 .dll 文件创建的业务对象可以通过 IIS 来使用，也可以通过 HTTP 使用。 如果使用的是 Windows NT，还可以通过组件服务或通过 Microsoft 事务服务器使用 DCOM。 业务对象 Dll 需要在 IIS 服务器计算机上注册，才能通过 IIS 进行访问。 有关如何将 DLL 配置为在 DCOM 上运行的信息，请参阅 [启用 dll 在 dcom 上运行](./enabling-a-dll-to-run-on-dcom.md)部分。  
  
> [!NOTE]
>  当使用 **GetObjectContext**、 **SetComplete** 和 **SetAbort** 将中间层上的业务对象作为组件服务组件实现时，如果使用的是 Windows NT) 上下文对象，则业务对象可以使用组件服务 (或 MTS 来跨多个客户端调用维护其状态。 这种情况可以与 DCOM 一起使用，DCOM 通常在 intranet 中的受信任的客户端和服务器之间实现。 在这种情况下， [RDS。](../../reference/rds-api/dataspace-object-rds.md) 客户端上的 "空间对象" 和 " [CreateObject](../../reference/rds-api/createobject-method-rds.md) " 方法替换为事务上下文对象和 **CreateInstance** 方法，该方法由 **ITransactionContext** 接口提供并由组件服务实现。  
  
## <a name="see-also"></a>另请参阅  
 [RDS 基础知识](./rds-fundamentals.md)