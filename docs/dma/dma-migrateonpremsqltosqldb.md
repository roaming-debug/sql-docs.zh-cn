---
title: 使用数据迁移助手将 SQL Server 迁移到 Azure SQL Database
description: 了解如何使用数据迁移助手将本地 SQL Server 迁移到 Azure SQL 数据库
ms.date: 06/29/2020
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
ms.openlocfilehash: 7bd846f4c4abd2a6b9fbcdc434964b789f32d047
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030635"
---
# <a name="migrate-on-premises-sql-server-or-sql-server-on-azure-vms-to-azure-sql-database-using-the-data-migration-assistant"></a>使用数据迁移助手将 Azure Vm 上的本地 SQL Server 或 SQL Server 迁移到 Azure SQL 数据库

数据迁移助手提供对本地 SQL Server 的无缝评估，并可升级到 Azure Vm 或 Azure SQL 数据库上的 SQL Server 或 SQL Server 迁移到更高版本。

本文提供使用数据迁移助手将本地 SQL Server 迁移到 Azure SQL 数据库的分步说明。

## <a name="create-a-new-migration-project"></a>创建新迁移项目

1. 在左侧窗格中，选择 " **新建** (" +) "，然后选择" **迁移** "项目类型。

2. 将 "源类型" 设置为 " **SQL Server** "，将 "目标服务器" 类型设置为 " **Azure SQL 数据库**"。

3. 选择“创建”。

   ![创建迁移项目](../dma/media/NewCreate1.png)

## <a name="specify-the-source-server-and-database"></a>指定源服务器和数据库

1. 对于 "源"，在 " **连接到源服务器**" 下的 " **服务器名称** " 文本框中，输入源 SQL Server 实例的名称。

2. 选择源 SQL Server 实例支持的“身份验证类型”。

   > [!NOTE]
   > 建议通过选中 "**连接 poperties**" 下的 "**加密连接**" 复选框对连接进行加密。

    ![选择源服务器](../dma/media/select-source-server.png)

3. 选择“连接”  。

4. 选择要迁移到 Azure SQL 数据库的单个源数据库。

   > [!NOTE]
   > 如果要在迁移之前评估数据库并查看和应用建议的修复，请选中 " **迁移之前评估数据库？** " 复选框。

    ![选择源数据库](../dma/media/select-source-database.png)

5. 选择“**下一页**”。

## <a name="specify-the-target-server-and-database"></a>指定目标服务器和数据库

1. 对于目标，在 " **连接到目标服务器**" 下的 " **服务器名称** " 文本框中，输入 Azure SQL 数据库实例的名称。 

2. 选择目标 Azure SQL 数据库实例支持的 **身份验证类型** 。

   > [!NOTE]
   > 建议通过选中 "**连接 poperties**" 下的 "**加密连接**" 复选框对连接进行加密。

     ![选择目标服务器](../dma/media/select-target-server.png)

3. 选择“连接”  。

4. 选择要迁移到的单个目标数据库。

   > [!NOTE]
   > 如果要迁移 Windows 用户，请在 " **目标外部用户域名** " 文本框中，确保正确指定了目标外部用户域名。

    ![选择目标数据库](../dma/media/select-target-database.png)

5. 选择“**下一页**”。

## <a name="select-schema-objects"></a>选择架构对象

1. 从源数据库中选择要迁移到 Azure SQL 数据库的架构对象。

    ![选择架构对象](../dma/media/select-schema-objects.png)

    > [!NOTE]
    > 一些无法按原样转换的对象会提供自动修复机会。 单击左窗格中的这些对象将在右窗格中显示建议的修补程序。 查看修补程序并按逐个对象地应用或忽略所有更改。 请注意，应用或忽略一个对象的所有更改不会影响对其他数据库对象的更改。 无法转换或自动修复的语句将复制到目标数据库并进行注释。

    ![建议的修复](../dma/media/suggested-fix.png)

2. 选择 " **常规 SQL 脚本**"。

3. 查看生成的脚本。

    ![生成的脚本](../dma/media/generated-script.png)

## <a name="deploy-schema"></a>部署架构

1. 选择 " **部署架构**"。

2. 查看架构部署的结果。

    ![架构部署结果](../dma/media/schema-deployment-results.png)

3. 选择 " **迁移数据** " 以启动数据迁移过程。

4. 选择具有要迁移的数据的表。

    ![选择要迁移的表](../dma/media/select-tables-to-migrate.png) 

5. 选择 " **开始数据迁移**"。

最终屏幕显示整体状态。

   ![迁移状态](../dma/media/migration-status.png) 

## <a name="see-also"></a>另请参阅

* [数据迁移助手 (DMA)](../dma/dma-overview.md)
* [数据迁移助手：配置设置](../dma/dma-configurationsettings.md)
* [数据迁移助手：最佳实践](../dma/dma-bestpractices.md)
