---
description: 第 3-2 课：添加和配置日志记录
title: 步骤 2：添加和配置日志记录 | Microsoft Docs
ms.custom: ''
ms.date: 01/04/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 56105f3f-e500-4669-8c8e-acf434527727
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 32daaf012159bd42a6f73330bc8371157e459609
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125604"
---
# <a name="lesson-3-2-add-and-configure-logging"></a>第 3-2 课：添加和配置日志记录

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



在本任务中，为 Lesson 3.dtsx 包中的数据流启用日志记录。 然后，配置一个文本文件日志提供程序，以记录 PipelineExecutionPlan 和 PipelineExecuteTrees 事件。 该文本文件日志提供程序可以创建便于查看和传输的日志。 由于便于使用，因此，这些日志文件在包的基本测试阶段很有用。 也可以在 [!INCLUDE[ssIS](../includes/ssis-md.md)] 设计器的“日志事件”窗口中查看日志条目  。  
  
## <a name="add-logging-to-the-package"></a>向包中添加日志记录  
  
1.  在“SSIS”菜单中，选择“日志记录”   。  
    在 Visual Studio 2019 中，SSIS 菜单位于“扩展”>“SSIS”下。 确保选中“数据流”选项卡，而不是“控制流” 
  
2.  在“配置 SSIS 日志”对话框的“容器”窗格中，确保选中 topmost 对象   。 此对象代表第 3 课包。
  
3.  在“提供程序和日志”选项卡的“提供程序类型”框中，选择“用于文本文件的 SSIS 日志提供程序”，然后选择“添加”     。  
  
    Integration Services 将向包中添加一个默认名称为“用于文本文件的 SSIS 日志提供程序”的新文本文件日志提供程序  。 现在便可对新的日志提供程序进行配置。  
  
4.  在“名称”列中，输入“Lesson 3 Log File”   。  
  
5.  也可以修改“说明”  。  
  
6.  在“配置”列中，选择“\<New Connection>”以指定 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 写入日志信息的位置 。  
  
    在“文件连接管理器编辑器”对话框中，对“使用类型”选择“创建文件”，然后选择“浏览”     。 默认情况下，“选择文件”对话框将打开项目文件夹，但你可以将日志信息保存到任何位置  。  
  
7.  在“选择文件”对话框的“文件名”框中，输入 TutorialLog.log，然后选择“打开”     。
  
8.  选择“确定”，关闭“文件连接管理器编辑器”对话框   。  
  
9. 在“容器”  窗格中，展开包容器层次结构中的所有节点，然后清除包括 **Extract Sample Currency Data** 复选框在内的所有复选框。 现在选中“Extract Sample Currency Data”  复选框以仅获取有关此节点的事件。  
  
    > [!NOTE]  
    > 如果“Extract Sample Currency Data”复选框显示为灰色而并非选中状态，则该任务将使用父容器的日志设置，无法启用任务特定的日志事件  。 要解决此实例，请清除父级复选框。
  
10. 在“详细信息”  选项卡的“事件”  列中，选择“PipelineExecutionPlan”  和“PipelineExecutionTrees”  事件。  
  
11. 选择“高级”可查看日志提供程序为每个事件写入日志的详细信息  。 默认情况下，将为您指定的事件自动选择所有信息类别。  
  
12. 选择“基本”可隐藏信息类别  。  
  
13. 在“提供程序和日志”  选项卡上的“名称”  列中，选择“Lesson 3 Log File”  。 为包创建日志提供程序后，可以选择取消选择它以关闭日志记录，而不必删除后再重新创建日志提供程序。  
  
14. 选择“确定”  。  
  
## <a name="go-to-next-task"></a>转到下一个任务  
[步骤 3：测试第 3 课包](../integration-services/lesson-3-3-testing-the-lesson-3-tutorial-package.md)  
  
