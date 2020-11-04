---
description: 第 1 课：连接到数据库引擎
title: 第 1 课：连接到数据库引擎 | Microsoft Docs
ms.custom: ''
ms.date: 02/05/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: quickstart
ms.assetid: e8db82f0-50ed-4531-9209-940006ed34cb
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6b1685a4d93d14b3cd49a4c9a4a031943a5b9f7e
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243826"
---
# <a name="lesson-1-connecting-to-the-database-engine"></a>第 1 课：连接到数据库引擎
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

安装 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]时，安装哪些工具取决于版本和您的安装选择。 本课将介绍主要的工具以及如何连接并执行一项基本功能（授权多个用户）。  

本课程包含以下任务：  
- [入门工具](#tools)  
- [使用 Management Studio 进行连接](#connect)  
- [授权其他连接](#additional) 

## <a name=""></a><a name="tools">入门工具</a> 
- [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 提供了多种工具。 本主题说明您首先需要的工具，并帮助选择适合于作业工具。 所有工具都可以从“开始”菜单上访问。 默认情况下不会安装 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 之类的一些工具。 必须在安装过程中将这些工具选择为客户端组件的一部分。 有关下面所述工具的完整说明，请在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 联机图书中进行搜索。 [!INCLUDE[ssExpress](../includes/ssexpress-md.md)] 仅包含其中的一部分工具。  

### <a name="basic-tools"></a>基本工具
- [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (SSMS) 是管理 [!INCLUDE[ssDE](../includes/ssde-md.md)] 和编写 [!INCLUDE[tsql](../includes/tsql-md.md)] 代码的主要工具。 它驻留在 [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 外壳中。 SSMS 可以从 [Microsoft 下载中心](../ssms/download-sql-server-management-studio-ssms.md)免费下载。 最新版本可同较旧版本的 [!INCLUDE[ssDE_md](../includes/ssde-md.md)]一同使用。  

- [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器同 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 和客户端工具一起安装。 使用它，您可以启用服务器协议，配置协议选项（例如 TCP 端口），将服务器服务配置为自动启动，以及将客户端计算机配置为以所需的方式连接。 此工具会配置更高级的连接元素，但不会启用功能。  

### <a name="sample-database"></a>示例数据库
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]不包含示例数据库和示例。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 联机丛书中所述的大多数示例都使用 [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] 示例数据库。  

##### <a name="to-start-sql-server-management-studio"></a>启动 SQL Server Management Studio
- 在 Windows 的当前版本中，在“开始”页键入 SSMS，然后单击“Microsoft SQL Server Management Studio”。  
- 使用较旧版本的 Windows 时，在“开始”菜单上，依次指向“所有程序”、[!INCLUDE[ssCurrentUI](../includes/sscurrentui-md.md)]，然后单击“SQL Server Management Studio”。  

##### <a name="to-start-sql-server-configuration-manager"></a>启动 SQL Server 配置管理器  
- 在当前版本的 Windows 中，在“开始”菜单上，键入 **Configuration Manager** ，然后单击“SQL Server 版本配置管理器”。   
- 使用较旧版本的 Windows 时，在“开始”菜单上，依次指向“所有程序”、[!INCLUDE[ssCurrentUI](../includes/sscurrentui-md.md)]、“配置工具”，然后单击“SQL Server 配置管理器”。  

## <a name="connecting-with-management-studio"></a><a name="connect"></a>使用 Management Studio 进行连接  
- 如果知道实例名并且以计算机上的本地管理员组成员身份进行连接，则可以使用同一台计算机上运行的工具轻松连接到 [!INCLUDE[ssDE](../includes/ssde-md.md)] 。 必须在承载 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的同一台计算机上执行下面的过程。  

> [!NOTE]  
> 本主题讨论连接到本地 SQL Server 的相关内容。 要连接到 Azure SQL 数据库，请参阅 [Connect to SQL Database with SQL Server Management Studio and execute a sample T-SQL query](/azure/azure-sql/database/connect-query-ssms)（使用 SQL Server Management Studio 连接到 SQL 数据库并执行示例 T-SQL 查询）。  

##### <a name="to-determine-the-name-of-the-instance-of-the-database-engine"></a>确定数据库引擎实例的名称  

1.  以 Administrators 组成员身份登录到 Windows，然后打开 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]。  
2.  在 **“连接到服务器”** 对话框中，单击 **“取消”** 。  
3.  如果未显示“已注册的服务器”，请在“视图”菜单中，单击“已注册的服务器”。
4.  在“已注册的服务器”工具栏中选择“数据库引擎”后，展开“数据库引擎”，右键单击“本地服务器组”，指向“任务”，然后单击“注册本地服务器”。 展开“本地服务器组”以查看所显示计算机上安装的 [!INCLUDE[ssDE](../includes/ssde-md.md)] 的所有实例。 默认实例未命名，并显示为计算机名称。 命名实例显示为计算机名称，后跟反斜杠 (\\)，然后是实例名。 对于 [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]，除非在安装过程中更改了名称，否则实例将命名为 *<computer_name>* \sqlexpress。  

##### <a name="to-verify-that-the-database-engine-is-running"></a>验证数据库引擎是否正在运行

1.  在“已注册的服务器”中，如果 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例的名称中有绿色的点并在名称旁边有白色箭头，则表示 [!INCLUDE[ssDE](../includes/ssde-md.md)] 正在运行，无需执行其他操作。  

2.  如果 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例的名称中有红色的点并在名称旁边有白色正方形，则表示[!INCLUDE[ssDE](../includes/ssde-md.md)]已停止。 右键单击 [!INCLUDE[ssDE](../includes/ssde-md.md)] 的名称，单击“服务控制”，然后单击“开始”。 出现确认对话框之后，[!INCLUDE[ssDE](../includes/ssde-md.md)]应启动，圆圈应变为绿色且带有白色箭头。  

##### <a name="to-connect-to-the-database-engine"></a>连接到数据库引擎  

安装 [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 时，至少选中了一个管理员帐户。 以管理员身份登录到 Windows 时，执行以下步骤。

1.  在 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 中的“文件”菜单上，单击“连接对象资源管理器”。 
- 将打开“连接到服务器”对话框。 “服务器类型”框中将显示上次使用的组件的类型。  

2.  选择“数据库引擎”。

![对象资源管理器的屏幕截图，其中显示“连接”下拉列表，并突出显示了“数据库引擎”选项。](../relational-databases/media/object-explorer.png)

3.  在“服务器名称”框中，键入 [!INCLUDE[ssDE](../includes/ssde-md.md)] 实例的名称。 对于默认的 SQL Server 实例，服务器名称即计算机名称。 对于 SQL Server 的命名实例，服务器名称为 _\<computer_name\>_ **\\** _\<instance_name\>_ ，如 ACCTG_SRVR\SQLEXPRESS。 以下屏幕截图显示连接到名为“PracticeComputer”的计算机上 [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 的默认（未命名）实例。 已登录到 Windows 的用户是 Contoso 域中的 Mary。 使用 Windows 身份验证时，无法更改用户名称。 

![“连接到服务器”对话框的屏幕截图，其中突出显示了“服务器名称”文本框。](../relational-databases/media/connect-to-server.png)

4.  单击“连接”。

> [!NOTE]
> 本教程假定你刚接触 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ，并且连接时没有出现特殊问题。 这应足以满足大多数人的需求，并使本教程保持简单。 有关疑难解答步骤的详细信息，请参阅 [对连接到 SQL Server 数据库引擎的疑难解答](../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)。 

## <a name="authorizing-additional-connections"></a><a name="additional"></a>授权其他连接  
现在，您已经以管理员身份连接到了 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ，您的首要任务之一是授权其他用户进行连接。 实现此任务的步骤是创建一个登录名，然后授权此登录名以用户身份访问数据库。 登录名可以是使用 Windows 凭据的 Windows 身份验证登录名；也可以是 SQL Server 身份验证登录名（这些登录名在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中存储身份验证信息并独立于 Windows 凭据）。 尽可能使用 Windows 身份验证。

> [!TIP]
> 大多数组织具有域用户，且将使用 Windows 身份验证。 可以通过在计算机上创建其他本地用户，自行进行试验。 计算机将对本地用户进行身份验证，因而域为计算机名称。 例如，如果计算机名为 `MyComputer`，并且创建了一个名为 `Test` 的用户，则 Windows 对用户的描述是 `Mycomputer\Test`。  

##### <a name="create-a-windows-authentication-login"></a>创建 Windows 身份验证登录名 

1.  在上一个任务中，您使用 [!INCLUDE[ssDE](../includes/ssde-md.md)] 连接到了[!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]。 在对象资源管理器中，依次展开服务器实例、“安全性”，右键单击“登录名”，再单击“新建登录名”。 将显示“登录名 - 新建”对话框。  

2.  在“常规”页中的“登录名”框中，键入以下格式的 Windows 登录名：`<domain>\\<login>`

![“登录 - 新建”对话框的屏幕截图，其中突出显示了“登录名称”文本框。](../relational-databases/media/new-login.png)

3.  在“默认数据库”框中，选择 [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)]（如果有）。 否则选择“master”。  
4.  在“服务器角色”页中，如果新建登录名要成为管理员，则单击“sysadmin”，否则保留此项为空白。  
5.  在“用户映射”页中，针对 [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] 数据库（如果有）选择“映射”。 否则选择“master”。 注意，“用户”框使用该登录名进行填充。 关闭后，该对话框将在数据库中创建此用户。  
6.  在“默认架构”框中，键入 **dbo** 将登录名映射到数据库所有者架构。   
7.  接受“安全对象”和“状态”框的默认设置，然后单击“确定”创建登录名。  

> [!IMPORTANT]  
> 这是基本的入门信息。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供了丰富的安全环境，而安全显然是数据库操作的一个重要方面。  

## <a name="next-lesson"></a>下一课  
[第 2 课：从其他计算机进行连接](../relational-databases/lesson-2-connecting-from-another-computer.md)    
