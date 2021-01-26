---
title: SQL Server 连接器错误和信息日志记录
description: 本文介绍如何通过修改注册表项来启用 SQL Server 连接器错误和日志记录
ms.date: 10/08/2020
ms.localizationpriority: medium
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: how-to
author: rupp29
ms.author: arupp
ms.openlocfilehash: 8d3f89803711e96cb1e37adc9792c54bceb259c4
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596500"
---
# <a name="sql-server-connector-error-and-information-logging"></a>SQL Server 连接器错误和信息日志记录

本文介绍如何修改注册表项以启用 SQL Server 连接器错误和信息日志记录。

## <a name="sql-server-connector-for-microsoft-azure-key-vault"></a>适用于 Microsoft Azure 密钥保管库的 SQL Server 连接器

借助[用于 Microsoft Azure Key Vault 的 SQL Server 连接器](https://www.microsoft.com/download/details.aspx?id=45344)，SQL Server 加密可以使用 Microsoft Azure Key Vault 作为可扩展密钥管理 (EKM) 提供程序来保护其加密密钥。

[下载项](https://www.microsoft.com/download/details.aspx?id=45344)包括 SQL Server 连接器，以及让 SQL Server 管理员可以了解如何配置 SQL Server 连接器和启用 SQL Server 加密方案的示例脚本。 有关详细信息，请参阅[使用 Key Vault 的可扩展密钥管理 (SQL Server)](./extensible-key-management-using-azure-key-vault-sql-server.md)。

在 [Azure Key Vault 论坛](https://social.msdn.microsoft.com/Forums/AzureKeyVault)上提出问题、分享见解并讨论 SQL Server 连接器。

> [!NOTE]
> 在正常执行过程中，SQL Server 连接器 DLL 会动态创建注册表项来与 Azure Key Vault 建立连接，以创建密钥 `[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\SQL Server Cryptographic Provider]`。 SQL Server 启动帐户必须是本地管理员，或其服务帐户必须是 NT SERVICE\MSSQLSERVER

## <a name="upgrade-sql-server-connector-to-the-latest-version"></a>将 SQL Server 连接器升级到最新版本

若要将 SQL Server 连接器（版本：1.0.5.0，日期为 2020 年 9 月）升级到最新版本的 DLL 加密提供程序，请执行以下步骤。

### <a name="upgrade"></a>升级

1. 使用 SQL Server 配置管理器停止 SQL Server 服务
1. 使用“控制面板”\“程序”\“程序和功能”卸载旧版本
    1. 应用程序名称：用于 Microsoft Azure Key Vault 的 SQL Server 连接器
    1. 版本：15.0.300.96
    1. DLL 文件日期：2018/01/30 下午 3:00
1. 安装（升级）用于 Microsoft Azure Key Vault 的 SQL Server 连接器的新版本
    1. 版本：15.0.2000.367
    1. DLL 文件日期：2020/09/11 上午 5:17
1. 启动 SQL Server 服务
1. 测试能否访问加密数据库

### <a name="rollback"></a>回退

1. 使用 SQL Server 配置管理器停止 SQL Server 服务
1. 使用“控制面板”\“程序”\“程序和功能”卸载新版本
    1. 应用程序名称：用于 Microsoft Azure Key Vault 的 SQL Server 连接器
    1. 版本：15.0.2000.367
    1. DLL 文件日期：2020/09/11 上午 5:17
1. 安装用于 Microsoft Azure Key Vault 的 SQL Server 连接器的旧版本
    1. 版本：15.0.300.96
    1. DLL 文件日期：2018/01/30 下午 3:00
1. 启动 SQL Server 服务
1. 测试能否访问加密数据库

> [!NOTE]
> - 已替换 SQL Server 连接器版本 1.0.0.440 和更早的版本，且生产环境不再支持这些版本。 有关排查 SQL Server 连接器问题的详细信息，请参阅 [SQL Server 连接器维护与故障排除](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)。
> - 从版本 1.0.3.0 开始，SQL Server 连接器向 Windows 事件日志报告相关错误消息，以便进行故障排除。
> - 从 [1.0.4.0（版本 13.0.811.168）](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/SQLServerConnectorforMicrosoftAzureKeyVault.msi)开始，支持私有 Azure 云，包括 Azure 中国、Azure 德国和 Azure 政府。
> - 1\.0.5.0 版在指纹算法方面进行了一项重大更改。 升级到 1.0.5.0 后，可能会遭遇数据库还原失败。 有关详细信息，请参阅[知识库文章 447099](https://support.microsoft.com/help/4470999/db-backup-problems-to-sql-server-connector-for-azure-1-0-5-0)。
> - 从版本 1.0.5.0（文件日期为 2020 年 9 月）开始，SQL Server 连接器支持筛选消息和网络请求重试逻辑。
> - SQL Server 连接器的旧版本是指：[1.0.5.0（版本 15.0.300.96）- 文件日期为 2018 年 1 月](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/ENU/SQLServerConnectorforMicrosoftAzureKeyVault.msi)。 如果遇到任何问题，请升级到最新的 SQL Server 连接器。

系统要求 - 支持的 SQL Server 版本：

- SQL Server 2019 RTM Enterprise 64 位
- SQL Server 2017 RTM Enterprise 64 位
- SQL Server 2016 RTM Enterprise 64 位
- SQL Server 2014 RTM Enterprise 64 位
- SQL Server 2012 SP2 Enterprise 64 位
- SQL Server 2012 SP1 CU6 Enterprise 64 位
- SQL Server 2008 R2 SP2 CU8 Enterprise 64 位

在低于上述版本的 SQL Server 2008 和 2012 版本上，需要安装以下知识库文章中指定的补丁：[https://support2.microsoft.com/kb/2859713](https://support2.microsoft.com/kb/2859713)。

用于 Microsoft Azure Key Vault 的 SQL Server 连接器还需要在 Azure 上的 Microsoft SQL Server 虚拟机中安装 .NET 版本 4.5.1。 安装 SQL Server 连接器之前应安装此库。

已安装适当版本的 Visual Studio C++ 可再发行程序包，该程序包版本基于当前运行的 SQL Server 版本：

- 对于 SQL Server 版本 2008、2008 R2、2012 和 2014，请安装 2013 Visual C++ 可再发行程序包。

- 对于 SQL Server 2016，请安装 2015 Visual C++ 可再发行程序包。

## <a name="modify-windows-registry-steps"></a>修改 Windows 注册表步骤

修改注册表项以启用 Windows应用程序事件日志中的 SQL Server 连接器日志记录错误和信息事件。

> [!CAUTION]
> 请仔细按照此部分中的步骤进行操作，并承担相应的风险。 如果没有正确修改注册表，可能会出现严重问题。 修改注册表之前，请[备份注册表](https://support.microsoft.com/help/322756)，以便在出现问题时还原。

1. 可以通过两种方式在 Windows 10 中打开注册表编辑器：
    - 在任务栏上的搜索框中，键入 regedit。 然后，选择注册表编辑器（桌面应用）的最佳结果。

    ![ekm regedit 打开](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-open.png "ekm regedit 打开")
    - 按住或右键单击“开始”按钮，然后选择“运行”。 在对话框中输入 regedit，然后选择“确定”。

   ![ekm regedit 开始](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-start.png "ekm regedit 开始")

1. 导航到此注册表项：

    HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\

    ![ekm regedit akv](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv.png "ekm regedit akv")  

1. 在“Azure Key Vault”下添加名为 `Log` 的新项：

    HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\Log\\

    ![ekm regedit akv log](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log.png "ekm regedit akv log.png")  

1. 在“Log”项下方，添加一个名为 `Level` 的 DWORD（32 位）值：

    HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\Log\\

    ![ekm regedit akv log dword](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log-dword.png "ekm regedit akv log dword")  

1. 将 DWORD 的值设置为适当的日志级别（0、1、2）：
   1. 0（信息）- 默认值
   1. 1（错误）
   1. 2（无日志）

   ![ekm regedit akv 日志级别](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log-level.png "ekm regedit akv 日志级别")  

本文中所述的注册表项可在此项下找到：

```console
\Computer
    \HKEY_LOCAL_MACHINE
       \SOFTWARE
          \Microsoft
             \SQL Server Cryptographic Provider
                \Azure Key Vault
                   \Log\
                      <Level>
```

也可以使用命令行生成此项：

```cmd
--Create the logging parameter using (Administrator) Command Line:
REG ADD "HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\Log" /v Level /t REG_DWORD /d 1 

--Validate the new registry entry
REG QUERY "HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\Log" /v Level
```

还可以使用注册表项修复遗漏消息的应用程序事件日志条目。 该事件日志可能包含消息 `The description for Event ID 0 from source SQL Server Connector for Microsoft Azure Key Vault cannot be found...`。  

```cmd
--Create the registry entry to enable missing messages (this works with any version)
REG ADD "HKLM\SYSTEM\ControlSet001\Services\EventLog\Application\SQL Server Connector for Microsoft Azure Key Vault" /v EventMessageFile /t REG_EXPAND_SZ /d "C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll"

--Validate the new registry entry
REG QUERY "HKLM\SYSTEM\ControlSet001\Services\EventLog\Application\SQL Server Connector for Microsoft Azure Key Vault" /v EventMessageFile
```

## <a name="related-articles"></a>相关文章

- 有关其他示例脚本，请参阅博客[使用 Azure Key Vault 的 SQL Server 透明数据加密和可扩展密钥管理](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549)
- [可扩展的密钥管理 (EKM)](extensible-key-management-ekm.md)  
- [使用 Azure Key Vault 的可扩展密钥管理](extensible-key-management-using-azure-key-vault-sql-server.md)
- [SQL Server 连接器维护与故障排除](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)