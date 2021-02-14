---
title: 使用 Azure 资源浏览器浏览 Azure SQL 资源
description: 了解如何通过 Azure 资源浏览器浏览和管理 Azure SQL Server、Azure SQL 数据库以及 Azure SQL 托管实例。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: yanancai
ms.author: yanacai
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 5d28054e9f459bc3334a863e7ee5edb3bd98afea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048727"
---
# <a name="explore-and-manage-azure-sql-resources-with-azure-resource-explorer"></a>使用 Azure 资源浏览器浏览和管理 Azure SQL 资源

在本文档中，将了解如何通过 [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)] 中的 Azure 资源浏览器浏览和管理 Azure SQL Server、Azure SQL 数据库以及 Azure SQL 托管实例资源。

>[!NOTE]
>SQL Server 2019 支持 Azure 资源浏览器。 之后，可通过[扩展管理器](./extensions/add-extensions.md)或通过“文件” > “从 VSIX 包安装包”安装扩展。

## <a name="connect-to-azure"></a>连接到 Azure

安装 SQL 插件后，左侧菜单栏中会出现一个 Azure 图标。 单击该图标可打开 Azure 资源浏览器。 如果没有看到 Azure 图标，请右键单击左侧菜单栏，然后​​选择“Azure 资源浏览器”  。

### <a name="add-an-azure-account"></a>添加 Azure 帐户

若要查看与某个 Azure 帐户关联的 SQL 资源，必须先将该帐户添加到 [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)]。

1. 通过左下角的帐户管理图标或通过 Azure 资源浏览器中的“登录 Azure...”链接，打开“已关联帐户”对话框   。

    ![登录到 Azure](media/azure-resource-explorer/sign-in-to-azure.png)

2. 在“已关联帐户”对话框中，单击“添加帐户”。

    ![添加 Azure 帐户](media/azure-resource-explorer/add-an-azure-account.png)

3. 单击“复制并打开”以打开浏览器进行身份验证  。

    ![在浏览器中打开身份验证页](media/azure-resource-explorer/open-authentication-in-browser.png)

4. 将“用户代码”粘贴到网页中，然后单击“继续”进行身份验证   。

    ![在浏览器中进行身份验证](media/azure-resource-explorer/authenticate-in-browser.png)

5. 在 [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)] 中，现在应该可以在“已关联帐户”对话框中找到已登录的 Azure 帐户  。

    ![已登录的 Azure 帐户](media/azure-resource-explorer/signed-in-azure-account.png)

### <a name="add-more-azure-accounts"></a>添加更多 Azure 帐户

[!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)] 支持多个 Azure 帐户。 若要添加更多 Azure 帐户，请单击“已关联帐户”对话框右上方的按钮，按照“添加 Azure 帐户”部分中的相同步骤添加更多 Azure 帐户  。

![添加更多 Azure 帐户](media/azure-resource-explorer/add-more-azure-account.png)

### <a name="remove-an-azure-account"></a>删除 Azure 帐户

若要删除现有的已登录的 Azure 帐户，请执行以下操作：

1. 通过左下方的帐户管理图标打开“已关联帐户”对话框  。
2. 单击 Azure 帐户右侧的“X”按钮将其删除  。

    ![删除 Azure 帐户](media/azure-resource-explorer/remove-azure-account.png)

## <a name="filter-subscription"></a>筛选订阅

登录某个 Azure 帐户后，与该 Azure 帐户关联的所有订阅都将显示在 Azure 资源浏览器中。 可以筛选每个 Azure 帐户的订阅。

1. 单击 Azure 帐户右侧的“选择订阅”按钮  。

   ![筛选订阅](media/azure-resource-explorer/filter-subscription.png)

2. 选中要浏览的帐户订阅的复选框，然后单击“确定”  。

   ![选择订阅](media/azure-resource-explorer/select-subscription.png)

## <a name="explore-azure-sql-resources"></a>浏览 Azure SQL 资源

若要在 Azure 资源浏览器中导航 Azure SQL 资源，请展开 Azure 帐户和资源类型组。

Azure 资源浏览器目前支持 Azure SQL Server、Azure SQL 数据库和 Azure SQL 托管实例。

## <a name="connect-to-azure-sql-resources"></a>连接到 Azure SQL 资源

Azure 资源浏览器提供快速访问，可帮助你连接到 SQL Server 和数据库以进行查询和管理。

1. 从树状视图中浏览要连接的 SQL 资源。
2. 右键单击资源并选择“连接”，也可以在资源右侧找到连接按钮  。

   ![连接到 Azure SQL 资源](media/azure-resource-explorer/connect-to-azure-sql-resource.png)

3. 在打开的“连接”对话框中，输入密码并单击“连接”   。

   ![SQL 连接对话框](media/azure-resource-explorer/sql-connection-dialog.png)
4. 连接成功后，“服务器”窗口会自动打开新连接的 SQL Server/数据库  。

## <a name="next-steps"></a>后续步骤

- [使用 [!INCLUDE[Azure Data Studio](../includes/name-sos-short.md)] 连接并查询 Azure SQL 数据库](quickstart-sql-database.md)
- [使用 [!INCLUDE[Azure Data Studio](../includes/name-sos-short.md)] 在 Azure Synapse Analytics 中连接和查询数据](quickstart-sql-dw.md)