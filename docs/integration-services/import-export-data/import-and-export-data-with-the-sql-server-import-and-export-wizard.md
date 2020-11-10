---
description: 使用 SQL Server 导入和导出向导导入和导出数据
title: 使用 SQL Server 导入和导出向导导入和导出数据 | Microsoft Docs
ms.custom: ''
ms.date: 10/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- exporting data
- mapping files [Integration Services]
- SQL Server Import and Export Wizard
- SSIS packages, creating
- packages [Integration Services], copying
- Integration Services packages, creating
- packages [Integration Services], creating
- SQL Server Integration Services packages, creating
- Import and Export Wizard
- copying data [Integration Services]
- importing data, SSIS packages
- sources [Integration Services], copying data
ms.assetid: c0e4d867-b2a9-4b2a-844b-2fe45be88f81
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e8146e56063f73ddc054595c588e396ff4fcb8c0
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384716"
---
# <a name="import-and-export-data-with-the-sql-server-import-and-export-wizard"></a>使用 SQL Server 导入和导出向导导入和导出数据

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



 利用[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导，可轻松将数据从源复制到目标。 本概述介绍向导可用作源和目标的数据源，以及运行向导所需的权限。

## <a name="get-the-wizard"></a>获取向导
如果想要运行向导，但是尚未在计算机上安装 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，则可以通过安装 SQL Server Data Tools (SSDT) 来安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导。 有关详细信息，请参阅 [下载 SQL Server Data Tools (SSDT)](../../ssdt/download-sql-server-data-tools-ssdt.md)。

## <a name="what-happens-when-i-run-the-wizard"></a>运行向导时会发生什么情况？
-    **查看步骤列表。** 有关向导中步骤的介绍，请参阅 [SQL Server 导入和导出向导中的步骤](../../integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard.md)。 还为向导的每一页专门设有一页文档信息页。  
    \- 或 \-
-   **查看示例。** 若要快速浏览典型会话中常见的多个屏幕，请查看此仅占用一页的简单示例 — [开始使用这一简单的导入和导出向导示例](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)。  

##  <a name="what-sources-and-destinations-can-i-use"></a><a name="wizardSources"></a>我可以使用哪些源和目标？  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导可以将数据复制到下表中列出的数据源以及从中复制数据。 若要连接其中某些数据源，可能需要下载并安装其他文件。
 
| 数据源 | 是否必须下载其他文件？ |
|-------------|-----------------------------------------|
|**企业数据库**<br/>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、Oracle、DB2 等。|SQL Server 或 SQL Server Data Tools (SSDT) 安装连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时所需的文件。 但 SSDT 不安装连接到其他企业数据库（如 Oracle 或 IBM DB2）时所需的所有文件。<br/><br/>若要连接企业数据库，通常需要具有以下两项内容：<br/><br/>1. **客户端软件** 。 如果已经为企业数据库系统安装了客户端软件，则通常会有建立连接所需的文件。 如果尚未安装客户端软件，请询问数据库管理员如何安装获得许可的副本。<br/><br/>2. **驱动程序或提供程序** 。 Microsoft 安装用于连接 Oracle 的驱动程序和提供程序。 若要连接到 IBM DB2，请从 [Microsoft SQL Server 2016 功能包](https://www.microsoft.com/download/details.aspx?id=56833)中获取用于 Microsoft SQL Server 的 DB2 v5.0 的 MicrosoftÂ® OLEDB 提供程序。<br/><br/>有关详细信息，请参阅[连接到 SQL Server 数据源](connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md)或[连接到 Oracle 数据源](connect-to-an-oracle-data-source-sql-server-import-and-export-wizard.md)。|
|**文本文件** （平面文件）|无需任何其他文件。<br/><br/>有关详细信息，请参阅[连接到平面文件数据源](connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md)。|
|**Microsoft Excel 和 Microsoft Access 文件**|Microsoft Office 并不会安装连接到作为数据源的 Excel 和 Access 文件所需的所有文件。 获取下载：[Microsoft Access 数据库引擎 2016 可再发行组件](https://www.microsoft.com/download/details.aspx?id=54920)。<br/><br/>有关详细信息，请参阅[连接到 Excel 数据源](../../integration-services/import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md)或[连接到 Access 数据源](../../integration-services/import-export-data/connect-to-an-access-data-source-sql-server-import-and-export-wizard.md)。|
|**Azure 数据源**<br/>目前仅限 Azure Blob 存储。|SQL Server Data Tools 不会安装需要作为数据源连接到 Azure Blob 存储的文件。 获取以下下载 — [用于 Azure 的 Microsoft SQL Server 2016 Integration Services 功能包](https://www.microsoft.com/download/details.aspx?id=49492)。<br/><br/>有关详细信息，请参阅[连接到 Azure Blob 存储](../../integration-services/import-export-data/connect-to-azure-blob-storage-sql-server-import-and-export-wizard.md)。|
|**打开源数据库**<br/>PostgreSQL、MySql 等。|必须下载其他文件才能连接到这些数据源。<br/><br/>- 对于 PostgreSQL，请参阅[连接到 PostgreSQL 数据源](../../integration-services/import-export-data/connect-to-a-postgresql-data-source-sql-server-import-and-export-wizard.md)。<br/>- 对于 MySql，请参阅[连接到 MySQL 数据源](../../integration-services/import-export-data/connect-to-a-mysql-data-source-sql-server-import-and-export-wizard.md)。|
|**提供驱动程序或提供程序的其他任何数据源**|通常必须下载其他文件才能连接到以下类型的数据源。<br/><br/>- 提供 **ODBC 驱动程序** 的任何源。 有关详细信息，请参阅[连接到 ODBC 数据源](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md)。<br/>- 提供 **.Net Framework 数据提供程序** 的任何源。<br/>- 提供 **OLE DB 提供程序** 的任何源。<br/><br/>有时，为其他数据源提供源和目标功能的第三方组件被标记为 SQL Server Integration Services (SSIS) 的附加产品。|

## <a name="how-do-i-connect-to-my-data"></a>如何连接到我的数据？
有关如何连接到常用数据源的信息，请参阅以下页面之一：
-   [连接到 SQL Server](../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md)
-   [连接到 Oracle](../../integration-services/import-export-data/connect-to-an-oracle-data-source-sql-server-import-and-export-wizard.md)
-   [连接到平面文件（文本文件）](../../integration-services/import-export-data/connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md)
-   [连接到 Excel](../../integration-services/import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md)
-   [连接到 Access](../../integration-services/import-export-data/connect-to-an-access-data-source-sql-server-import-and-export-wizard.md)
-   [连接到 Azure Blob 存储](../../integration-services/import-export-data/connect-to-azure-blob-storage-sql-server-import-and-export-wizard.md)
-   [使用 ODBC 连接](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md)
-   [连接到 PostgreSQL](../../integration-services/import-export-data/connect-to-a-postgresql-data-source-sql-server-import-and-export-wizard.md)
-   [连接到 MySQL](../../integration-services/import-export-data/connect-to-a-mysql-data-source-sql-server-import-and-export-wizard.md)


有关如何连接到此处未列出的数据源的信息，请参阅 [The Connection Strings Reference](https://www.connectionstrings.com/)（连接字符串参考）。 该第三方站点包含示例连接字符串、关于数据提供程序的详细信息以及它们需要的连接信息。

## <a name="what-permissions-do-i-need"></a>我需要哪些权限？  
 若要成功运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导，必须至少具有下列权限。 如果已使用数据源和目标，则可能已具有所需的权限。
  
|执行这些操作时需要权限|如果要连接到 SQL Server，则需要这些特定权限 |  
|-------------------------|----------------------------------------------------|  
|连接到源数据库和目标数据库或文件共享。|服务器和数据库的登录权限。|  
|从源数据库或文件中导出或读取数据。|对源表和源视图具有 SELECT 权限。|  
|向目标数据库或文件导入或写入数据。|对目标表具有 INSERT 权限。|  
|创建目标数据库或文件（如果适用）。|CREATE DATABASE 或 CREATE TABLE 权限。|  
|保存向导创建的 SSIS 包（如果适用）。|如果要将包保存到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，需要有将包保存到 **msdb** 数据库的权限。|  
  
## <a name="get-help-while-the-wizard-is-running"></a>在向导运行期间获得帮助
> [!TIP]
> 从向导的任何页面或对话框中点击 F1 键，可查看当前页的相关文档。   
  
##  <a name="the-wizard-uses-sql-server-integration-services-ssis"></a><a name="wizardSSIS"></a> 向导使用 SQL Server Integration Services (SSIS)  
 向导使用 SQL Server Integration Services (SSIS) 复制数据。 SSIS 是一种用于提取、转换和加载数据 (ETL) 的工具。 向导页面会使用某些 SSIS 语言。
  
 在 SSIS 中，基本单位是 **包** 。 当你在向导的各个页面之间移动并指定选项时，向导会在内存中创建 SSIS 包。    
  
如果安装了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标准版或更高版本，那么在向导结束时，可以选择保存 SSIS 包。 之后可以重新使用包，或者通过使用 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器添加任务、转换和事件驱动逻辑来扩展包。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导为创建从源向目标复制数据的基本 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包提供了最简便的方法。

有关 SSIS 的详细信息，请参阅 [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md)。

## <a name="whats-next"></a>下一步操作  
 启动向导。 有关详细信息，请参阅 [启动 SQL Server 导入和导出向导](../../integration-services/import-export-data/start-the-sql-server-import-and-export-wizard.md)。  

## <a name="see-also"></a>另请参阅
[导入和导出向导的简单示例入门](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)  
[SQL Server 导入和导出向导中的数据类型映射](../../integration-services/import-export-data/data-type-mapping-in-the-sql-server-import-and-export-wizard.md)