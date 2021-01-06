---
title: 在系统管理员被锁定时连接到 SQL Server | Microsoft Docs
description: 了解如何以系统管理员的身份重新获得对 SQL Server 的访问权限（如果被错误锁定）。
ms.custom: contperf-fy20q4
ms.date: 05/20/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- sa account
- connecting when locked out [SQL Server]
- locked out [SQL Server]
ms.assetid: c0c0082e-b867-480f-a54b-79f2a94ceb67
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3b4cf5946fba4eb3c79150b29ef44bd232f9c155
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878940"
---
# <a name="connect-to-sql-server-when-system-administrators-are-locked-out"></a>在系统管理员被锁定时连接到 SQL Server 
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
本文介绍如何以系统管理员身份重新获得对 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的访问权限（如果你已被锁定）。系统管理员可能会由于下列原因之一失去对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的访问权限：  
  
-   作为 sysadmin 固定服务器角色成员的所有登录名都已经被误删除。  
  
-   作为 sysadmin 固定服务器角色成员的所有 Windows 组都已经被误删除。  
  
-   作为 sysadmin 固定服务器角色成员的登录名用于已经离开公司或者无法找到的个人。  
  
-   sa 帐户被禁用或者没有人知道密码。  
  
## <a name="resolution"></a>解决方法

