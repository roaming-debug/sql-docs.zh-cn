---
description: '在 SQL Server 上安装 SSMA 组件 (SybaseToSQL) '
title: 在 SQL Server 上安装 SSMA 组件 (SybaseToSQL) |Microsoft Docs
ms.custom: ''
ms.date: 07/14/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 5ad9e12c-2cdb-4dd2-8703-05a23242d19d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 82f5d7eef228326f0dba9b194b87bffce4bb5eff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078228"
---
# <a name="installing-ssma-components-on-sql-server-sybasetosql"></a>在 SQL Server 上安装 SSMA 组件 (SybaseToSQL) 

除了安装 SSMA 外，还必须在运行的计算机上安装组件，才能使用服务器端数据迁移 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 这些组件包括支持数据迁移的 SSMA 扩展包和用于启用服务器到服务器连接的 Sybase 提供程序。

## <a name="ssma-for-sybase-extension-pack"></a>用于 Sybase 扩展包的 SSMA

SSMA 扩展包将数据库、 **sysdb** 和 **ssmatesterdb_syb** 添加到指定的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 **Sysdb** 数据库包含迁移数据所需的表和存储过程。 **Ssmatester_syb** 数据库包含架构 **ssma_sybase_utilities**，在这种情况下，将在其中创建 ssma 测试器组件使用的对象 (表、触发器和视图) 。

此外，在将数据迁移到时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，SSMA [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会在服务器端数据迁移引擎用于迁移数据时创建代理作业。

### <a name="prerequisites"></a>必备条件

在上安装用于 Sybase server 组件的 SSMA 之前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请确保系统满足以下要求：

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已安装实例。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3.1 或更高版本。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] 版本4.7.2 或更高版本。 你可以从 [.NET Framework 开发人员中心](https://go.microsoft.com/fwlink/?LinkId=48882)获取它。
- Sybase OLE DB/ADO.Net/ODBC 提供程序和与 SAP ASE 数据库服务器的连接，其中包含要迁移的数据库。 可以从 SAP ASE 产品媒体安装提供程序。 有关连接的详细信息，请参阅 [连接到 SYBASE ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md)。
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]必须在安装过程中运行 Browser 服务。 这用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在安装向导中填充实例的列表。 你可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装后禁用 Browser 服务。

  > [!NOTE]
  > 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务正在运行，但你仍未在安装程序中看到实例列表，则必须解除 UDP 端口1434的阻止。 可以使用 Windows 防火墙暂时取消阻止端口，也可以暂时禁用 Windows 防火墙。 你可能还必须暂时禁用防病毒软件。 请确保在安装后启用防火墙和防病毒软件。

### <a name="installing-the-extension-pack"></a>安装扩展包

在将数据迁移到之前，你可以随时安装扩展包 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

> [!IMPORTANT]
> 若要安装扩展包，您必须是实例上 sysadmin 服务器角色的成员 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

安装扩展包：

1. 将 **SSMAforSybaseExtensionPack_ *n*.msi**（其中 *n* 是内部版本号）复制到运行的计算机 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。
2. 双击 SSMAforSybaseExtensionPack_ " **。**
3. 在“欢迎”页面上，单击“下一步”。 
4. 在 " **最终用户许可协议** " 页上，阅读许可协议。 如果同意，请选择 " **我接受协议"** 选项，然后单击 " **下一步**"。
5. 在 " **选择安装类型** " 页上，单击 " **典型**"。
6. 在 " **准备安装** " 页上，单击 " **安装**"。
7. 在 " **完成第一步安装** " 页上，单击 " **下一** 步"。

   此时将显示一个新对话框，您可以在其中选择 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用于安装扩展包的实例。

8. 选择 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 要将 SAP ASE 数据库迁移到的实例，然后单击 " **下一步**"。

   默认实例与计算机的名称相同。 命名实例后跟反斜杠和实例名称。

9. 在 "连接" 页上，选择身份验证方法，然后单击 " **下一步**"。

   Windows 身份验证将使用您的 Windows 凭据来尝试登录到的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果选择 "服务器身份验证"，则必须输入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和密码。

10. 下一步要求你设置主密钥的密码，主密钥用于在服务器端数据迁移期间对存储在扩展包数据库中的所有敏感数据进行加密。 提供强密码，并单击 " **下一步**"。

11. 在下一页上，选择 " **安装实用程序数据库 *n* " 并安装扩展包库**，其中 *n* 是版本号。 如果计划使用测试人员功能，请选择 " **安装测试人员数据库** " 复选框，然后选择 " **下一步**"。

    **Sysdb** 数据库是使用 (在此数据库中创建的，这些表和存储过程是使用) 在此数据库中创建的。

    如果选中了 " **安装测试器数据库** " 选项，则将创建 **ssmatesterdb_syb** 数据库。

12. 安装完成后，将显示一条提示，询问你是否要在另一个实例上安装实用工具数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，选择 **"是**"，然后选择 " **下一步**"，或者退出向导，选择 " **否** "，然后选择 " **退出**"。

### <a name="sql-server-database-objects"></a>SQL Server 数据库对象

安装扩展包后，你将看到 **sysdb** 数据库中的 **ssma_syb _migration_packages** 表。 你还将看到以下存储过程：

- `bcp_clean_migration_data`
- `bcp_ensure_message_table`
- `bcp_insert_new_message`
- `bcp_post_process`
- `bcp_read_new_migration_messages`
- `bcp_save_migration_package`
- `bcp_smart_truncate`
- `bcp_start_migration_process`
- `get_jobstep_info`
- `stop_agent_process`

每次将数据迁移到时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，SSMA 会创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。 这些作业 **ssma_syb 数据迁移包 {GUID}** 命名，在 "作业" 文件夹的 "代理" 节点中可见 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。  

## <a name="sybase-providers"></a>Sybase 提供程序

使用服务器端数据迁移将数据从 SAP ASE 移到时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，数据直接在 SAP ase 和之间迁移 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 它不会经历 SSMA，因为这会降低数据迁移速度。

### <a name="installing-the-sybase-providers"></a>安装 Sybase 提供程序

以下说明提供了安装 Sybase 提供程序的基本安装步骤。 确切的说明将有所不同，具体取决于 Sybase 安装程序的版本。

> [!IMPORTANT]
> 在运行安装程序之前，请验证你是否不违反许可协议。

1. 运行 Sybase ASE 安装程序。
2. 选择 "自定义安装"。
3. 在 "功能选择" 页上，选择 ODBC、OLE DB 和 ADO.NET 数据提供程序。
4. 验证所选功能，然后单击 " **完成** " 以安装数据提供程序。

## <a name="see-also"></a>另请参阅

- [为 Sybase 客户端安装 SSMA](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)
- [将 Sybase ASE 数据库迁移到 SQL Server-Azure SQL 数据库](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)
