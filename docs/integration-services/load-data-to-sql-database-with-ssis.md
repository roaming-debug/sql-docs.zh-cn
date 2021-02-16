---
title: 使用 SQL Server Integration Services (SSIS) 将数据加载到 SQL Server 或 Azure SQL 数据库中 | Microsoft Docs
description: 介绍如何创建 SQL Server Integration Services (SSIS) 包，将数据从各种数据源移到 SQL Server 或 Azure SQL 数据库。
documentationcenter: NA
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
ms.custom: loading
ms.date: 08/20/2018
ms.author: chugu
author: chugugrace
ms.openlocfilehash: 4bdd992e4cea1f2c869f3669905709f91727c874
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074179"
---
# <a name="load-data-into-sql-server-or-azure-sql-database-with-sql-server-integration-services-ssis"></a>使用 SQL Server Integration Services (SSIS) 将数据加载到 SQL Server 或 Azure SQL 数据库中

[!INCLUDE[sqlserver-ssis](../includes/ssis-appliesto-ssvrpluslinux-asdb-xxxx-xxx.md)]

创建 SQL Server Integration Services (SSIS) 包，将数据加载到 SQL Server 或 [Azure SQL 数据库](/azure/sql-database/)中。 可以选择在数据通过 SSIS 数据流时对其进行重构、转换和清理。

本文演示如何完成以下操作：

* 在 Visual Studio 中创建新的 Integration Services 项目。
* 设计可将数据从源加载到目标中的 SSIS 包。
* 运行 SSIS 包以加载数据。

## <a name="basic-concepts"></a>基本概念

包是 SSIS 中的基本工作单位。 相关包在项目中进行分组。 可使用 SQL Server Data Tools 在 Visual Studio 中创建项目并设计包。 设计过程是一个可视化过程，通过该过程，可将工具箱中的组件拖放到设计图面、连接这些组件并设置其属性。 完成后可以运行包，并可选择将其部署到 SQL Server 或 SQL 数据库，从而实现全面管理、监视和安全性。

本文不对 SSIS 做详细介绍。 若要了解详细信息，请参阅以下文章：

- [SQL Server Integration Services](sql-server-integration-services.md)

- [如何创建 ETL 包](ssis-how-to-create-an-etl-package.md)

## <a name="about-the-solution"></a>关于解决方案

解决方案是一种典型包，它使用包含源和目标的数据流任务。 此方法支持各种数据源，包括 SQL Server 和 Azure SQL 数据库。

本教程使用 SQL Server 作为数据源。 SQL Server 可在本地或 Azure 虚拟机上运行。

若要连接到 SQL Server 和 SQL 数据库，可使用 ADO.NET 连接管理器以及源和目标，或 OLE DB 连接管理器以及源和目标。 本教程使用 ADO NET，因为它的配置选项最少。 与 ADO NET 相比，OLE DB 提供的性能稍好。

可使用 SQL Server 导入和导出向导快速创建基本包。 然后保存此包，在 Visual Studio 或 SSDT 中打开包后进行自定义。 有关详细信息，请参阅[使用 SQL Server 导入和导出向导导入和导出数据](import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md)。

## <a name="prerequisites"></a>必备条件
若要逐步完成本教程，需要以下各项：

