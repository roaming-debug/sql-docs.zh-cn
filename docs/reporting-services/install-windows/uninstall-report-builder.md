---
description: 卸载报表生成器
title: 卸载报表生成器 | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: 009538c6-4941-4393-b14b-9144cffdbdaf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3f473c53a8e0a04603a391300fb2fc26dbd8210e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077478"
---
# <a name="uninstall-report-builder"></a>卸载报表生成器

可以从控制面板或命令行卸载报表生成器的独立版本。

除了使用 /x 选项而不是 /i 选项外，从命令行卸载报表生成器所使用的语法与安装时使用的语法完全相同。 用于卸载的命令行也可包括 /quiet 选项和其他标准选项。 如果已删除报表生成器 Windows Installer 包 (ReportBuilder3_x86.msi)，则无法轻松地使用命令行卸载报表生成器。 若要了解有关如何能够通过使用报表生成器的 GUID 将其删除的详细信息，请参阅 [命令行选项](/windows/desktop/Msi/command-line-options)中 msiexec 程序的文档。  

如果报表生成器使用的文件夹中包含自定义文件，则在删除报表生成器时会保留这些文件夹和文件。 仅删除报表生成器文件。  

### <a name="to-uninstall-report-builder-from-the-control-panel"></a>从控制面板卸载报表生成器

1.  在 **“开始”** 菜单上，单击 **“控制面板”** 。  
  
2.  在“控制面板”中，单击 **“程序和功能”**。  
  
3.  在“名称”列表中找到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server 报表生成器并单击它。  
  
4.  单击“卸载”  。  
  
5.  如果提示需要确认卸载报表生成器，请单击 **“是”**。  
  
### <a name="to-uninstall-report-builder-from-the-command-line"></a>从命令行卸载报表生成器  
  
1.  在 **“开始”** 菜单上，单击 **“运行”** 。  
  
2.  在“打开”  文本框中，输入 **cmd.**  
  
3.  在命令提示符窗口中，导航到包含 ReportBuilder3_x86.msi 的文件夹。  
  
4.  键入如下基本命令行：  
  
 `msiexec /x ReportBuilder3_x86.msi /quiet /l*v install.log`  
  
 如果要包括日志记录，请使用如下命令行：  
  
 `msiexec /x ReportBuilder3_x86.msi /quiet /l*v c:\junk\install.log`  
  
5.  按 **Enter**。  

## <a name="next-steps"></a>后续步骤

[安装报表生成器](../../reporting-services/install-windows/install-report-builder.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
