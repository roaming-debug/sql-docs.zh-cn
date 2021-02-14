---
title: 如何：调试自定义程序集 | Microsoft Docs
description: 了解如何使用 Microsoft .NET Framework 调试工具帮助分析自定义程序集代码并查找其中的错误。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- custom assemblies [Reporting Services], debugging
- debugging custom assemblies [Reporting Services]
- troubleshooting [Reporting Services], custom assemblies
ms.assetid: 3a3215b3-548c-4474-81ba-3a98dd3912bf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 255a1007c7a1ebb270c19d92a3129ea444599d41
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064620"
---
# <a name="how-to-debug-custom-assemblies"></a>如何：调试自定义程序集
  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 提供了一些可以帮助分析自定义程序集代码和查找其中错误的调试工具。 要使用的最佳工具取决于您试图完成的任务。 此示例使用 [!INCLUDE[vsOrcas](../../includes/vsorcas-md.md)]。  
  
 设计、开发和测试 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 自定义程序集的建议方式是创建包含您的测试报表和自定义程序集的解决方案。  
  
### <a name="to-debug-assemblies-using-a-single-instance-of-visual-studio"></a>使用 Visual Studio 的单个实例调试程序集  
  
1.  使用 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 创建新的报表项目。  
  
     创建报表项目时，[!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 还会创建一个用以包含该项目的解决方案。  
  
2.  将一个新的类库项目添加到现有解决方案。 请确保该报表项目设置为启动项目。 有关如何实现此操作的详细信息，请参阅 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 文档。  
  
3.  在解决方案资源管理器中，选择解决方案。  
  
4.  在“视图”菜单上，单击“属性页”   。  
  
     “解决方案属性页”对话框会打开  。  
  
5.  在左窗格中，根据需要展开“自定义属性”，然后单击“项目依赖项”   。 从“项目”下拉列表中选择相应的报表项目  。 在“依赖于”列表中选择程序集项目  。  
  
6.  单击“确定”以保存更改，然后关闭“属性页”对话框   。  
  
7.  在解决方案资源管理器中，选择您的自定义程序集项目。  
  
8.  在“视图”菜单上，单击“属性页”   。  
  
     “项目属性页”对话框会打开  。  
  
9. 单击“生成”选项卡（如果处于 C# 项目中）或“编译”选项卡（如果处于 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 项目中）   。  
  
10. 在“生成”/“编译”页上，输入指向报表设计器文件夹的路径   。 默认情况下，在“输出路径”文本框中是 C:\Program Files\Microsoft SQL Server\100\Tools\Binn\VSShell\Common7\IDE  。 这将在执行您的报表之前将自定义程序集的更新的版本直接生成和部署到报表设计器中。  
  
11. 在您设计了报表并开发了自定义程序集后，在自定义程序集代码中设置断点。  
  
12. 通过按下 F5 键在 DebugLocal 模式下运行报表  。 在报表在弹出式预览窗口中执行时，调试器命中与程序集中的可执行代码相对应的任何断点。 使用 F11 可以单步执行您的自定义程序集代码。  
  
### <a name="to-debug-assemblies-using-two-instances-of-visual-studio"></a>使用 Visual Studio 的两个实例调试程序集  
  
1.  启动 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 并打开您的自定义程序集项目。  
  
2.  生成项目，并将自定义程序集以及随附的 .pdb 文件部署到报表设计器。 有关部署的详细信息，请参阅[部署自定义程序集](../../reporting-services/custom-assemblies/deploying-a-custom-assembly.md)。  
  
3.  打开使用您的自定义程序集的报表项目，同时在 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 的一个单独实例中让自定义程序集代码处于打开状态。  
  
4.  导航到 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 中包含自定义程序集项目的实例，并在代码中设置一些断点。  
  
5.  在自定义程序集项目的窗口仍然保持活动状态的同时，在“调试”菜单上单击“附加到进程”   。  
  
     “附加到进程”对话框会打开  。  
  
6.  从进程列表中，选择与报表项目对应的 devenv.exe 进程并单击“附加”  。  
  
7.  定义将用于您的自定义程序集的报表的表达式并设计报表。  
  
8.  在完成报表设计后，单击“预览”选项卡  。  
  
     报表将执行，并且自定义程序集代码应在您预定义的断点处中断。  
  
    > [!NOTE]  
    >  使用“预览”选项卡并不强制对程序集的代码权限  。 对于包括任何代码访问权限错误的完整测试，启动 DebugLocal 配置设置下的报表项目  。  
  
9. 使用 F11 键分步执行代码。 有关使用 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 进行调试的详细信息，请参阅 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 文档。  
  
## <a name="see-also"></a>另请参阅  
 [将自定义程序集用于报表](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)  
  
  
