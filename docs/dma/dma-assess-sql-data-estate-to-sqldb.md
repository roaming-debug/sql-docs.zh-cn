---
title: 评估要迁移到 Azure SQL Database SQL Server 准备情况
titleSuffix: Data Migration Assistant
description: 了解如何使用数据迁移助手迁移 SQL Server 数据空间迁移到 Azure SQL Database
ms.date: 12/19/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 1094f10eedc819123cd446c0933ec14df511ac75
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247348"
---
# <a name="assess-the-readiness-of-a-sql-server-data-estate-migrating-to-azure-sql-database-using-the-data-migration-assistant"></a>使用数据迁移助手评估迁移到 Azure SQL 数据库的 SQL Server 数据空间的准备情况

将数百个 SQL Server 实例和数千个数据库迁移到 Azure SQL 数据库或 Azure SQL 托管实例，我们的平台即服务 (PaaS) 产品/服务，这是一个很大的任务。 为了尽可能简化该过程，您需要自信地了解迁移的相对就绪性。 确定低挂起的可理解性，包括完全准备就绪的服务器和数据库，或者需要最少的工作量来做好迁移准备，从而使您的工作更加轻松并加速。

本文提供了有关利用 [数据迁移助手](./dma-overview.md) 汇总就绪结果并在 [Azure Migrate](https://portal.azure.com/?feature.customPortal=false#blade/Microsoft_Azure_Migrate/AmhResourceMenuBlade/overview) 中心进行介绍的分步说明。

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

>
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Migration-Assistant/player?WT.mc_id=dataexposed-c9-niner]

## <a name="create-a-project-and-add-a-tool"></a>创建项目并添加工具

在 Azure 订阅中设置新的 Azure Migrate 项目，然后添加工具。

Azure Migrate 项目用于存储从正在评估或迁移的环境中收集的发现、评估和迁移元数据。 你还可以使用项目跟踪发现的资产并安排评估和迁移。

1. 登录到 Azure 门户，选择 " **所有服务**"，然后搜索 Azure Migrate。
2. 在“服务”下选择“Azure Migrate”。 

   ![Azure Migrate-选择服务](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-services.png)

3. 在 " **概述** " 页上，选择 " **评估和迁移数据库**"。

   ![Azure Migrate-启动评估](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-hub-assess.png)

4. 在 " **数据库**" 中，在 "入门 **" 下，** 选择 " **添加工具 (s)**"。

   ![Azure Migrate-添加工具](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-add-tools.png)

5. 在 " **迁移项目** " 选项卡上，选择你的 Azure 订阅和资源组 (如果你还没有资源组，请创建一个) 。
6. 在 " **项目详细信息**" 下，指定要在其中创建项目的项目名称和地理位置。

    ![Azure Migrate "添加工具" 对话框](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-add-tool-dialog.png)

    可在下述任一地理位置创建 Azure Migrate 项目。

    | **地域**  | **存储位置区域** |
    | ------------- | ------------- |
    | 亚洲 | 东南亚或东亚 |
    | 欧洲 | 欧洲南部或西欧 |
    | United Kingdom | 英国南部或英国西部 |
    | United States | 美国中部或美国西部 2 |

    为项目指定的地理位置仅用于存储从本地 VM 中收集的元数据。 可为实际迁移选择任一目标区域。

7. 选择 " **下一步**"，然后添加一个评估工具。

   > [!NOTE]
   > 创建项目时，必须至少添加一个评估或迁移工具。

8. 在 " **选择评估工具** " 选项卡上， **Azure Migrate：数据库评估** 显示为要添加的评估工具。 如果当前不需要评估工具，请选中 " **立即跳过添加评估工具** " 复选框。 选择“**下一页**”。

    ![Azure Migrate-选择评估工具选项卡](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-select-assessment-tool.png)

9. 在 " **选择迁移工具** " 选项卡上， **Azure Migrate：数据库迁移** 显示为要添加的迁移工具。 如果当前不需要迁移工具，请选择 " **立即跳过添加迁移工具**"。 选择“**下一页**”。

    ![Azure Migrate-选择迁移工具选项卡](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-select-migration-tool.png)

