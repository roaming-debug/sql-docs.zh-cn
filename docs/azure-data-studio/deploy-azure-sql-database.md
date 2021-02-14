---
title: 通过笔记本部署 Azure SQL 数据库
description: 本教程介绍如何创建 Azure SQL 数据库。
author: ninarn
ms.author: ninarn
ms.reviewer: alayu, markingmyname
ms.topic: tutorial
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.custom: ''
ms.date: 10/16/2020
ms.openlocfilehash: 8bac1e0d10208283e40843288ab7dbe7d042b8ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052208"
---
# <a name="create-an-azure-sql-database-using-azure-data-studio"></a>使用 Azure Data Studio 创建 Azure SQL 数据库

可以使用 Azure Data Studio 通过部署向导和笔记本创建 Azure SQL 数据库。

## <a name="pre-requisites"></a>先决条件

 - 已安装 [Azure Data Studio](download-azure-data-studio.md)
 - 有效的 Azure 帐户和订阅。 如果没有，请[创建一个免费帐户](https://azure.microsoft.com/free/)。

## <a name="use-the-deployment-wizard"></a>使用部署向导

按照以下步骤使用部署向导，它将指导你在简单的 UI 体验中完成所需设置。

首先，在部署向导中找到并选择“Azure SQL 数据库”。

 1. 在 Azure Data Studio 中，在左侧导航栏中选择“连接 Viewlet”。
 2. 选择“连接”面板顶部的“...”按钮，然后选择“新建部署...” 
 3. 在部署向导中，选择“Azure SQL 数据库”磁贴，并选中“接受条款”复选框
 4. 在“资源类型”下拉列表中，选择要创建的内容（数据库、数据库服务器或弹性池）。 如果 Azure 中没有任何 SQL 数据库，我们建议首先创建一个数据库。
 5. 如果选择创建服务器或池，则选择“在 Azure 门户创建”，如果选择创建数据库，则选择“选择”。

如果选择创建数据库，请执行以下步骤。

 1. 登录到 Azure 帐户（如果尚未登录）。 如果向导的此页上出现问题，可以刷新连接。
 2. 选择所需的订阅和服务器。 然后，选择“下一步”。
 3. 输入数据库名称。
 4. 输入防火墙规则名称以及运行 Azure Data Studio 的本地客户端计算机的 IP 范围。 这样，你就可以在创建服务器和数据库后直接从 ADS 进行连接。
 5. 选择“**下一页**”。
 6. 查看已输入的参数，然后选择“脚本到笔记本”。

笔记本打开后，你可以查看内容和代码，并根据需要进行更改。 特别是，如果有特定的性能或成本首选项，则可以从默认值更改计算和存储设置。 但请注意，对笔记本进行的任何更改都可能会导致验证错误。

最后一步是选择“全部运行”，以运行笔记本中的所有单元格。 完成此过程后，你应该具有一个完全创建且正在运行的 SQL 数据库，可连接到该数据库并从 ADS 进行查询。

## <a name="next-steps"></a>后续步骤

创建数据库、服务器或池后，可以在 Azure Data Studio 中[连接和查询](quickstart-sql-database.md)数据库。
