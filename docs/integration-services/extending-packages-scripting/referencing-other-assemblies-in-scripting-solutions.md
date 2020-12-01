---
description: 引用脚本解决方案中的其他程序集
title: 引用脚本解决方案中的其他程序集 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- SSIS Script task, .NET Framework
- Script task [Integration Services], adding references
- referencing custom assemblies
- SSIS Script task, VisualBasic namespace
- assemblies [Integration Services]
- VisualBasic namespace
- Script task [Integration Services], VisualBasic namespace
- Microsoft.VisualBasic namespace
- Script task [Integration Services], .NET Framework
- .NET Framework [Integration Services]
- referencing Web services
ms.assetid: 9b655bcd-19f6-43d8-9f89-1b4d299c6380
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d476bd87096a68be5154cb57563f08ad982e7d3c
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88425319"
---
# <a name="referencing-other-assemblies-in-scripting-solutions"></a>引用脚本解决方案中的其他程序集

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 类库为脚本开发人员提供了一组强大的工具，用于在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包中实现自定义功能。 脚本任务和脚本组件还可以使用自定义托管程序集。  
  
> [!NOTE]
>  若要使包能够使用 Web 服务中的对象和方法，可使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) 中提供的“添加 Web 引用”命令。 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的早期版本中，必须生成代理类才能使用 Web 服务。  
  
## <a name="using-a-managed-assembly"></a>使用托管程序集  
 对于 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]，若要在设计时查找托管程序集，必须执行以下步骤：  
  
1.  将托管程序集存储在计算机上的任何文件夹中。  
  
    > [!NOTE]  
    >  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的早期版本中，只能添加对存储在 %windir%\Microsoft.NET\Framework\vx.x.xxxxx 文件夹或 %ProgramFiles%\Microsoft SQL Server\100\SDK\Assemblies 文件夹中的托管程序集的引用。  
  
2.  添加对托管程序集的引用。  
  
     若要添加引用，请在 VSTA 的“添加引用”对话框的“浏览”选项卡中查找和添加托管程序集。  
  
 对于 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]，若要在运行时查找托管程序集，必须执行以下步骤：  
  
1.  用强名称为托管程序集签名。  
  
2.  将程序集安装到运行包的计算机的全局程序集缓存中。  
  
     有关详细信息，请参阅[生成、部署和调试自定义对象](../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)。  
  
## <a name="using-the-microsoft-net-framework-class-library"></a>使用 Microsoft .NET Framework 类库  
 脚本任务和脚本组件能够利用 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 类库公开的所有其他对象和功能。 例如，使用 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 可以检索有关环境的信息，并与运行包的计算机进行交互。  
  
 下表介绍了一些比较常用的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 类：  
  
-   System.Data 包含 ADO.NET 体系结构。  
  
-   System.IO 提供文件系统和流的接口。  
  
-   System.Windows.Forms 提供窗体创建。  
  
-   System.Text.RegularExpressions 提供处理正则表达式的类。  
  
-   System.Environment 返回有关本地计算机、当前用户以及计算机和用户设置的信息。  
  
-   System.Net 提供网络通信。  
  
-   System.DirectoryServices 公开 Active Directory。  
  
-   System.Drawing 提供丰富的图像处理库。  
  
-   System.Threading 启用多线程编程。  
  
 有关 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 的详细信息，请参阅 MSDN Library。  
  
## <a name="see-also"></a>另请参阅  
 [用脚本扩展包](../../integration-services/extending-packages-scripting/extending-packages-with-scripting.md)  
  
  
