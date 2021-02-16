---
title: 验证部署到 Azure 的 SSIS 包| Microsoft Docs
description: 了解 SSIS 包部署向导如何检查包是否存在可能阻止包在 Azure 中按预期方式运行的已知问题。
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 3da89cbe8d0028a2b362e6358d1b30b3075e030c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345681"
---
# <a name="validate-sql-server-integration-services-ssis-packages-deployed-to-azure"></a>验证部署到 Azure 的 SQL Server Integration Services (SSIS) 包

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



将 SQL Server Integration Services (SSIS) 项目部署到 Azure 服务器上的 SSIS 目录 (SSISDB) 时，包部署向导将在“评审”页面后添加一个额外的验证步骤  。 检查项目中的包是否存在可能阻止包在 Azure SSIS Integration Runtime 中按预期运行的已知问题。 然后向导在“验证”页面上显示所有适用的警告  。

> [!IMPORTANT]
> 使用 SQL Server Data Tools (SSDT) 17.4 版或更高版本部署项目时，将出现本文所述的验证。 要获取最新版 SSDT，请参阅[下载 SQL Server Data Tools (SSDT)](../../ssdt/download-sql-server-data-tools-ssdt.md)。

有关包部署向导的详细信息，请参阅[部署 Integration Services (SSIS) 项目和包](../packages/deploy-integration-services-ssis-projects-and-packages.md)。

## <a name="validate-connection-managers"></a>验证连接管理器

向导检查某些连接管理器是否存在以下问题，这些问题可能会导致连接失败：
- **Windows 身份验证**。 如果连接字符串使用 Windows 身份验证，验证将引发警告。 Windows 身份验证需要其他配置步骤。 有关详细信息，请参阅[使用 Windows 身份验证连接到数据和文件共享](/azure/data-factory/ssis-azure-connect-with-windows-auth)。
- **文件路径**。 如果连接字符串包含硬编码的本地文件路径（例如 `C:\\...`），验证将引发警告。 包含绝对路径的包可能会失败。
- **UNC 路径**。 如果连接字符串包含 UNC 路径，验证将引发警告。 包含 UNC 路径的包可能会失败，这通常是因为 UNC 路径需要 Windows 身份验证才能访问。
- **主机名**。 如果服务器属性包含主机名而不是 IP 地址，验证将引发警告。 包含主机名的包可能会失败，这通常是因为 Azure 虚拟网络需要正确的 DNS 配置才能支持 DNS 名称解析。
- **提供程序或驱动程序**。 如果不支持提供程序或驱动程序，验证将引发警告。 目前仅支持少量内置提供程序或驱动程序。

向导对列表中的连接管理器执行以下验证检查。

| 连接管理器 | Windows 身份验证 | 文件路径 | UNC 路径 | 主机名 | 提供程序或驱动程序 |
|--------------------|----------|-----------|-----|-----------|-------------------|
| Ado                | âœ"        |           |     | âœ"         | âœ"                 |
| AdoNet             | âœ"        |           |     | âœ"         | âœ"                 |
| 缓存              |          | âœ"         | âœ"   |           |                   |
| Excel              |          | âœ"         | âœ"   |           |                   |
| 文件               |          | âœ"         | âœ"   |           |                   |
| FlatFile           |          | âœ"         | âœ"   |           |                   |
| Ftp                |          |           |     | âœ"         |                   |
| MsOLAP100          |          |           |     | âœ"         | âœ"                 |
| MultiFile          |          | âœ"         | âœ"   |           |                   |
| MultiFlatFile      |          | âœ"         | âœ"   |           |                   |
| OData              | âœ"        |           |     | âœ"         |                   |
| Odbc               | âœ"        |           |     | âœ"         | âœ"                 |
| OleDb              | âœ"        |           |     | âœ"         | âœ"                 |
| SmoServer          | âœ"        |           |     | âœ"         |                   |
| Smtp               | âœ"        |           |     | âœ"         |                   |
| SqlMobile          |          | âœ"         | âœ"   |           |                   |
| Wmi                | âœ"        |           |     |           |                   |
|||||||

## <a name="validate-sources-and-destinations"></a>验证源和目标
不支持以下第三方源和目标：

-   Attunity 提供的 Oracle 源和目标
-   Attunity 提供的 Teradata 源和目标
-   SAP BW 源和目标

## <a name="validate-tasks-and-components"></a>验证任务和组件

### <a name="execute-process-task"></a>执行进程任务

如果命令指向具有绝对路径的本地文件或具有 UNC 路径的文件，验证将引发警告。 这些路径可能导致在 Azure 上执行操作失败。

### <a name="script-task-and-script-component"></a>脚本任务和脚本组件

如果某个包所含的脚本任务或脚本组件可能会引用或调用不受支持的程序集，则验证将引发警告。 这些引用或调用可能会导致执行失败。

### <a name="other-components"></a>其他组件

HDFS 目标和 Azure Data Lake Store 目标不支持 Orc 格式。

## <a name="next-steps"></a>后续步骤
若要了解如何计划 Azure 上的包执行，请参阅[计划 Azure 上的 SSIS 包执行](ssis-azure-schedule-packages.md)。