---
title: 升级 Data Quality Services | Microsoft Docs
description: 本文提供的信息介绍如何升级现有 SQL Server Data Quality Services (DQS) 安装。
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: f396666b-7754-4efc-9507-0fd114cc32d5
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 1be8d8e6b320af96bc3acf7a28ec2c017a92914a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480318"
---
# <a name="upgrade-data-quality-services"></a>升级 Data Quality Services

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

本文提供的信息介绍如何升级现有 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]Data Quality Services (DQS) 安装。 升级 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Data Quality Server 的过程中，还必须升级 DQS 数据库架构。  
  
> [!IMPORTANT]
>  -   您必须先备份 DQS 数据库，然后才能升级 DQS，以防止在架构升级过程中出现任何意外数据损失。 有关备份 DQS 数据库的信息，请参阅 [Backing Up and Restoring DQS Databases](../../data-quality-services/backing-up-and-restoring-dqs-databases.md)。  
> -   通过使用当前或早期版本的 Data Quality Client 或 Integration Services 中的 [DQS 清除转换](../../integration-services/data-flow/transformations/dqs-cleansing-transformation.md)，可以连接到 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Data Quality Server 执行数据质量任务。  
> -   升级 Data Quality Services 和 Master Data Services 后，用于 Excel 的 Master Data Services 加载项的任何早期版本都将不再适用。 你可以从 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 此处 [下载用于 Excel 的](../../master-data-services/master-data-services-installation-and-configuration.md)版本 Master Data Services 外接程序。  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   您必须作为 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 计算机上 Administrators 组的成员登录。  
  
-   您的 Windows 用户帐户必须是安装了 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 的 SQL Server 实例中 sysadmin 固定服务器角色的成员。  
  
##  <a name="upgrading-dqs"></a><a name="Upgrade"></a> 升级 DQS  
 要升级 DQS：  
  
1.  首先备份 DQS 数据库，然后再启动升级过程。 有关备份 DQS 数据库的信息，请参阅 [Backing Up and Restoring DQS Databases](../../data-quality-services/backing-up-and-restoring-dqs-databases.md)。  
  
2.  升级安装 DQS 的 SQL Server 实例。  
  
    1.  运行 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 安装向导。  
  
    2.  在左窗格中，单击 **“安装”** 。  
  
    3.  在右窗格中，单击“升级版本...”从 SQL Server 早期版本升级。  
  
    4.  完成安装向导。  
  
        > [!NOTE]  
        >  这会将您的 SQL Server 实例升级为 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ，而且如果此计算机以前安装过数据质量客户端，还会安装最新的数据质量客户端。 如果您在其他计算机上安装了数据质量客户端，您必须在那些计算机上运行步骤 2 的这些分步骤，安装最新版的数据质量客户端。 安装向导会安装与现有的数据质量客户端并存的当前版本数据质量客户端。 升级完 DQS 数据架构后，通过使用当前或早期版本的数据质量客户端，您可以连接 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 版数据质量服务器。  
  
3.  升级 DQS 数据库架构。  
  
    1.  作为管理员启动命令提示符。  
  
    2.  在命令提示符下，将目录更改为 DQSInstaller.exe 出现的位置。 对于 SQL Server 的默认实例，可在 C:\Program Files\Microsoft SQL Server\MSSQL[nn].MSSQLSERVER\MSSQL\Binn 获取 DQSInstaller.exe 文件：  

        >[!NOTE]
        >在文件夹路径中，将 [nn] 替换为 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 的版本号。
        >- 对于 SQL Server 2016：13
        >- 对于 SQL Server 2017：14
    
        ```console
        cd C:\Program Files\Microsoft SQL Server\MSSQL[nn].MSSQLSERVER\MSSQL\Binn  
        ```  
  
    3.  在命令提示符下，键入以下命令，再按 Enter：  
  
        ```console
        dqsinstaller.exe -upgrade  
        ```  
  
    4.  安装程序会提示您在继续操作之前备份 DQS 数据库。 如果已经备份 DQS 数据库，请键入 **Y** 或 **Yes**，然后按 Enter 以继续升级。  
  
    5.  在成功升级 DQS 数据库架构之后，将显示一条完成消息。  
  
##  <a name="verifying-the-dqs-databases-schema-upgrade"></a><a name="Verify"></a> 验证 DQS 数据库架构升级  
 要验证是否成功升级了 DQS 数据库架构，您可以查询每个数据库中的 A_DB_VERSION 表来检查 DQS_MAIN 和 DQS_PROJECTS 数据库中的当前版本。 为此，请执行以下操作：  
  
1.  启动 SQL Server Management Studio 并连接到包含升级的 DQS 数据库架构的 SQL Server 实例。  
  
2.  运行以下查询：  
  
    ```sql
    SELECT * FROM DQS_MAIN.dbo.A_DB_VERSION WHERE STATUS=2;  
    SELECT * FROM DQS_PROJECTS.dbo.A_DB_VERSION WHERE STATUS=2;  
    ```  
  
3.  输出将为每个升级显示一条信息，并显示升级的日期。 最新日期的最大 VERSION_ID 和 ASSEMBLY_VERSION 是当前版本。 STATUS 列中的值为 2 时指示成功。 如果发生错误，错误将在 ERROR 列中列出。 示例输出：  
  
    |ID|UPGRADE_DATE|VERSION_ID|ASSEMBLY_VERSION|USER_NAME|状态|ERROR|  
    |--------|-------------------|-----------------|-----------------------|----------------|------------|-----------|  
    |1000|2013-08-11 05:26:39.567|1200|11.0.3000.0|\<DOMAIN\UserName>|2||  
    |1001|2013-09-19 15:09:37.750|1600|12.0.xxxx.0|\<DOMAIN\UserName>|2||  
  
## <a name="see-also"></a>另请参阅  
 [安装 Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [删除 Data Quality Server 对象](../../sql-server/install/remove-data-quality-server-objects.md)   
 [升级 SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
  
