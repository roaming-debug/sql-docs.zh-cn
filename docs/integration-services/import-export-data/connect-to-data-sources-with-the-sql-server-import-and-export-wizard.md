---
description: 使用 SQL Server 导入和导出向导连接到数据源
title: 连接到数据源（SQL Server 导入和导出向导）| Microsoft Docs
ms.custom: ''
ms.date: 02/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: fd726506-54b7-433b-bf70-3642235b7b31
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7d8738fb22682ad4a6f432187998663c4c8282cd
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122831"
---
# <a name="connect-to-data-sources-with-the-sql-server-import-and-export-wizard"></a>使用 SQL Server 导入和导出向导连接到数据源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


本部分中的主题说明如何在运行 SQL Server 导入和导出向导时连接到多个常用的数据源。 用户需要在向导的“选择数据源”和“选择目标”页上为数据源提供连接信息。

本部分中的主题仅介绍如何从向导的“选择数据源”和“选择目标”页 **连接到数据源**。 如需其他信息，请参阅[相关任务和内容](#related)。

## <a name="connect-to-a-commonly-used-data-source"></a>连接到常用数据源
单击链接详细了解如何连接到以下常用数据源之一。
-   [SQL Server](../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md)
-   [Oracle](../../integration-services/import-export-data/connect-to-an-oracle-data-source-sql-server-import-and-export-wizard.md)
-   [平面文件（文本文件）](../../integration-services/import-export-data/connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md)
-   [Excel](../../integration-services/import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md)
-   [访问](../../integration-services/import-export-data/connect-to-an-access-data-source-sql-server-import-and-export-wizard.md)
-   [Azure Blob 存储](../../integration-services/import-export-data/connect-to-azure-blob-storage-sql-server-import-and-export-wizard.md)
-   [ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md)
-   [PostgreSQL](../../integration-services/import-export-data/connect-to-a-postgresql-data-source-sql-server-import-and-export-wizard.md)
-   [MySQL](../../integration-services/import-export-data/connect-to-a-mysql-data-source-sql-server-import-and-export-wizard.md)

## <a name="connect-to-other-data-providers"></a>连接到其他数据提供程序
有关如何连接到此处未列出的数据源的信息，请参阅 [The Connection Strings Reference](https://www.connectionstrings.com/)（连接字符串参考）。 该第三方站点包含示例连接字符串、关于数据提供程序的详细信息以及它们需要的连接信息。

## <a name="related-tasks-and-content"></a><a name="related"></a>相关任务和内容  
以下是一些其他的基本任务。
-   **查看有关向导工作原理的简短示例。**

    -   **如果想查看屏幕快照。** 查看此仅占用一页的简单端对端示例 — [开始使用这一简单的导入和导出向导示例](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)。

    -   **如果想观看视频。** 观看这个来自 YouTube 的四分钟视频，该视频演示了此向导，并清晰简明地说明了将数据导出到 Excel 的方法：[使用 SQL Server 导入和导出向导导出到 Excel](https://go.microsoft.com/fwlink/?linkid=829049)。

-   **了解有关向导工作原理的详细信息。**

    -   **了解有关向导的详细信息。** 如果正在寻找有关该向导的概述，请参阅 [使用 SQL Server 导入和导出向导来导入和导出数据](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md)。

    -   **了解有关向导中的步骤的信息。** 如需有关向导中的各个步骤的信息，请参阅 [SQL Server 导入和导出向导中的步骤](../../integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard.md)。 还为向导的每一页专门设有一页文档信息页。

-   **启动向导。** 如果已准备好运行向导，并且只想知道如何启动向导，请参阅[启动 SQL Server 导入和导出向导](../../integration-services/import-export-data/start-the-sql-server-import-and-export-wizard.md)。

-   **获取向导。** 如果想要运行向导，但是尚未在计算机上安装 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，则可以通过安装 SQL Server Data Tools (SSDT) 来安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导。 有关详细信息，请参阅 [下载 SQL Server Data Tools (SSDT)](../../ssdt/download-sql-server-data-tools-ssdt.md)。

## <a name="see-also"></a>另请参阅
[选择数据源](../../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md)  
[选择目标](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md)