10. 在 "查看" 和 " **添加工具**" 中，查看设置，然后选择 " **添加工具**"。

    ![Azure Migrate 审核 + 添加工具 (s) "选项卡](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-review-tools.png)

    创建项目后，可以选择用于评估和迁移服务器、数据库和 web 应用的其他工具。

## <a name="assess-and-upload-assessment-results"></a>评估和上传评估结果

成功创建迁移项目后，请在 " **评估工具**" 下的 " **Azure Migrate：数据库评估** " 框中，下载和使用数据迁移助手工具显示的说明。

   ![添加 Azure Migrate-评估工具](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessment-tool-added.png)

1. 使用提供的链接下载数据迁移助手，并将其安装在可访问源 SQL Server 实例的计算机上。
2. 启动数据迁移助手。

### <a name="create-an-assessment"></a>创建评估

1. 在左侧，选择 "" **+** 图标，然后选择 "评估" **项目类型**
2. 指定项目名称，然后选择 "源服务器" 和 "目标服务器类型"。

    如果要将本地 SQL Server 实例升级到 SQL Server 的更高版本，或在 Azure VM 上托管 SQL Server，请将源和目标服务器类型设置为 " **SQL Server**"。 将 Azure SQL 数据库的目标服务器类型设置为 **AZURE sql 托管实例** (PaaS) 目标准备情况评估。

3. 选择“创建”。

   ![Azure Migrate 数据迁移助手接口](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-interface.png)

### <a name="choose-assessment-options"></a>选择评估选项

1. 选择 "报表类型"。

    您可以选择以下一种或两种报告类型：
    * 检查数据库兼容性
    * 检查功能奇偶校验

   ![Azure Migrate 数据迁移助手评估选项屏幕](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-options-screen.png)

2. 选择“**下一页**”。

### <a name="add-databases-to-assess"></a>添加要评估的数据库

1. 选择 " **添加源** " 以打开 "连接" "弹出" 菜单。
2. 输入 SQL server 实例名称，选择 "身份验证类型"，设置正确的连接属性，然后选择 " **连接**"。
3. 选择要评估的数据库，然后选择 " **添加**"。

   > [!NOTE]
   > 可以通过在按住 Shift 或 Ctrl 键的同时选择多个数据库，然后单击 "删除源" 来删除多个数据库。 还可以通过使用 "添加源" 按钮，从多个 SQL Server 实例添加数据库。

4. 单击“下一步”，以开始评估。

   ![Azure Migrate 数据迁移助手-选择源屏幕](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-select-sources-screen.png)

5. 评估完成后，选择 " **上载到 Azure Migrate**"。

   ![显示数据迁移助手，其中包含上传到 Azure Migrate 选项（称为 out）。](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-review-results-screen.png)

6. 登录到 Azure 门户。

   ![显示 Azure 门户登录窗口的数据迁移助手屏幕截图。](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-portal-signin.png)

7. 选择要将评估结果上载到的订阅和 Azure Migrate 项目，然后选择 " **上传**"。

   等待评估上传确认。

## <a name="view-target-readiness-assessment-results"></a>查看目标准备情况评估结果

1. 登录 Azure 门户，搜索 "Azure 迁移"，然后选择 " **Azure Migrate**"。

   ![Azure Migrate Azure 门户服务搜索](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-portal-search.png)

2. 选择 " **评估和迁移数据库** " 以转到评估结果。

   ![Azure Migrate-查看评估结果](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-hub-assess.png)

    你可以查看 SQL Server 准备情况摘要，确保你处于适当的迁移项目，否则，请使用 change 选项来选择其他迁移项目。

    每次将评估结果更新到 Azure 迁移项目时，Azure 迁移中心将合并所有结果并提供摘要报告。  可以并行执行多个数据迁移助手评估，并将结果上传到单个迁移项目以获取合并的就绪状态报告。

   ![Azure Migrate-查看准备情况结果](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-review-readiness.png)

    **评估的数据库实例**：到目前为止已评估的 SQL Server 实例的数目。
    已 **评估的数据库**：在一个或多个 SQL Server 实例中评估的数据库可 **用于 SQL 数据库** 的数据库总数：已准备好迁移到 Azure sql database (PaaS) 的数据库数。
    **适用于 AZURE SQL VM 的数据库**：数据库数包含一个或多个针对 Azure sql 数据库的迁移阻止程序 (PaaS) ，但可以迁移到 Azure SQL Server vm。

3. 选择 " **评估的数据库实例** "，以转到 SQL Server 实例级别视图。

   ![Azure Migrate 审核实例准备情况](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessed-instances.png)

    可以在 (PaaS) 上查找迁移到 Azure SQL 数据库的每个 SQL Server 实例的准备状态百分比。

4. 选择一个特定实例以转到数据库准备情况视图。

   ![Azure Migrate-查看数据库准备情况](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessed-databases.png)

    可以查看每个数据库的迁移阻塞程序数，以及上述视图中每个数据库的建议目标。

5. 查看 DMA 评估结果以获取有关迁移阻止程序的更多详细信息。

   ![Azure Migrate-查看迁移阻止程序](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-migration-blockers.png)

## <a name="see-also"></a>请参阅

* [数据迁移助手 (DMA)](../dma/dma-overview.md)
* [数据迁移助手：配置设置](../dma/dma-configurationsettings.md)
* [数据迁移助手：最佳实践](../dma/dma-bestpractices.md)
