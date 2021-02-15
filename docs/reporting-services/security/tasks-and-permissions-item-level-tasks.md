---
description: 任务和权限 - 项级任务
title: 项级任务 | Microsoft Docs
ms.date: 02/04/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- item-level tasks [Reporting Services]
ms.assetid: fdeb7bc3-167a-4342-84e3-32e3faa1fa39
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 82101577abe86914aff2ac1d5296d7dc176a60b0
ms.sourcegitcommit: 6f4fb9cfd0cad06127a6328adc745e2ba7c191d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2021
ms.locfileid: "99570431"
---
# <a name="tasks-and-permissions---item-level-tasks"></a>任务和权限 - 项级任务
  
  [!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]
  
  项级任务是一个权限的集合，并且这些权限是与报表、文件夹、报表模型、资源或共享数据源有关的。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 还包括应用于整个报表服务器站点的系统级任务。 有关详细信息，请参阅 [系统级任务](../../reporting-services/security/tasks-and-permissions-system-level-tasks.md)。 有关任务和权限总体情况的详细信息，请参阅 [Tasks and Permissions](../../reporting-services/security/tasks-and-permissions.md)。  
  
> [!NOTE]  
>  如果要通过编程的方式使用这些任务，则必须使用支持项级任务的方法。 有关详细信息，请参阅 <xref:ReportService2010.ReportingService2010.ListTasks%2A> 和 <xref:ReportService2010.ReportingService2010.ListRoles%2A>。  
  
## <a name="permissions-in-item-level-tasks"></a>项级任务中的权限  
 下表列出了项级任务、每个任务所包括的权限以及应用权限的项。 所列出的权限供您参考，仅用于为每个任务的可用功能提供更确切的说明。  
  
 共享数据集使用与报表相同的权限集。 报表部件使用与资源相同的权限集。  
  
|任务|应用到的项|权限|  
|----------|---------------------|-----------------|  
|向报表添加注释<br />（SSRS 2017 和更高版本，Power BI 报表服务器）|报表|读取属性<br /><br /> 创建注释<br /><br /> 删除注释<br /><br /> 读取注释<br /><br /> 更新注释|  
|使用报表|报表|读取内容<br /><br /> 读取报表定义<br /><br /> 读取属性|  
|使用报表|共享数据集|读取内容<br /><br /> 读取报表定义<br /><br /> 读取属性|  
|创建链接报表|报表|创建链接<br /><br /> 读取属性|  
|管理所有订阅|报表|读取属性<br /><br /> 读取任何订阅<br /><br /> 创建任何订阅<br /><br /> 删除任何订阅<br /><br /> 更新任何订阅|  
|管理注释<br />（SSRS 2017 和更高版本，Power BI 报表服务器）|报表|读取属性<br /><br />删除任何注释|  
|管理数据源|文件夹|创建数据源|  
|管理数据源|Data Sources|更新属性<br /><br /> 删除更新内容<br /><br /> 读取属性|  
|管理文件夹|文件夹|创建文件夹<br /><br /> 删除更新属性<br /><br /> 读取属性|  
|管理单独的订阅|报表|读取属性<br /><br /> 创建订阅<br /><br /> 删除订阅<br /><br /> 读取订阅<br /><br /> 更新订阅|  
|管理模型|文件夹|创建模型|  
|管理模型|模型|读取属性<br /><br /> 读取内容<br /><br /> 删除更新内容<br /><br /> 读取数据源<br /><br /> 更新数据源<br /><br /> 读取模型项授权策略<br /><br /> 更新模型项授权策略<br /><br /> 删除更新属性|  
|管理报表历史记录|报表|读取属性<br /><br /> 创建报表历史记录<br /><br /> 删除报表历史记录<br /><br /> 执行读取策略<br /><br /> 更新策略<br /><br /> 列出报表历史记录|  
|管理报表|文件夹|创建报表<br /><br /> 还应用于创建共享数据集|  
|管理报表|报表|读取属性<br /><br /> 删除更新属性<br /><br /> 更新参数<br /><br /> 读取数据源<br /><br /> 更新数据源<br /><br /> 读取报表定义<br /><br /> 更新报表定义<br /><br /> 执行读取策略<br /><br /> 更新策略|  
|管理报表|共享数据集|读取属性<br /><br /> 删除更新属性<br /><br /> 更新参数<br /><br /> 读取数据源<br /><br /> 更新数据源<br /><br /> 读取报表定义<br /><br /> 更新报表定义<br /><br /> 执行读取策略<br /><br /> 更新策略|  
|管理资源|文件夹|创建资源|  
|管理资源|资源|更新属性<br /><br /> 删除更新内容<br /><br /> 读取属性|  
|管理资源|报表部件|更新属性<br /><br /> 删除更新内容<br /><br /> 读取属性|  
|设置各项的安全性|报表、资源、数据源、共享数据集、文件夹|读取安全策略更新安全策略|  
|查看数据源|数据源|读取内容<br /><br /> 读取属性|  
|查看文件夹|文件夹|读取属性<br /><br /> 执行和查看<br /><br /> 列出报表历史记录|  
|查看模型|报表模型|读取属性<br /><br /> 读取内容<br /><br /> 读取数据源|  
|查看报表|报表|读取内容<br /><br /> 读取属性|  
|查看报表|共享数据集|读取内容<br /><br /> 读取属性|  
|查看资源|资源|读取内容<br /><br /> 读取属性|  
|查看资源|报表部件|读取内容<br /><br /> 读取属性|  
  
## <a name="see-also"></a>另请参阅  
 [授予对本机模式报表服务器的权限](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
  
  