为了解决访问权限问题，我们建议在单用户模式下启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 此模式可防止在你尝试重新获得访问权限时发生其他连接。 在此，你可以连接到 SQL Server 实例，并将登录名添加到“sysadmin”服务器角色。 此解决方案的详细步骤在[分步说明](#step-by-step-instructions)部分中提供。


可以从命令行使用 `-m` 或 `-f` 选项以单用户模式启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例。 计算机的本地管理员组的任何成员都可以随后作为“sysadmin”固定服务器角色的成员连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  

在单用户模式下启动实例时，请首先停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务。 否则，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理可能会首先进行连接，获取到服务器的唯一可用连接并阻止你登录。

未知客户端应用程序也可能在能够登录之前获取唯一可用的连接。 为了防止发生这种情况，可以使用后跟应用程序名称的 `-m` 选项将连接限制为来自指定应用程序的单个连接。 例如，用 `-mSQLCMD` 启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将连接限制为将自身标识为 sqlcmd 客户端程序的单个连接。 若要通过 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中的查询编辑器进行连接，请使用 `-m"Microsoft SQL Server Management Studio - Query"`。  


> [!IMPORTANT]  
> 请勿将 `-m` 与应用程序名称一起用作安全功能。 客户端应用程序通过连接字符串设置指定应用程序名称，因此很容易受假名称欺骗。

下表总结了在命令行中以单用户模式启动实例的不同方法。

| 选项 | 说明 | 何时使用 |
|:---|:---|:---|
|`-m` | 将连接限制为单个连接 | 当没有其他用户尝试连接到实例，或者不确定用于连接到实例的应用程序名称时。 |
|`-mSQLCMD`| 将连接限制为必须将自身标识为 sqlcmd 客户端程序的单个连接| 当计划使用 sqlcmd 连接到实例，并且想要阻止其他应用程序使用唯一可用的连接时。 |
|`-m"Microsoft SQL Server Management Studio - Query"`| 将连接限制为必须将自身标识为“Microsoft SQL Server Management Studio - 查询”应用程序的单个连接。| 当计划通过 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中的查询编辑器连接到实例，并且想要阻止其他应用程序使用唯一可用的连接时。 |
|`-f`| 将连接限制为单个连接，并以最小配置启动实例 | 当某个其他配置阻止你启动时。 |
| &nbsp; | &nbsp; | &nbsp; |
  
有关如何在单用户模式下启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的分步说明，请参阅[在单用户模式下启动 SQL Server](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md)。

## <a name="step-by-step-instructions"></a>分步说明

以下分步说明介绍了如何将系统管理员权限授予被错误剥夺访问权限的 SQL Server 登录名。

这些说明假定，

* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 Windows 8 或更高版本上运行。 在适用的情况下，将对早期的 SQL Server 或 Windows 版本进行细微的调整。

* [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 安装在计算机上。  

在以本地管理员组的成员身份登录到 Windows 时，请执行以下指令。

1.  在 Windows 的“开始”菜单中，右键单击 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Configuration Manager 的图标，然后选择“以管理员身份运行”将管理员凭据传递给 Configuration Manager。  
  
2.  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器的左窗格中，选择“SQL Server 服务” 。 在右窗格中，查找 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 （[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的默认实例包括在计算机名称后的 **(MSSQLSERVER)** 。 命名实例显示为大写，名称与在“已注册的服务器”中的名称相同。）右键单击 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例，然后单击“属性”。  
  
3.  在“启动参数”选项卡上的“指定启动参数”框中，键入 `-m`，然后单击“添加”  。 （这是短划线后跟小写字母 m。）  
  
    > [!NOTE]  
    >  对于某些早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，没有“启动参数”  选项卡。在这种情况下，在“高级”选项卡上，双击“启动参数” 。 参数在非常小的窗口中打开。 请注意不要更改任何现有参数。 在最后，添加新参数 `;-m`，然后单击“确定”。 （这是一个分号，后跟短划线和小写字母 m。）  
  
4.  单击“确定”，在显示重启的消息后右键单击你的服务器名称，然后单击“重启” 。  
  
5.  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 重启之后，服务器将处于单用户模式。 请确保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理未在运行。 如果启动，它将占用您唯一的连接。  
  
6.  在 Windows 的“开始”菜单中，右键单击 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 图标，然后选择“以管理员身份运行”。 这会将你的管理员凭据传递到 SSMS。
  
    > [!NOTE]  
    >  对于 Windows 的早期版本，“以管理员身份运行”选项显示为子菜单。  
  
     在某些配置中，SSMS 将尝试进行多个连接。 多个连接将失败，因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 处于单用户模式。 视具体情况，执行以下操作之一。  
  
    1.  使用 Windows 身份验证（包括管理员凭据）与对象资源管理器连接。 依次展开“安全性”和“登录名”，然后双击你自己的登录名 。 在“服务器角色”  页上，选择 **sysadmin**，然后单击“确定” 。  
  
    2.  使用 Windows 身份验证（包括您的管理员凭据）与“查询窗口”连接，而非与对象资源管理器连接。 （如果未与对象资源管理器连接，则只能这样连接）。执行类似以下的代码，以添加作为 **sysadmin** 固定服务器角色成员的新 Windows 身份验证登录名。 以下示例添加名为 `CONTOSO\PatK`的域用户。  
  
        ```  
        CREATE LOGIN [CONTOSO\PatK] FROM WINDOWS;  
        ALTER SERVER ROLE sysadmin ADD MEMBER [CONTOSO\PatK];  
        ```  
  
    3.  如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 正在混合身份验证模式下运行，请使用 Windows 身份验证（包括您的管理员凭据）与“查询窗口”连接。 执行类似以下的代码，以创建作为 sysadmin 固定服务器角色成员的新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录名。  
  
        ```  
        CREATE LOGIN TempLogin WITH PASSWORD = '************';  
        ALTER SERVER ROLE sysadmin ADD MEMBER TempLogin;  
        ```  
  
        > [!WARNING]  
        >  使用强密码替换 ************。  
  
    4.  如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 正在混合身份验证模式下运行且你要重置 **sa** 帐户的密码，则请使用 Windows 身份验证（包括你的管理员凭据）与“查询窗口”连接。 使用以下语法更改 **sa** 帐户的密码。  
  
        ```  
        ALTER LOGIN sa WITH PASSWORD = '************';  
        ```  
  
        > [!WARNING]  
        >  使用强密码替换 ************。  

7. 关闭 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]。  
  
8. 接下来的几个步骤将 SQL Server 改回多用户模式。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器的左窗格中，选择“SQL Server 服务” 。

9. 在右侧窗格中，右键单击 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例，然后单击“属性”。  
  
10. 在“启动参数”选项卡上的“现有参数”框中，选择 `-m`，然后单击“删除”  。  
  
    > [!NOTE]  
    >  对于某些早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，没有“启动参数”  选项卡。在这种情况下，在“高级”选项卡上，双击“启动参数” 。 参数在非常小的窗口中打开。 删除您以前添加的 `;-m` ，然后单击“确定” 。  
  
11. 右键单击你的服务器名称，然后单击“重启”。 如果在单用户模式下启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之前停止了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理，请确保再次启动它。
  
现在您应能与以下帐户之一正常连接：它们是 **sysadmin** 固定服务器角色的成员。  
  
## <a name="see-also"></a>另请参阅  

* [配置服务器启动选项](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)
* [数据库引擎服务启动选项](../../database-engine/configure-windows/database-engine-service-startup-options.md)  
