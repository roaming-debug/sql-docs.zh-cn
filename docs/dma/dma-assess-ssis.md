---
title: 使用数据迁移助手创建 SSIS 迁移评估
description: 了解如何在迁移到 Azure SQL Database 或 Azure SQL 之前，使用数据迁移助手评估本地 SQL Server Integration Service (SSIS) 托管实例
ms.date: 08/23/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: chugugrace
ms.author: chugu
ms.custom: seo-lt-2019
ms.openlocfilehash: e32a7334ecef7531311ead57332f9b7e4d7f6322
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247317"
---
# <a name="perform-a-sql-server-integration-service-migration-assessment-with-data-migration-assistant"></a>使用数据迁移助手执行 SQL Server Integration Service 迁移评估

## <a name="prerequisites"></a>先决条件

若要评估 SQL Server Integration Service (SSIS) 包，需要安装以下组件数据迁移助手：

- SQL Server 集成服务，该服务具有与 SSIS 包相同的版本，以便进行评估。
- Azure 功能包或其他第三方组件。  

DMA 需要使用 **管理员** 访问权限来评估包存储区中的 SSIS 包。

## <a name="performance-assessments"></a>性能评估

下面的分步说明将帮助你执行第一个评估，以便使用数据迁移助手将 SQL Server Integration Service (SSIS) 包迁移到 Azure SQL 数据库或 Azure SQL 托管实例。

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

## <a name="create-an-assessment"></a>创建评估

1. 选择 " **新建** (" +) "图标，然后选择" **评估** "项目类型作为" **Integration Service**"。

1. 设置源和目标服务器类型。

    选择 "源" 作为 " **SQL Server**"，并将 "目标服务器类型" 设置为 " **azure sql 数据库** " 或 " **azure sql 托管实例**"。

1. 单击 **“创建”** 。

    ![创建评估](media/dma-assess-ssis/dma-assess-ssis-create.png)

## <a name="connect-to-a-server"></a>连接到服务器

1. 遵循默认选项，然后单击 " **下一步** " " **选择源**"。
1. 输入 SQL server 实例名称，选择 "身份验证类型"，设置正确的连接属性。
1.  (可选) 输入包含 SSIS 包的文件夹路径。
1.  (可选) 输入包加密密码（如果适用）。
1. 单击 " **连接** 到源 SQL server"。
  ![显示 "连接到服务器" 窗格的屏幕截图，其中包含 "输入包含 SSIS 包的文件夹路径" 选项，如果有适用选项，则输入包加密密码。](media/dma-assess-ssis/dma-assess-ssis-addsource.png)

## <a name="add-sources-to-assess"></a>添加要评估的源

1. 选择要评估的 SSIS 包存储类型，然后选择 " **添加**"。
![显示 "添加源" 窗格的屏幕截图。](media/dma-assess-ssis/dma-assess-ssis-addsource-type.png)
1. 如果需要评估多个文件夹，请选择 " **添加源** " 以打开 "连接" 弹出菜单。
1. 单击“启动评估”。
  ![开始评估](media/dma-assess-ssis/dma-assess-ssis-assess.png)

## <a name="view-results"></a>查看结果

兼容性问题类别提供部分支持或不支持的功能，这些功能会阻止将本地 SSIS 包迁移到 Azure-SSIS Integration Runtime。 然后，它提供了帮助你解决这些问题的建议。

![查看结果](media/dma-assess-ssis/dma-assess-ssis-result.png)

## <a name="next-steps"></a>后续步骤

- [在 ADF 中将本地 SSIS 工作负荷迁移到 SSIS 概述](/azure/data-factory/scenario-ssis-migration-overview)
- [将 SQL Server Integration Services 包迁移到 Azure SQL 托管实例](/azure/dms/how-to-migrate-ssis-packages-managed-instance)
- [将 SQL Server Integration Services 包重新部署到 Azure SQL 数据库](/azure/dms/how-to-migrate-ssis-packages)