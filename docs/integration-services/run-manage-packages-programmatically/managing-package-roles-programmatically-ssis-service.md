---
description: 以编程方式管理包角色（SSIS 服务）
title: 以编程方式管理包角色（SSIS 服务）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Integration Services packages, roles
- roles [Integration Services]
- packages [Integration Services], roles
ms.assetid: 2e0ca0d5-d4f5-421d-b17d-a48b37b923e5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 33922748916193e875799839fbbf6fe0f7a76a26
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88495508"
---
# <a name="managing-package-roles-programmatically-ssis-service"></a>以编程方式管理包角色（SSIS 服务）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  以编程方式使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包时，您可能希望确定哪些角色可以应用于包，或确定或设置应用于各个包的角色。 <xref:Microsoft.SqlServer.Dts.Runtime.Application> 命名空间的 <xref:Microsoft.SqlServer.Dts.Runtime> 类提供了多种满足这些要求的方法。  
  
 角色仅适用于存储在  msdb 数据库中的包[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关包角色的详细信息，请参阅 [Integration Services 角色（SSIS 服务）](../../integration-services/security/integration-services-roles-ssis-service.md)。  
  
 本主题中讨论的所有方法都需要引用 **Microsoft.SqlServer.ManagedDTS** 程序集。 在新项目中添加引用后，请使用 using 或 Imports 语句导入 <xref:Microsoft.SqlServer.Dts.Runtime> 命名空间。  
  
> [!IMPORTANT]  
>  <xref:Microsoft.SqlServer.Dts.Runtime.Application> 类中用于 SSIS 包存储的方法仅支持“.”、localhost 或本地服务器的服务器名称。 不能使用“(local)”。  
  
## <a name="determining-which-roles-are-available"></a>确定哪些角色可用  
 若要确定哪些角色可用于存储在特定服务器上的包，可调用 <xref:Microsoft.SqlServer.Dts.Runtime.Application.GetDtsServerRoles%2A> 类的 <xref:Microsoft.SqlServer.Dts.Runtime.Application> 方法。  
  
## <a name="determining-which-roles-are-assigned"></a>确定哪些角色已分配  
 若要确定哪些角色已经分配给了特定包，可调用 <xref:Microsoft.SqlServer.Dts.Runtime.Application.GetPackageRoles%2A> 方法。 若要将角色分配给包，可调用 <xref:Microsoft.SqlServer.Dts.Runtime.Application.SetPackageRoles%2A> 方法。  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 角色（SSIS 服务）](../../integration-services/security/integration-services-roles-ssis-service.md)  
  
  
