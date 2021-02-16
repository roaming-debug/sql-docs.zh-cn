---
description: 以编程方式管理正在运行的包
title: 以编程方式管理正在运行的包 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- packages [Integration Services], managing
- running packages [Integration Services]
ms.assetid: 0e91f4ac-6f29-40d7-8c28-9b82e4802c35
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e73d7c3c29ab5e22f45795b9746baedf00bcb413
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347440"
---
# <a name="managing-running-packages-programmatically"></a>以编程方式管理正在运行的包

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  以编程方式使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包时，您可能希望确定哪些包当前正在运行。 <xref:Microsoft.SqlServer.Dts.Runtime.Application> 命名空间的 <xref:Microsoft.SqlServer.Dts.Runtime> 类提供了满足这些需求的方法和类。  
  
 有关监视包的详细信息，请参阅[包管理（SSIS 服务）](../../integration-services/service/package-management-ssis-service.md)。  
  
 本主题中讨论的所有方法都需要引用 **Microsoft.SqlServer.ManagedDTS** 程序集。 在新项目中添加引用后，请使用 **using** 或 **Imports** 语句导入 <xref:Microsoft.SqlServer.Dts.Runtime> 命名空间。  
  
> [!IMPORTANT]  
>  <xref:Microsoft.SqlServer.Dts.Runtime.Application> 类中用于 SSIS 包存储的方法仅支持“.”、localhost 或本地服务器的服务器名称。 不能使用“(local)”。  
  
## <a name="determining-which-packages-are-currently-running"></a>确定当前正在运行的包  
 若要确定指定服务器上哪些包当前正在运行，请调用 <xref:Microsoft.SqlServer.Dts.Runtime.Application.GetRunningPackages%2A> 方法。 此方法返回 <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackages> 对象的 <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage> 集合。  
  
> [!NOTE]  
>  管理员可以看到当前正在该计算机上执行的所有包；其他用户只能看到他们启动的包。  
  
## <a name="working-with-running-packages"></a>使用正在运行的包  
 确定当前正在运行的包后，可以检索有关这些包的信息以及请求停止包。  
  
### <a name="getting-information-about-a-running-package"></a>获取有关正在运行的包的信息  
 遍历 <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackages> 集合时，可以使用 <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage> 对象的以下属性来查找包或者获取有关正在运行的包的其他信息：  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.ExecutionDuration%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.ExecutionStartTime%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.InstanceID%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.PackageDescription%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.PackageID%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.PackageName%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.UserName%2A>  
  
### <a name="stopping-a-running-package"></a>停止正在运行的包  
 可以调用 <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.Stop%2A> 对象的 <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage> 方法来请求停止包。 发出停止请求的时间和包实际停止的时间之间可能存在延迟。  
  
## <a name="see-also"></a>另请参阅  
 [包管理（SSIS 服务）](../../integration-services/service/package-management-ssis-service.md)   
 [以编程方式枚举可用的包](../../integration-services/run-manage-packages-programmatically/enumerating-available-packages-programmatically.md)  
  
  
