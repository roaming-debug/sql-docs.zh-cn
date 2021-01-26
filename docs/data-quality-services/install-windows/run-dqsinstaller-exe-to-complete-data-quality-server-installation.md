---
title: 运行 DQSInstaller.exe 以完成数据质量服务器安装
description: 了解如何运行 DQSInstaller.exe 来完成 (DQS) 的 SQL Server Data Quality Services 安装。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: 7a8c96e0-1328-4f35-97fc-b6d9cb808bae
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 2933803ce4a8bce84468f4ce2e360a18d2444635
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765799"
---
# <a name="run-dqsinstallerexe-to-complete-data-quality-server-installation"></a>运行 DQSInstaller.exe 以便完成数据质量服务器安装

[!INCLUDE [SQL Server - Windows only ](../../includes/applies-to-version/sql-windows-only.md)]

  若要完成 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 安装，您必须安装完 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]后运行 DQSInstaller.exe 文件。 本主题介绍如何从 **“开始”** 屏幕、 **“开始”** 菜单、Windows 资源管理器或命令提示符运行 DQSInstaller.exe，您可以选择上述任何方法来运行 DQSInstaller.exe 文件。  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> 先决条件  
  
-   安装 SQL Server 时，必须在 SQL Server 安装程序的“功能选择”页上的 **“数据库引擎服务”** 下选择了 **“Data Quality Services”** ，并且必须已完成 SQL Server 安装。 有关详细信息，请参阅 [Install Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)。  
  
-   您的 Windows 用户帐户必须是 SQL Server 数据库引擎实例中 sysadmin 固定服务器角色的成员。  
  
-   您必须以您在运行 DQSInstaller.exe 的计算机上 Administrators 组成员的身份登录。  
  
##  <a name="run-dqsinstallerexe-from-start-screen-start-menu-or-windows-explorer"></a><a name="WindowsExplorer"></a> 从 "开始" 屏幕、"开始" 菜单或 Windows 资源管理器运行 DQSInstaller.exe  
  
1.  在您选择安装 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]的计算机上，通过以下任意可行方法运行 DQSInstaller.exe 文件：  
  
    -   **“开始”屏幕**：在 **“开始”** 屏幕上，单击 **“数据质量服务器安装程序”**  
  
    -   “开始菜单”：在任务栏上，单击“开始” ，指向“所有程序” ，然后单击 [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]。 在 [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]下，单击 **Data Quality Services**，然后单击“数据质量服务器安装程序”   
  
    -   **Windows 资源管理器**：查找 DQSInstaller.exe 文件。 如果你已安装了 SQL Server 的默认实例，则 DQSInstaller.exe 文件将出现在 C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn 下。 双击 DQSInstaller.exe 文件。  
  
