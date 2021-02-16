---
title: 连接到 Azure 中的 SSIS 目录 (SSISDB) | Microsoft Docs
description: 查找连接到 Azure SQL 数据库服务器上托管的 SSIS 目录 (SSISDB) 所需的连接信息。
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: e5ce8d83ba29843cc14c6798eb4f550eab62e557
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352010"
---
# <a name="connect-to-the-ssis-catalog-ssisdb-in-azure"></a>连接到 Azure 中的 SSIS 目录 (SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



查找连接到 Azure SQL 数据库服务器上托管的 SSIS 目录 (SSISDB) 所需的连接信息。 连接时需要以下项：
- 完全限定服务器名称
- 数据库名称
- 登录信息 

> [!IMPORTANT]
> 如果尚未在 Azure 数据工厂中创建 Azure-SSIS Integration Runtime，则无法在 Azure SQL 数据库中创建 SSISDB 目录数据库。 Azure-SSIS IR 是在 Azure 上运行 SSIS 包的运行时环境。 有关过程的演练，请参阅[在 Azure 中部署和运行 SSIS 包](/azure/data-factory/tutorial-create-azure-ssis-runtime-portal)。 

## <a name="prerequisites"></a>先决条件
开始之前，请确保具有 SQL Server Management Studio (SSMS) 版本 17.2 或更高版本。 如果 SSISDB 目录数据库托管在 SQL 托管实例中，请确保安装了 17.6 版或更高版本的 SSMS。 若要下载 SSMS 最新版本，请参阅[下载 SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)。

## <a name="get-the-connection-info-from-the-azure-portal"></a>从 Azure 门户获取连接信息
1. 登录到 [Azure 门户](https://portal.azure.com/)。
2. 从 Azure 门户的左侧菜单中选择“SQL 数据库”  ，然后在“SQL 数据库”  页上选择 `SSISDB` 数据库。 
3. 在 `SSISDB` 数据库的“概述”  页上，查看完全限定服务器名称，如下图所示。 将鼠标悬停在服务器名称上，以打开“单击进行复制”  选项。

    ![服务器连接信息](media/ssis-azure-connect-to-catalog-database/server-name.png) 

4. 如果忘记了 SQL 数据库服务器的登录信息，则导航到 SQL 数据库服务器页。 可在其中查看服务器管理员名称，如有必要，还可重置密码。

## <a name="connect-with-ssms"></a>与 SSMS 连接
1. 打开 SQL Server Management Studio。

2. **连接到该服务器**。 在“连接到服务器”对话框中，输入以下信息： 

   | 设置       | 建议的值 | 说明 | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **服务器类型** | 数据库引擎 | 此值是必需的。 |
   | **服务器名称** | 完全限定的服务器名称 | 名称应采用此格式：**mysqldbserver.database.windows.net**。 |
   | **身份验证** | SQL Server 身份验证 | |
   | **登录** | 服务器管理员帐户 | 这是在创建服务器时指定的帐户。 |
   | **密码** | 服务器管理员帐户的密码 | 这是在创建服务器时指定的密码。 |

    ![使用 SSMS 连接到服务器](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-1.png)

3. **连接到 SSISDB 数据库**。 选择“选项”  展开“连接到服务器”  对话框。 在展开的“连接到服务器”  对话框中，选择“连接属性”  选项卡。在“连接到数据库”  字段中，选择或输入 `SSISDB`。

    > [!IMPORTANT]
    > 如果连接时未选择 `SSISDB`，对象资源管理器中可能不会显示 SSIS 目录。

    ![选择 SSISDB 数据库以供连接](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-2.png)

4. 然后选择“连接”  。

5. 在对象资源管理器中，展开“Integration Services 目录”，然后展开“SSISDB”，查看 SSIS 目录数据库中的对象   。

    ![在 SSMS 对象资源管理器中查找 SSISDB 数据库](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-3.png)

## <a name="next-steps"></a>后续步骤
- 部署包。 有关详细信息，请参阅[使用 SQL Server Management Studio (SSMS) 部署 SSIS 项目](../ssis-quickstart-deploy-ssms.md)。
- 运行包。 有关详细信息，请参阅[使用 SQL Server Management Studio (SSMS) 运行 SSIS 包](../ssis-quickstart-run-ssms.md)。
- 计划包执行。 有关详细信息，请参阅[计划 Azure 中的 SSIS 包](ssis-azure-schedule-packages.md)