1. **SQL Server Integration Services (SSIS)** 。 SSIS 是 SQL Server 的一个组件，需要 SQL Server 的许可版、开发人员版或评估版。 要获取 SQL Server 评估版，请参阅[评估 SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-rtm)。
2. Visual Studio（可选）。 要获取免费的 Visual Studio Community Edition，请参阅 [Visual Studio Community][Visual Studio Community]。 如果不想安装 Visual Studio，可以只安装 SQL Server Data Tools (SSDT)。 SSDT 安装的 Visual Studio 版本功能有限。
3. **适用于 Visual Studio 的 SQL Server Data Tools (SSDT)** 。 要获取适用于 Visual Studio 的 SQL Server Data Tools，请参阅[下载 SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)]。
4. 本教程将连接到 SQL Server 或 SQL 数据库实例并向其中加载数据。 必须具有在以下目标之一上连接、创建表和加载数据的权限：
   - Azure SQL 数据库  。 有关详细信息，请参阅 [Azure SQL 数据库](/azure/sql-database/)。  
      或
   - SQL Server 实例  。 SQL Server 可在本地或 Azure 虚拟机上运行。 若要下载 SQL Server 的免费评估版或开发人员版，请参阅 [SQL Server 下载](https://www.microsoft.com/sql-server/sql-server-downloads)。

5. **示例数据**。 本教程使用 AdventureWorks 示例数据库中存储在 SQL Server 中的示例数据，作为源数据。 要获取 AdventureWorks 示例数据库，请参阅 [AdventureWorks 示例数据库][AdventureWorks 2014 Sample Databases]。
6. 要将数据加载到 SQL 数据库中，请参阅 **防火墙规则**。 必须先使用本地计算机的 IP 地址在 SQL 数据库上创建防火墙规则，才可将数据上传到 SQL 数据库。

## <a name="create-a-new-integration-services-project"></a>创建新的 Integration Services 项目
1. 启动 Visual Studio。
2. 在“文件”菜单中，依次选择“新建”、“项目”   。
3. 依次导航到“已安装”、“模板”、“商业智能”、“Integration Services”项目类型  。
4. 选择“Integration Services 项目”  。 提供“名称”和“位置”的值，然后选择“确定”    。

Visual Studio 随即打开，并创建新的 Integration Services (SSIS) 项目。 然后，Visual Studio 在项目中打开单个新 SSIS 包 (Package.dtsx) 的设计器。 可看到以下屏幕区域：

* 左侧是 SSIS 组件的“工具箱”  。
* 中间是包含多个选项卡的设计图面。 通常情况下，至少会使用“控制流”和“数据流”选项卡   。
* 右侧是“解决方案资源管理器”和“属性”窗格   。
  
    ![显示“工具箱”窗格、“设计”窗格、“解决方案资源管理器”窗格和“属性”窗格的 Visual Studio 屏幕截图。][01]

## <a name="create-the-basic-data-flow"></a>创建基本数据流
1. 将“数据流任务”从“工具箱”拖动到“控制流”选项卡上的设计图面。
   
    ![显示将数据流任务拖动到“设计”窗格的“控制流”选项卡的 Visual Studio 屏幕截图。][02]
2. 双击“数据流任务”以便切换到“数据流”选项卡。
3. 从工具箱中的其他源列表中，将 ADO.NET 源拖到设计图面。 如果源适配器仍处于选中状态，请在“属性”窗格中将其名称更改为“SQL Server 源” 。
4. 从“工具箱”的“其他目标”列表中，将 ADO.NET 目标拖动到 ADO.NET 源下的设计图面。 如果目标适配器仍处于选中状态，请在“属性”窗格中将其名称更改为“SQL 目标” 。
   
    ![将目标适配器拖动到源适配器正下方位置的屏幕截图。][09]

## <a name="configure-the-source-adapter"></a>配置源适配器
1. 双击源适配器，打开“ADO.NET 源编辑器”。
   
    ![ADO.NET 源编辑器的屏幕截图。 “连接管理器”选项卡可见，控件可用于配置数据流属性。][03]
2. 在“ADO.NET 源编辑器”的“连接管理器”选项卡上，单击“ADO.NET 连接管理器”列表旁边的“新建”按钮，打开“配置 ADO.NET 连接管理器”对话框，并创建适用于 SQL Server 数据库的连接设置，本教程将从该数据库加载数据    。
   
    ![“配置 ADO.NET 连接管理器”对话框的屏幕截图。 控件可用于设置和配置连接管理器。][04]
3. 在“配置 ADO.NET 连接管理器”对话框中，单击“新建”按钮，打开“连接管理器”对话框并创建新的数据连接  。
   
    ![“连接管理器”对话框的屏幕截图。 控件可用于配置数据连接。][05]
4. 在“连接管理器”对话框中，执行以下操作。
   
   1. 对于“提供程序”，请选择 SqlClient 数据提供程序。
   2. 对于“服务器名称”，请输入 SQL Server 名称。
   3. 在“登录服务器”部分中，选择或输入身份验证信息。
   4. 在“连接到数据库”部分中，选择 AdventureWorks 示例数据库。
   5. 单击 **“测试连接”** 。
      
       ![显示“确定”按钮和指示测试连接成功的文本的对话框的屏幕截图。][06]
   6. 在报告连接测试结果的对话框中，单击“确定”返回“连接管理器”对话框 。
   7. 在“连接管理器”对话框中，单击“确定”返回“配置 ADO.NET 连接管理器”对话框  。
5. 在“配置 ADO.NET 连接管理器”对话框中，单击“确定”返回“ADO.NET 源编辑器”  。
6. 在“ADO.NET 源编辑器”的“表格或视图名称”列表中，选择 Sales.SalesOrderDetail 表  。
   
    ![ADO.NET 源编辑器的屏幕截图。 在“表格或视图名称”列表中，选择 Sales.SalesOrderDetail 表。][07]
7. 单击“预览”在“预览查询结果”对话框中查看源表中的前 200 行数据 。
   
    ![“预览查询结果”对话框的屏幕截图。 源表中的多行销售数据是可见的。][08]
8. 在“预览查询结果”对话框中，单击“关闭”返回“ADO.NET 源编辑器”  。
9. 在“ADO.NET 源编辑器”中，单击“确定”完成配置数据源 。

## <a name="connect-the-source-adapter-to-the-destination-adapter"></a>将源适配器连接到目标适配器
1. 在设计图面上选择源适配器。
2. 选择从源适配器延伸出来的蓝色箭头，并将其拖动到目标编辑器，直到完好入位。
   
    ![显示源和目标适配器的屏幕截图。 从源适配器指向目标适配器的蓝色箭头。][10]
   
    在典型的 SSIS 包中，可在源和目标之间使用 SSIS 工具箱中的许多其他组件，以在数据通过 SSIS 数据流时对其重构、转换和清理。 为了使此示例尽可能简单，我们将源直接连接到目标。

## <a name="configure-the-destination-adapter"></a>配置目标适配器
1. 双击目标适配器，打开“ADO.NET 目标编辑器”。
   
    ![ADO.NET 目标编辑器的屏幕截图。 “连接管理器”选项卡可见，其中包含用于配置数据流属性的控件。][11]
2. 在“ADO.NET 目标编辑器”的“连接管理器”选项卡上，单击“连接管理器”列表旁边的“新建”按钮，打开“配置 ADO.NET 连接管理器”对话框，并创建适用于数据库的连接设置，本教程将向该数据库加载数据    。
3. 在“配置 ADO.NET 连接管理器”对话框中，单击“新建”按钮，打开“连接管理器”对话框并创建新的数据连接  。
4. 在“连接管理器”对话框中，执行以下操作。
   1. 对于“提供程序”，请选择 SqlClient 数据提供程序。
   2. 对于“服务器名称”，输入 SQL Server 或 SQL 数据库服务器的名称。
   3. 在“登录服务器”部分中，选择“使用 SQL Server 身份验证”或输入身份验证信息 。
   4. 在“连接到数据库”部分中，选择现有数据库。
    a. 单击 **“测试连接”** 。
    b. 在报告连接测试结果的对话框中，单击“确定”返回“连接管理器”对话框 。
    c. 在“连接管理器”对话框中，单击“确定”返回“配置 ADO.NET 连接管理器”对话框  。
5. 在“配置 ADO.NET 连接管理器”对话框中，单击“确定”返回“ADO.NET 目标编辑器”  。
6. 在“ADO.NET 目标编辑器”中，单击“使用表格或视图”列表旁边的“新建”，打开“创建表格”对话框，创建包含与源表匹配的列列表的新目标表   。
   
    ![“创建表”对话框的屏幕截图。 用于创建目标表的 S Q L 代码可见。][12a]
7. 在“创建表格”对话框中，执行以下操作。
   
   1. 将目标表的名称更改为 SalesOrderDetail。
      
       ![“创建表”对话框的屏幕截图。 若要创建名为 SalesOrderDetail 的表，则可看到 S Q L 代码。][12b]

   2. 单击“确定”创建表格并返回“ADO.NET 目标编辑器” 。
8. 在“ADO.NET 目标编辑器”中，选择“映射”选项卡以查看源中的列如何映射到目标中的列 。
   
    ![ADO.NET 目标编辑器的“映射”选项卡的屏幕截图。 源表和目标表中具有相同名称的行连接列。][13]
9. 单击“确定”完成配置目标。

## <a name="run-the-package-to-load-the-data"></a>运行包以加载数据
单击工具栏上的“开始”按钮，或在“调试”菜单上选择其中一个“运行”选项，以运行包  。

以下段落介绍了使用本文中所述的第二个选项（即使用包含源和目标的数据流）创建包时所显示的内容。

当包开始运行时，可看到黄色的旋转齿轮，指示活动以及目前为止处理的行数。

![显示源和目标适配器的屏幕截图。 每个适配器上都有黄色的旋转轮，并且它们之间的文本为“89748 行”。][14]

包运行完毕后，可看到绿色的对勾标记，指示操作成功以及从源加载到目标的数据总行数。

![显示源和目标适配器的屏幕截图。 每个适配器上都有绿色的复选标记，并且它们之间的文本为“121317 行”。][15]

祝贺你！ 已成功使用 SQL Server Integration Services 将数据加载到 SQL Server 或 Azure SQL 数据库。

## <a name="next-steps"></a>后续步骤

- 了解如何在设计环境中调试包和排查包问题。 从此处开始：[包开发的疑难解答工具][Troubleshooting Tools for Package Development]。

- 了解如何将 SSIS 项目和包部署到 Integration Services 服务器或其他存储位置。 从此处开始：[项目和包的部署][Deployment of Projects and Packages]。

<!-- Image references -->
[01]:  ./media/load-data-to-sql-database-with-ssis/ssis-designer-01.png
[02]:  ./media/load-data-to-sql-database-with-ssis/ssis-data-flow-task-02.png
[03]:  ./media/load-data-to-sql-database-with-ssis/ado-net-source-03.png
[04]:  ./media/load-data-to-sql-database-with-ssis/ado-net-connection-manager-04.png
[05]:  ./media/load-data-to-sql-database-with-ssis/ado-net-connection-05.png
[06]:  ./media/load-data-to-sql-database-with-ssis/test-connection-06.png
[07]:  ./media/load-data-to-sql-database-with-ssis/ado-net-source-07.png
[08]:  ./media/load-data-to-sql-database-with-ssis/preview-data-08.png
[09]:  ./media/load-data-to-sql-database-with-ssis/source-destination-09.png
[10]:  ./media/load-data-to-sql-database-with-ssis/connect-source-destination-10.png
[11]:  ./media/load-data-to-sql-database-with-ssis/ado-net-destination-11.png
[12a]: ./media/load-data-to-sql-database-with-ssis/destination-query-before-12a.png
[12b]: ./media/load-data-to-sql-database-with-ssis/destination-query-after-12b.png
[13]:  ./media/load-data-to-sql-database-with-ssis/column-mapping-13.png
[14]:  ./media/load-data-to-sql-database-with-ssis/package-running-14.png
[15]:  ./media/load-data-to-sql-database-with-ssis/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[Download SQL Server Data Tools (SSDT)]: ../ssdt/download-sql-server-data-tools-ssdt.md
[Data Flow]: ./data-flow/data-flow.md
[Troubleshooting Tools for Package Development]: ./troubleshooting/troubleshooting-tools-for-package-development.md
[Deployment of Projects and Packages]: ./packages/deploy-integration-services-ssis-projects-and-packages.md

<!--Other Web references-->
[Microsoft SQL Server 2017 Integration Services Feature Pack for Azure]: https://www.microsoft.com/download/details.aspx?id=54798
[SQL Server Evaluations]: https://www.microsoft.com/evalcenter/evaluate-sql-server-2017
[Visual Studio Community]: https://www.visualstudio.com/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks
