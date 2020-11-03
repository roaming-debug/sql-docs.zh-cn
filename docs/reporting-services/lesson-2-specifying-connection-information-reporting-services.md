---
title: 第 2 课：指定连接信息 (Reporting Services) | Microsoft Docs
description: 在本课中，将定义一个数据源：报表用于从关系数据库或其他源访问数据的连接信息。
ms.date: 12/09/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 54405a3a-d7fa-4d95-8963-9aa224e5901e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a3000abfed5258f02d47148ce351a6390cb225c7
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92678886"
---
# <a name="lesson-2-specifying-connection-information-reporting-services"></a>第 2 课：指定连接信息 (Reporting Services)

在第 1 课中，你向 Tutorial 项目添加了 [!INCLUDE[ssrsnoversion-md](../includes/ssrsnoversion-md.md)] 分页报表。
  
在本课中，将定义数据源  ，这是报表用于访问关系数据库或其他源中的数据的连接信息。

对于此报表，你将添加 AdventureWorks2016 示例数据库作为数据源。 本教程假定数据库位于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../includes/ssde-md.md)] 的默认实例中，并安装在本地计算机上。  

## <a name="to-set-up-a-connection"></a>设置连接  

1. 在“报表数据”  窗格中，选择“新建”   > “数据源”  。 如果“报表数据”  窗格不可见，则选择“视图”  菜单 > “报表数据”  。

    ![“报表数据”窗格的屏幕截图，其中选择了“新建”>“数据源”。](media/ssrs-table-tutorial-2-new-data-source.png)

    “数据源属性”  对话框将打开，并显示“常规”  部分。

    ![“数据源属性”对话框](media/lesson-2-specifying-connection-information-reporting-services/vs-datasource-connection-properties-dialog-box.png)

2. 在“名称”  文本框中，键入“AdventureWorks2016”。

3. 选择“嵌入连接”  单选按钮。

4. 在“类型”  下拉选择框中，选择“Microsoft SQL Server”。
  
5. 在“连接字符串”  文本框中，键入以下字符串：

    `Data source=localhost; initial catalog=AdventureWorks2016`

    > [!NOTE]
    > 该连接字符串假定 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]、报表服务器和 AdventureWorks2016 数据库都已安装在本地计算机中。
    >
    >如果假设不正确，请更改连接字符串，并将“localhost”替换为数据库服务器/实例的名称。 如果使用的是 [!INCLUDE[ssExpress](../includes/ssexpress-md.md)] 或 SQL Server 命名实例，则必须修改连接字符串，以包含实例信息。 例如：
    >
    > `Data source=localhost\SQLEXPRESS; initial catalog=AdventureWorks2016`
    >
    > 有关连接字符串的详细信息，可以参考下面的 `See also` 部分。

6. 选择“凭据”  选项卡，然后在“更改用于连接到数据源的凭据”  部分下，选择“使用 Windows 身份验证(集成安全性)”  单选按钮。

7. 若要完成该过程，请单击“确定”  。

报表设计器将数据源 AdventureWorks2016 添加至“报表数据”  窗格。

![ssrs-adventureworks-datasource](media/lesson-2-specifying-connection-information-reporting-services/ssrs-adventureworks-datasource2016.png)

## <a name="next-steps"></a>后续步骤

在本课中，你已成功定义了到 AdventureWorks2016 示例数据库的连接。 继续学习[第 3 课：为表报表定义数据集 (Reporting Services)](lesson-3-defining-a-dataset-for-the-table-report-reporting-services.md)，为报表定义数据集。

## <a name="see-also"></a>另请参阅

[创建数据连接字符串 - 报表生成器和 SSRS](report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)
