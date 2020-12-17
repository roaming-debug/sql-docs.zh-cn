---
title: 安装和管理功能扩展
description: 了解如何安装功能扩展，以便可以增强 SQL Server Data Tools 的功能。 了解安装不同类型的扩展的位置。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9cdc8cd5-c36f-4bee-a191-87ed457803e7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: 19bdca4ab4b380d5a971078eb8e264cb409caa7c
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559079"
---
# <a name="how-to-install-and-manage-feature-extensions"></a>如何：安装和管理功能扩展

你可以添加用于分析数据库代码的规则、用于数据库单元测试和生成/部署参与者的条件，以增强 Visual Studio 版本（包括 SQL Server Data Tools）提供的功能。 但是，无论你是创建了扩展还是安装了其他人创建的扩展，必须首先安装功能扩展，之后才能使用它。  
  
安装扩展的位置取决于扩展类型和计划使用它的位置。 在 Visual Studio 的最新版本中，某些组件的安装位置已从 SQL Server 安装目录移动到 Visual Studio 目录内。 此安装程序使不同版本的软件更容易并排运行，但这意味着如果你想要在不同版本的 SQL Server Data Tools 中和通过命令行使用扩展，你可能需要将其安装在多个位置中。  
  
### <a name="installing-extensions-for-use-inside-visual-studio"></a>安装扩展以供在 Visual Studio 内使用  
  
|扩展类型|安装位置|  
|------------------|--------------------|  
|SQL Server 单元测试的自定义测试条件|<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\TestConditions|  
|生成参与者<br /><br />部署参与者<br /><br />静态代码分析规则|<Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions|  
  
<Visual Studio Install Dir> 将有所不同，具体取决于你使用的是哪个版本的 Visual Studio 以及你选择安装它的位置。 对于 Visual Studio 2012，通常位于 C:\Program Files (x86)\\MicrosoftVisual Studio 11.0 中。 对于 Visual Studio 2013，通常位于 C:\Program Files (x86)\\MicrosoftVisual Studio 12.0 中。  
  
扩展可以作为命令行服务的一部分运行：  
  
|扩展类型|命令行服务|安装文件夹|  
|------------------|------------------------|------------------|  
|SQL Server 单元测试的自定义测试条件|MSBuild/MSTest 可用于从 Visual Studio 2013 和类似命令行工具的开发人员命令提示符处运行单元测试。|在 Visual Studio 内运行时也是如此。|  
|生成参与者<br /><br />部署参与者|[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md)，或通过在生成数据库项目时使用 MSBuild 部署或发布目标。|MSBuild：在 Visual Studio 内运行时也是如此。<br /><br />[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md)：如果位于 Visual Studio 目录中，则与之前相同。<br /><br />如果 SqlPackage.exe 和其他 DacFx DLLs 位于该目录之外，则扩展应放置在相同的目录或 C:\Program Files (x86)\\MicrosoftSQL Server\120\DAC\bin\Extensions 中。|  
|静态代码分析规则|MSBuild 可用于生成项目和运行静态代码分析。<br /><br />此外，可使用 CodeAnalysisService API 从你自己的应用程序运行代码分析。 在这种情况下，扩展查找规则与使用 SqlPackage.exe 时的作用相同。|对于生成和部署参与者也是如此。|  
  
> [!NOTE]  
> 若要访问“程序文件”文件夹下的任何安装目录，你必须在你的计算机上拥有管理员权限。 如果没有相应权限，请与网络管理员联系。  
  
**安全注意事项**  
  
在安装你未创建的扩展前，应了解以下风险：  
  
-   扩展安装程序可能为恶意程序，并且可能会基于安装权限获取对受保护资源的访问权限。  
  
-   扩展本身可能是恶意的，如果使用扩展的用户具有足够权限，它可能获得受保护资源的控制权。  
  
要尽可能减小风险，你应仅安装来自已知来源的扩展。 如果获得来自不受信任的源的扩展，应在安装和使用该扩展前检查它的源代码及其安装程序（如果有）。  
  
## <a name="to-install-a-custom-feature-extension"></a>安装自定义功能扩展  
将已签名程序集 (.dll) 复制到正确的安装文件夹。 关闭并重新打开 Visual Studio。 扩展现应可用。  
  
## <a name="see-also"></a>另请参阅  
[扩展数据库功能](../ssdt/extending-the-database-features.md)  
  
