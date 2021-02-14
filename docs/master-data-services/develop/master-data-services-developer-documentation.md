---
description: Master Data Services 开发人员文档
title: 开发人员文档
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: 067b1f69-84eb-4a13-b220-120cd63704b4
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4062f365552fbbb2076c68f5bf6d3d912b81a490
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350106"
---
# <a name="master-data-services-developer-documentation"></a>Master Data Services 开发人员文档

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  查找有关如何编写用于自定义您和您的用户如何与 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 交互的代码的信息。 了解如何：  
  
-   编写访问 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 服务的程序。 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 服务是一项 Windows Communication Foundation (WCF) 服务，开发人员使用此服务可通过代码控制 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 功能。  
  
-   将 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 功能合并到现有应用程序中。  
  
-   编写用于执行重复性操作或很难或不可能对 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] UI 执行的复杂操作的代码。  
  
-   创建为了响应您指定的业务规则而执行的自定义工作流。 自定义工作流将调用您编写的代码，此代码可以执行处理该工作流所需的任何操作。  
  
## <a name="master-data-manager-web-service"></a>主数据管理器 Web 服务  
 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 服务可让您通过任何能访问 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 网站的计算机以编程方式使用 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 的功能。 在开始编写访问 Web 服务的代码之前，必须先生成代理类，代理类包含在您指定的命名空间中。 本文档使用 <xref:Microsoft.MasterDataServices> 作为代理命名空间。 您用于执行 Web 服务操作的主代理类是 <xref:Microsoft.MasterDataServices.ServiceClient> 类，它可实现 <xref:Microsoft.MasterDataServices.IService> 接口。 通过您的代码，调用 <xref:Microsoft.MasterDataServices.ServiceClient> 类的方法以访问 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 服务。 命名空间中的类的其余部分供 Web 服务操作使用。  
  
### <a name="web-service-content"></a>Web 服务内容  
 [创建主数据管理器 Web 服务代理类](../../master-data-services/develop/create-master-data-manager-web-service-proxy-classes.md)  
 描述如何从[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]网站启用元数据发布，以及如何创建可用于以编程方式访问 Web 服务操作的代理类。  
  
 [分类的 Web 服务操作 &#40;Master Data Services&#41;](../../master-data-services/develop/categorized-web-service-operations-master-data-services.md)  
 <xref:Microsoft.MasterDataServices.ServiceClient> 类的 Web 服务操作的分类列表。  
  
## <a name="custom-workflows"></a>自定义工作流  
 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 使用业务规则来创建基本工作流解决方案。 您可以自动更新和验证数据，并可根据指定的条件发送电子邮件通知。 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 中的业务规则旨在管理最常见的工作流方案。 如果您的工作流需要更复杂的事件处理（如多层审核或复杂决策树），则可配置 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 将数据发送到您创建的自定义程序集。 若要处理自定义工作流，则必须在 web 应用程序计算机上配置和启动 SQL Server MDS Workflow Integration Service，并创建一个实现 [WorkflowTypeExtender IWorkflowTypeExtender](/previous-versions/sql/sql-server-2016/hh758785(v=sql.130)) 接口的程序集。  
  
### <a name="custom-workflow-content"></a>自定义工作流内容  
 [创建自定义工作流 &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-master-data-services.md)  
 有关如何创建工作流处理程序程序集、如何配置和启动 SQL Server MDS Workflow Integration Service 以及如何在启动自定义工作流的[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]中创建业务规则的说明。  
  
## <a name="web-server-namespaces"></a>Web 服务命名空间  
 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 在 Web 服务器计算机上安装一组程序集。 这些程序集包含可用于高级方案（用于自定义 Web 服务器计算机的行为）的命名空间。 下表介绍了这些命名空间。  
  
|命名空间|说明|  
|---------------|-----------------|  
|[MasterDataServices](/previous-versions/sql/sql-server-2016/ff487448(v=sql.130))|包含的类可用于从模型创建部署包或将包部署到 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库中。|  
|<xref:Microsoft.MasterDataServices.Services>|包含的类用于接收和处理通过[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序对 Web 服务器计算机执行的 Web 服务操作。|  
|<xref:Microsoft.MasterDataServices.Services.DataContracts>|包含的类用于定义如何将客户端计算机的数据通过[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序传递到 Web 服务器计算机。|  
|<xref:Microsoft.MasterDataServices.Services.MessageContracts>|包含的类用于定义如何将客户端计算机的请求和响应通过[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序传递到 Web 服务器计算机。|  
|<xref:Microsoft.MasterDataServices.Services.ServiceContracts>|包含用来定义可通过[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 服务调用的操作的接口。|  
  
  
