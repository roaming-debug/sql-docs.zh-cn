---
description: 第 3-3 课 — 测试已部署的包
title: 步骤 3：测试已部署的包 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 9159da3f-c9ca-4015-9e85-3bf4373a1349
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 96a06d721c1ccbb4ce254705ceb676c1c338b68a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354928"
---
# <a name="lesson-3-3---testing-the-deployed-packages"></a>第 3-3 课 — 测试已部署的包

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


在此任务中，将测试已部署到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的实例的包。  
  
在其他 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 教程中，可以在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 的开发环境中使用“调试”菜单上的“开始调试”选项运行 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] 中的包。 这一次，将以不同方式运行包。  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 提供了几个可用于在测试和生产环境中运行包的工具：命令提示实用工具 **dtexec** 和执行包实用工具。 执行包实用工具是基于 **dtexec** 构建的图形工具。 这两种工具均可直接执行包。 此外， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 还提供了一个 SQL Server 代理子系统，专门用于将包执行计划为 SQL Server 代理作业中的一个步骤。  
  
将使用执行包实用工具运行已部署的包。 将照原样使用包；因此，您不必更新对话框中任何页上的信息。 您将从“常规”页运行包，该页是执行包实用工具中的第一页。 如果愿意，还可以单击其他页以查看其中包含的有关每个包的信息。  
  
> [!NOTE]  
> 为确保在本教程的上下文中成功运行包，您不应修改任何选项。  
  
通过使用执行包实用工具在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中运行包之前，请确保 Integration Services 服务正在运行。 Integration Services 服务提供对包存储和执行的支持。 如果该服务已停止，则您无法连接到 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] ，而且 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 不列出要运行的包。 您还必须具有在其中已部署包的实例上运行包的权限。 有关详细信息，请参阅 [Integration Services Roles（SSIS 服务）](../integration-services/security/integration-services-roles-ssis-service.md)。  
  
“已存储的包”文件夹内的顶级文件夹是 Integration Services 服务监视的用户定义文件夹。 您可以在 MsDtsSrvr.ini.xml 文件中指定所需数目的文件夹。 本教程假定您使用的是默认 MsDtsSrvr.ini.xml 文件，并假定“已存储的包”文件夹内的顶级文件夹的名称是“文件系统”和“MSDB”。  
  
### <a name="to-connect-to-integration-services-in-sql-server-management-studio"></a>在 SQL Server Management Studio 中连接到 Integration Services  
  
1.  单击 **“开始”** ，依次指向 **“所有程序”** 和 **Microsoft SQL Server**，然后单击 **SQL Server Management Studio**。  
  
2.  在“连接到服务器”对话框中，选择“服务器类型”列表中的“Integration Services”，在“服务器名称”框中提供服务器名称，再单击“连接”。  
  
    > [!IMPORTANT]  
    > 如果无法连接到 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]，则 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 服务可能未运行。 若要了解该服务的状态，请单击 **“开始”** ，依次指向 **“所有程序”** 、 **Microsoft SQL Server** 和 **“配置工具”** ，再单击 **“SQL Server 配置管理器”** 。 在左窗格中，单击 **“SQL Server 服务”** 。 在右窗格中，查找 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 服务。 如果该服务尚未运行，请启动该服务。  
  
    [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 。 默认情况下，对象资源管理器窗口是打开的，且位于 Studio 的右上角。 如果对象资源管理器未打开，请单击 **“视图”** 菜单上的 **“对象资源管理器”** 。  
  
### <a name="to-run-the-packages-using-the-execute-package-utility"></a>使用执行包实用工具运行包  
  
1.  在对象资源管理器中，展开“已存储的包”文件夹。  
  
2.  展开 MSDB 文件夹。 由于您已将包部署到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]，因此所有已部署的包都存储在 msdb [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库中，并出现在 MSDB 文件夹中。 除非已将包部署到 Deployment Tutorial 外部的文件系统，否则“文件系统”文件夹为空。  
  
3.  从包列表的顶部开始，右键单击 DataTransfer，再单击“运行包”。  
  
4.  在“执行包实用工具”对话框中，单击“执行”。  
  
5.  在“执行包实用工具”对话框中，查看包的执行进度和执行结果。 当“停止”按钮变为不可用时（表示包的执行已完成），单击“关闭”。  
  
    > [!IMPORTANT]  
    > 如果在包正在执行时单击“停止”，则包的执行将无法完成。  
  
6.  在“执行包实用工具”对话框中，单击“关闭”。  
  
7.  对于 LoadXML 包，重复步骤 3-6。  
  
8.  在 **“文件”** 菜单中，单击 **“退出”** 。  
  
### <a name="to-verify-the-results-of-the-datatransfer-package"></a>验证 DataTransfer 包的结果  
  
1.  在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 的工具栏上，单击“新建查询”。  
  
2.  在“连接到服务器”对话框中，选择“服务器类型”列表中的“数据库引擎”，在“服务器名称”框中提供在其上已安装教程包的服务器的名称或键入 (local)，再选择身份验证模式。 如果使用 SQL Server 身份验证，请提供用户名和密码。  
  
3.  单击“连接”。  
  
4.  在查询窗口中，键入或粘贴以下 SQL 语句：  
  
    `USE AdventureWorks`  
  
    `SELECT * FROM HighIncomeCustomers`  
  
5.  按 **F5** 或单击工具栏上的“执行”图标。  
  
    查询将返回 31 行数据。 返回结果包含了文本文件 Customers.txt 的 YearlyIncome 列中值大于 100000 的任何行。  
  
6.  找到 DeploymentTutorial 文件夹，右键单击日志 XML 文件 Deployment Tutorial Log，再单击“打开”。 可以使用记事本或所选的文本/XML 编辑器打开该文件。  
  
### <a name="to-verify-the-results-of-the-loadxmldata-package"></a>验证 LoadXMLData 包的结果  
  
1.  在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 的工具栏上，单击“新建查询”。  
  
2.  如果再次提示进行连接，请在“连接到服务器”对话框中，选择“服务器类型”列表中的“数据库引擎”，在“服务器名称”框中提供在其上已安装教程包的服务器的名称或输入 (local)，再选择身份验证模式。 如果使用 SQL Server 身份验证，请提供用户名和密码。  
  
3.  单击“连接”。  
  
4.  在查询窗口中，键入或粘贴以下 SQL 语句：  
  
    `USE AdventureWorks`  
  
    `SELECT * FROM OrderDatesByCountryRegion`  
  
5.  按 **F5** 或单击工具栏上的“执行”图标。  
  
    查询将返回 21 行数据。 返回结果中包含来自 XML 数据文件 orders.xml 的行。 在每行中按国家/地区进行了汇总；行中列出了国家/地区的名称、每个国家/地区的订单数以及最新订单和最早订单的日期。  
  
## <a name="see-also"></a>另请参阅  
[dtexec 实用工具](../integration-services/packages/dtexec-utility.md)  
  
  
  
