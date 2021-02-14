---
title: 在 SQL Server 上安装 SSMA 组件 (OracleToSQL) |Microsoft Docs
description: 了解如何在运行 SQL Server 的计算机上安装 SSMA 扩展包和 Oracle 提供程序以支持 Oracle 数据库转换。
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Installing the extension pack
- SQL Server Database Objects
ms.assetid: 33070e5f-4e39-4b70-ae81-b8af6e4983c5
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 81d3f542e64030dbf6a0bf70067307222bef5a8b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344209"
---
# <a name="installing-ssma-components-on-sql-server-oracletosql"></a>在 SQL Server 上安装 SSMA 组件 (OracleToSQL) 

除了安装 SSMA 外，还必须在运行的计算机上安装组件 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 这些组件包括支持数据迁移的 SSMA 扩展包和用于启用服务器到服务器连接的 Oracle 提供程序。

## <a name="ssma-for-oracle-extension-pack"></a>Oracle 扩展包的 SSMA

SSMA 扩展包将部署扩展存储过程，并将 **sysdb** 和 **ssmatesterdb** 数据库添加到指定的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 扩展存储过程提供模拟 Oracle 的功能和 behaiov 所需的功能，而 **sysdb** 数据库包含迁移数据所需的表和存储过程。 如果安装) ， **ssmatesterdb** 数据库包含测试人员组件 (所需的表和过程。

此外，在将数据迁移到时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，SSMA [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会在服务器端数据迁移引擎用于迁移数据时创建代理作业。

### <a name="prerequisites"></a>先决条件

在上安装用于 Oracle 服务器组件的 SSMA 之前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请确保系统满足以下要求：

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已安装实例。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3.1 或更高版本。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] 版本4.7.2 或更高版本。 你可以从 [.NET Framework 开发人员中心](https://go.microsoft.com/fwlink/?LinkId=48882)获取它。
- 用于 Oracle (的 OLE DB 提供程序，如果使用 OLE DB) ，并连接到要迁移的 Oracle 数据库。 你可以从 Oracle 产品媒体或 Oracle 网站安装提供程序。
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]必须在安装过程中运行 Browser 服务。 这用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在安装向导中填充实例的列表。 你可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装后禁用 Browser 服务。

  > [!NOTE]
  > 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务正在运行，但你仍未在安装程序中看到实例列表，则必须解除 UDP 端口1434的阻止。 可以使用 Windows 防火墙暂时取消阻止端口，也可以暂时禁用 Windows 防火墙。 你可能还必须暂时禁用防病毒软件。 请确保在安装后启用防火墙和防病毒软件。

### <a name="installing-the-extension-pack"></a>安装扩展包

在将数据迁移到之前，你可以随时安装扩展包 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

> [!IMPORTANT]
> 若要安装扩展包，您必须是实例上 **sysadmin** 服务器角色的成员 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

安装扩展包：

1. 将 **SSMAforOracleExtensionPack_ *n*.msi** (，其中 *n* 是) 到运行的计算机的生成号 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。
2. 双击 SSMAforOracleExtensionPack_ " **。**
3. 在“欢迎”页面上，单击“下一步”。 
4. 在 " **最终用户许可协议** " 页上，阅读许可协议。 如果同意，请选择 **"我接受协议"** 选项，然后单击 " **下一步**"。
5. 在 " **选择安装类型** " 页上，选择 " **典型**"。
6. 在 " **准备安装** " 页上，选择 " **安装**"。
7. 在 " **完成第一步安装** " 页上，选择 " **下一** 步"。
  
   此时将显示一个新对话框。 选择扩展包类型。
  
8. 选择所需的安装类型，然后单击 " **下一步**"。

   > [!IMPORTANT]
   > 远程选项仅应在安装扩展包（在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Linux 上运行或面向时）时使用 [!INCLUDE[ssAzureMi](../../includes/ssazuremi_md.md)] 。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 Windows 上运行的安装应始终本地安装扩展包。 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 和 Azure Synapse Analytics 不支持扩展包。

   如果要在本地实例上安装扩展包 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则下一页将允许您选择要将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Oracle 架构迁移到的本地实例。 在下拉箭头中选择一个实例，然后选择 " **下一步**"。

   默认实例与计算机的名称相同。 命名实例后跟反斜杠和实例名称。

9. 在 "连接" 页上，选择身份验证方法，然后选择 " **下一步**"。

   Windows 身份验证将使用您的 Windows 凭据来尝试登录到的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果选择 "服务器身份验证"，则必须输入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和密码。

10. 下一步要求你设置主密钥的密码，主密钥用于在服务器端数据迁移期间对存储在扩展包数据库中的所有敏感数据进行加密。 提供强密码，并单击 " **下一步**"。

11. 在下一页上，选择 " **安装实用程序数据库 *n* " 并安装扩展包库**，其中 *n* 是版本号。 如果计划使用测试人员功能，请选择 " **安装测试人员数据库** " 复选框，然后选择 " **下一步**"。

    **Sysdb** 数据库是使用 (在此数据库中创建的，这些表和存储过程是使用) 在此数据库中创建的。

    如果选中了 " **安装测试器数据库** " 选项，则将创建 **ssmatesterdb** 数据库。

12. 安装完成后，将显示一条提示，询问你是否要在另一个实例上安装实用工具数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，选择 **"是**"，然后选择 " **下一步**"，或者退出向导，选择 " **否** "，然后选择 " **退出**"。

13. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或使用 `sqlcmd` 实用工具，运行以下脚本以启用 CLR：

    ```sql
    sp_configure 'clr enabled', 1
    GO
    RECONFIGURE
    GO
    ```

    如果未启用 CLR，则当 SSMA 连接到时，你将收到以下错误 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ：

    > SSMA 无法检索扩展包程序集版本信息。 在数据库服务器上重新安装扩展包。

### <a name="sql-server-database-objects"></a>SQL Server 数据库对象

安装扩展包后， **ssma_oracle _migration_packages** 表将出现在 **sysdb** 数据库中。

每次将数据迁移到时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，SSMA 会创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。 这些作业 **ssma_oracle 数据迁移包 {GUID}** 命名，在 "作业" 文件夹的 "代理" 节点中可见 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。

此外，将在 **master** 数据库中添加以下扩展存储过程：

- `xp_ora2ms_exec2`
- `xp_ora2ms_exec2_ex`
- `xp_ora2ms_versioninfo2`

## <a name="see-also"></a>请参阅

- [为 Oracle 客户端安装 SSMA](../../ssma/oracle/installing-ssma-for-oracle-client-oracletosql.md)
- [将 Oracle 数据库迁移到 SQL Server](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)
