---
description: 正在执行操作（SQL Server 导入和导出向导）
title: 执行操作（SQL Server 导入和导出向导）| Microsoft Docs
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.impexpwizard.performingoperation.f1
ms.assetid: 83259509-71d6-4a64-a7f2-4e9603b30bd4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cda6ec744ca603c1a03df22bcb391b6d75bbff58
ms.sourcegitcommit: fb8724fb99c46ecf3a6d7b02a743af9b590402f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92439341"
---
# <a name="performing-operation-sql-server-import-and-export-wizard"></a>正在执行操作（SQL Server 导入和导出向导）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


查看在向导中所做的选择，并单击“完成向导”页中的“完成”之后  ，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导将显示“执行操作”  。 在此页上，会看到在前页配置的操作的进度和结果。 不必在此页上执行任何操作。

## <a name="screen-shot---operation-in-progress"></a>屏幕截图 - 正在进行中的操作 
 以下屏幕快照显示操作进行过程中，向导的“执行操作”页  。  
  
 ![导入和导出向导的执行操作页](../../integration-services/import-export-data/media/performing-operation1.png "导入和导出向导的执行操作页")  

## <a name="screen-shot---operation-completed"></a>屏幕截图 - 已完成的操作 
 以下屏幕快照显示操作完成后，向导的“执行操作”页  。 单击“消息”列中的项  可获取有关相应步骤的详细信息。  
  
 ![“导入和导出向导”的成功页面的屏幕截图。](../../integration-services/import-export-data/media/performing-operation2.png "导入和导出向导的执行操作页")  
  
## <a name="watch-the-progress-of-the-operation"></a>监视操作的进度
 **操作**  
 显示操作的每个步骤。  
  
 **Status**  
 显示各步骤是成功还是失败。  
  
 **Message**  
 显示有关步骤的信息性消息和错误消息。 单击该列中的项可获取有关相应步骤的详细信息。

## <a name="interrupt-the-operation-or-save-the-results"></a>中断操作或保存结果
 **停止**  
 在必要时单击“停止”  按钮即可中断操作。  
  
 **Report**  
 查看结果报表，将报表保存到文件，将报表复制到剪贴板或通过电子邮件发送报表。  
  
## <a name="whats-next"></a>后续步骤  
 运行并成功完成了配置的操作后，即表示已完成运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导。  
-   若要立即运行该操作，可以打开所选目标，以查看向导复制的数据。  
-   如果保存了由向导创建的 SSIS 包，则可以在 SQL Server Data Tools 中打开该包，对其进行自定义和重复使用。 有关如何自定义已保存的包和稍后再次运行该包的信息，请参阅 [保存 SSIS 包](../../integration-services/import-export-data/save-ssis-package-sql-server-import-and-export-wizard.md)。

## <a name="see-also"></a>另请参阅
[导入和导出向导的简单示例入门](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)