2.  显示安装状态的提示符窗口将出现。 您将注意到以下三种情况：  
  
    1.  安装程序将创建一个安装日志文件 DQS_install.log，该文件包含与对 DQSInstaller.exe 文件执行的操作有关的信息。 如果你已安装了 SQL Server 的默认实例，则 DQS_install.log 文件将出现在 C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Log 下。  
  
    2.  该安装程序使用默认服务器排序规则 SQL_Latin1_General_CP1_CI_AS 来安装 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]。  
  
        > [!IMPORTANT]  
        >  只有在从命令提示符运行 DQSInstaller.exe 时，您才能提供用于安装 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 的不同服务器排序规则值。 有关详细信息，请参阅本主题后面的 [从命令提示符运行 DQSInstaller.exe](#CommandPrompt) 。  
  
    3.  该安装程序会检查因最近已经安装在计算机上的任何更新而导致计算机重新启动的任何挂起现象。 如果发现任何挂起的重新启动现象，则会显示一条消息，通知您这样的现象存在，您可以通过分别按 Y 或 N 选择继续或中止安装。 我们建议如果存在任何挂起的重新启动现象，您必须中止安装，重新启动计算机，然后再次运行 DQSInstaller.exe。  
  
3.  系统将提示您为数据库主密钥键入密码。 该数据库密钥是加密引用数据服务提供程序密钥所必需的，当您以后在 [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] (DQS) 中设置引用数据提供程序时，这些密钥将存储于 DQS_MAIN 数据库中。  
  
    > [!IMPORTANT]  
    >  密码长度必须至少为8个字符，并且必须包含以下四个类别中的三个：英文大写字母 (A，B，C,.。。Z) 、英文小写字母 (a、b、c,.。。z) ，数字 (0，1，2,.。。9) ，以及非字母数字或特殊字符 (~！ @ # $% ^& * ( # A8_-+ = | \\ {}[]:;"'<>,。?/) 。 例如：P@ssword。 如果当前密码与这些要求不匹配，安装程序将提示您输入其他密码。  
  
4.  提供一个密码，确认该密码，然后按下 ENTER 键以便继续安装。  
  
    > [!IMPORTANT]  
    >  必须保留为数据库主密钥指定的密码，因为在将来如果选择从备份还原 DQS 数据库时需要它。 有关还原 DQS 数据库的详细信息，请参阅 [Backing Up and Restoring DQS Databases](../../data-quality-services/backing-up-and-restoring-dqs-databases.md)。  
  
5.  如果 Master Data Services 数据库与 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]处于相同的 SQL Server 实例中，则该安装程序会创建一个用户映射到 Master Data Services 登录名，并向该用户授予对 DQS_MAIN 数据库的 dqs_administrator 角色。 有关安装 Master Data Services 和创建 Master Data Services 数据库的信息，请参阅 [Install Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)。  
  
6.  在安装成功完成后，将显示一条完成消息。 按任意键关闭命令提示符窗口。  
  
##  <a name="run-dqsinstallerexe-from-command-prompt"></a><a name="CommandPrompt"></a> 从命令提示符运行 DQSInstaller.exe  
 您可以从命令提示符处使用以下命令行参数运行 DQSInstaller.exe：  
  
|DQSInstaller.exe 参数|说明|示例语法|  
|--------------------------------|-----------------|-------------------|  
|-collation|安装 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]时要使用的服务器排序规则。<br /><br /> DQS 仅支持不区分大小写的排序规则。 如果您指定了区分大小写的排序规则，则该安装程序将尝试使用指定排序规则的不区分大小写版本。 如果没有不区分大小写的排序规则，或者 SQL 不支持排序规则，则 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 安装将失败。<br /><br /> 如果未指定服务器排序规则，则使用默认排序规则 SQL_Latin1_General_CP1_CI_AS。|`dqsinstaller.exe -collation <collation_name>`|  
|-upgradedlls|跳过重新创建 DQS 数据库（DQS_MAIN、DQS_PROJECTS 和 DQS_STAGING_DATA），并且仅更新 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 数据库中的 DQS 所使用的 SQL 公共语言运行时 (SQLCLR) 程序集。<br /><br /> 有关详细信息，请参阅 [.NET Framework 更新后升级 SQLCLR 程序集](../../data-quality-services/install-windows/upgrade-sqlclr-assemblies-after-net-framework-update.md)|`dqsinstaller.exe -upgradedlls`|  
|-exportkbs|将所有知识库导出到 DQS 备份文件 (.dqsb)。 您还必须指定要导出所有知识库的完整路径和文件名。<br /><br /> 有关详细信息，请参阅 [使用 DQSInstaller.exe 导出和导入 DQS 知识库](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md)|`dqsinstaller.exe -exportkbs <path><filename>`<br /><br /> 例如： `dqsinstaller.exe -exportkbs c:\DQSBackup.dqsb`|  
|-importkbs|完成 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 安装后，从 DQS 备份文件 (.dqsb) 导入所有知识库。 您还必须指定要从中导入所有知识库的完整路径和文件名。<br /><br /> 有关详细信息，请参阅 [使用 DQSInstaller.exe 导出和导入 DQS 知识库](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md)|`dqsinstaller.exe -importkbs <path><filename>`<br /><br /> 例如： `dqsinstaller.exe -importkbs c:\DQSBackup.dqsb`|  
|-upgrade|升级 DQS 数据库架构。 在以前配置的 DQS 实例上安装 SQL Server 更新程序后，必须使用此参数。 有关详细信息，请参阅 [Upgrade DQS Databases Schema After Installing SQL Server Update](../../data-quality-services/install-windows/upgrade-dqs-databases-schema-after-installing-sql-server-update.md)。|`dqsinstaller.exe -upgrade`|  
|-uninstall|从当前 SQL Server 实例中卸载 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 。<br /><br /> 还可以将现有数据质量服务器安装中的所有知识库导出到 DQS 备份文件 (.dqsb)，然后卸载数据质量服务器。 有关详细信息，请参阅 [使用 DQSInstaller.exe 导出和导入 DQS 知识库](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md)<br /><br /> **\* \* 重要 \* 提示 \*** 如果 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 使用命令行参数从 SQL server 实例中卸载，则在 `-uninstall` 卸载过程中将删除所有 DQS 对象。 卸载 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 后，无需按照 [删除数据质量服务器对象](../../sql-server/install/remove-data-quality-server-objects.md)中所述进行手动删除。|**仅卸载数据质量服务器：**<br /><br /> `dqsinstaller.exe -uninstall`<br /><br /> **要将所有知识库导出到文件，然后卸载数据质量服务器：**<br /><br /> `dqsinstaller.exe -uninstall <path><filename>`<br /><br /> 例如： `dqsinstaller.exe -uninstall c:\DQSBackup.dqsb`|  
  
 **从命令提示符运行 DQSInstaller.exe：**  
  
1.  启动命令提示符。  
  
2.  在命令提示符下，将目录更改为 DQSInstaller.exe 出现的位置。 如果你已安装了 SQL Server 的默认实例，DQSInstaller.exe 文件将出现在 C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn 下：  
  
    ```  
    cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn  
    ```  
  
3.  在命令提示符处，在使用或不使用命令行参数的情况下运行 DQSInstaller.exe：  
  
    -   **不使用命令行参数**：键入 `dqsinstaller.exe`，然后按 Enter。  
  
    -   **使用命令行参数**：键入上表中提到的必需命令，然后按 Enter。  
  
4.  基于指定的命令执行所需的操作。 如果你刚刚选择的是在不使用任何命令行参数的情况下安装 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] ，则其余步骤如上一节的步骤 2-6 中所述， [从开始屏幕、开始菜单或 Windows 资源管理器运行 DQSInstaller.exe](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md#WindowsExplorer)。  
  
## <a name="next-steps"></a>后续步骤  
  
-   基于工作配置文件向用户授予适当的 DQS 角色。 请参阅 [将 DQS 角色授予用户](../../data-quality-services/install-windows/grant-dqs-roles-to-users.md)。  
  
-   如果将从 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 远程访问 [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]，则使用此计算机上的 SQL Server 配置管理器来启用 TCP/IP 协议。  
  
-   确保您可以访问用于 DQS 操作的源数据，并且可以将已处理的数据导出到数据库中的某个表。 请参阅 [访问 DQS 操作数据](../../data-quality-services/install-windows/access-data-for-the-dqs-operations.md)。  
  
## <a name="see-also"></a>另请参阅  
 [安装 Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [.NET Framework 更新后升级 SQLCLR 程序集](../../data-quality-services/install-windows/upgrade-sqlclr-assemblies-after-net-framework-update.md)   
 [Export and Import DQS Knowledge Bases Using DQSInstaller.exe](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md)  
  
  
