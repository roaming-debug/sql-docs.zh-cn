---
title: 第 1 课：创建报表服务器项目 | Microsoft Docs
description: 在本课程中，将使用报表设计器创建报表服务器项目和报表定义 (.rdl) 文件。
ms.date: 12/09/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 675671ca-e6c9-48a2-82e9-386778f3a49f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: ac84fb8cf355103b28fbac8f5411943aa4ee51a8
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92679048"
---
# <a name="lesson-1-create-a-report-server-project-reporting-services"></a>第 1 课：创建报表服务器项目 (Reporting Services)

在本课程中，将使用报表设计器  创建报表服务器项目  和报表定义 (.rdl)  文件。

> [!NOTE]
> [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] 是用于创建商业智能解决方案的 [!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 环境。 SSDT 是报表设计器创作环境，你可以在其中打开、修改、预览、保存和部署 [!INCLUDE[ssrsnoversion_md](../includes/ssrsnoversion-md.md)] 分页报表定义、共享数据源、共享数据集和报表部件。

使用报表设计器创建报表时，它将创建一个包含报表文件和报表使用的其他资源文件的报表服务器项目。

## <a name="to-create-a-report-server-project"></a>创建报表服务器项目
  
1. 在“文件”  菜单中，选择“新建”   > “项目”  。  

    ![Visual Studio 的屏幕截图，其中选择了“文件”>“新建”>“项目”。](../reporting-services/media/ssrs-ssdt-file-01-new-project.png)
  
2. 在“已安装”  下的最左侧列中，选择“Reporting Services”  。 在某些情况下，可能会在“商业智能”  组下。

    ![“新建项目”对话框的屏幕截图，其中选择了 Reporting Services，并突出显示了“报表服务器项目”模板。](../reporting-services/media/lesson-1-creating-a-report-server-project-reporting-services/select-report-server-project-template.png)

    > [!IMPORTANT]
    > 对于 VS，如果在左侧列中没有看到 Reporting Services，则通过安装 SSDT 工作负载添加报表设计器。 从“工具”  菜单中，选择“获取工具和功能...”  ，然后从显示的工作负载中选择“SQL Server Data Tools”  。 如果没有在中心列中看到 Reporting Services 对象，请添加 Reporting Services 扩展。 从“工具”  菜单中，选择“扩展和更新”   > “联机”  。 在中心列中，从显示的扩展中选择“Microsoft Reporting Services 项目”   > “下载”  。 有关 SSDT，请参阅[下载 SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)。 在 Visual Studio 2019 中，如果前面的步骤不起作用，请尝试安装 [Microsoft Reporting Service 项目扩展](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftReportProjectsforVisualStudio)。


3. 选择“新建项目”对话框中心列中的“报表服务器项目”图标 &nbsp;&nbsp;![ssrs_ssdt_report_server_project](media/ssrs-ssdt-report-server-project.png)&nbsp;&nbsp;。

4. 在“名称”文本框中，键入项目名称“Tutorial”。 默认情况下，“位置”文本框显示指向“Documents\Visual Studio 20xx\Projects\"文件夹的路径。 报表设计器在此路径下创建一个名为“Tutorial”的文件夹，并在该文件夹中创建 Tutorial 项目。 如果项目不属于 VS 解决方案，VS 还将创建解决方案文件 (.sln)。

5. 选择“确定”以创建项目。 Tutorial 项目显示在右侧的“解决方案资源管理器”窗格中。
  
## <a name="creating-a-report-definition-file-rdl"></a>创建报表定义文件 (RDL)  
  
1. 在“解决方案资源管理器”窗格中，右键单击“报表”文件夹。 如果看不到“解决方案资源管理器”窗格，请单击“视图”菜单 > “解决方案资源管理器”。

2. 选择“添加” > “新项”。

    ![解决方案资源管理器的屏幕截图，其中选择了“报表”>“添加”>“新建项目”。](../reporting-services/media/ssrs-ssdt-add-report.png)

3. 在“添加新项”窗口中，选择“报表”图标。

4. 将“Sales Orders.rdl”键入“名称”文本框。

5. 选择“添加新项”对话框右下方的“添加”按钮，完成该过程。 此时报表设计器将打开，并在“设计”视图中显示“Sales Orders”报表文件。

    ![Visual Studio 的屏幕截图，其中显示了报表设计器以及“设计”视图中的销售订单报表。](media/ssrs-ssdt-01-new-report-designer.png)

## <a name="next-steps"></a>后续步骤

到目前为止，你已创建 Tutorial 报表项目和 Sales Orders 报表。 在剩下的课程中，你将了解如何：

- 配置报表数据源。
- 从数据源创建数据集。
- 设计报表布局并设置其格式。

继续学习[第 2 课：指定连接信息 (Reporting Services)](../reporting-services/lesson-2-specifying-connection-information-reporting-services.md)。
