---
title: 查看脱机日志文件 | Microsoft Docs
description: 了解如何在目标实例处于脱机状态或无法启动时，从 SQL Server 的本地或远程实例查看 SQL Server 日志文件。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Log File Viewer, viewing offline logs
- offline log files
ms.assetid: 9223e474-f224-4907-a4f2-081e11db58f5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4f0f124b9073335a5dc681c34fb7fa977123f04c
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96125137"
---
# <a name="view-offline-log-files"></a>查看脱机日志文件
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]开始，您可以在目标实例处于脱机状态或无法启动时，从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地或远程实例查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志文件。  
  
 您可以从注册的服务器访问脱机日志文件，或者以编程方式通过 WMI 和 WQL（WMI 查询语言）查询访问这些文件。  
  
> [!NOTE]  
>  您还可以使用这些方法连接到联机实例，但是因为某种原因，您无法通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接进行连接。  
  
## <a name="before-you-begin"></a>开始之前  
 若要连接到脱机日志文件， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例必须安装在您用于查看脱机日志文件的计算机上，以及您查看的日志文件所在的计算机上。 如果两台计算机上都安装有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，则您可以在任意一台计算机上查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的脱机文件，以及运行早期版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的脱机文件。  
  
 如果正在使用已注册的服务器，您想要连接到的实例必须在 **“本地服务器组”** 或 **“中央管理服务器”** 下注册。 实例可以单独进行注册，也可以注册为服务器组的成员。有关如何将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例添加到已注册的服务器的详细信息，请参阅以下主题：  
  
-   [创建或编辑服务器组 (SQL Server Management Studio)](../../ssms/register-servers/create-or-edit-a-server-group-sql-server-management-studio.md)  
  
-   [注册连接的服务器 (SQL Server Management Studio)](../../ssms/register-servers/register-a-connected-server-sql-server-management-studio.md)  
  
-   [创建中央管理服务器和服务器组 (SQL Server Management Studio)](../../ssms/register-servers/create-a-central-management-server-and-server-group.md)  
  
 有关如何以编程方式通过 WMI 和 WQL 查询查看脱机日志文件的详细信息，请参阅以下主题：  
  
-   [SqlErrorLogEvent 类](../../relational-databases/wmi-provider-configuration-classes/sqlerrorlogevent-class.md) （本主题显示如何检索指定日志文件中记录的事件的值。）  
  
-   [SqlErrorLogFile 类](../../relational-databases/wmi-provider-configuration-classes/sqlerrorlogfile-class.md) （本主题显示如何检索有关指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]日志文件的信息。）  
  
##  <a name="permissions"></a><a name="BeforeYouBegin"></a> 权限  
 若要连接到脱机日志文件，您必须在本地和远程计算机上同时具有以下权限：  
  
-   针对 **Root\Microsoft\SqlServer\ComputerManagement12** WMI 命名空间的读取访问权限。 默认情况下，每个人都可以通过“启用帐户”权限获得读取权限。 有关详细信息，请参阅本节后面的“验证 WMI 权限”过程。  
  
-   对包含错误日志文件的文件夹的读取权限。 默认情况下，错误日志文件位于下面的路径中（其中 \<*Drive>* 表示安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的驱动器，\<*InstanceName*> 是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称）：  
  
     \<Drive>:\Program Files\Microsoft SQL Server\MSSQL13.\<InstanceName>\MSSQL\Log  
  
 若要验证 WMI 命名空间安全设置，您可以使用 WMI 控制管理单元。  
  
#### <a name="to-verify-wmi-permissions"></a>验证 WMI 权限  
  
1.  打开 WMI 控制管理单元。 为此，请根据所用操作系统执行以下操作之一：  
  
    -   单击“开始”，在“开始搜索”框中键入 **wmimgmt.msc**，然后按 Enter。   
  
    -   依次单击 **“开始”** 、 **“运行”** ，键入 **wmimgmt.msc**，然后按 Enter。  
  
2.  默认情况下，WMI 控制管理单元管理本地计算机。  
  
     如果您想要连接到远程计算机，请执行以下步骤：  
  
    1.  右键单击“WMI 控制(本地)” ，然后单击“连接到另一台计算机” 。  
  
    2.  在 **“更改被管理的计算机”** 对话框中，单击 **“另一台计算机”** 。  
  
    3.  输入远程计算机名称，然后单击 **“确定”** 。  
  
3.  右键单击“WMI 控制(本地)”或 **“WMI 控制(** _RemoteComputerName_ **)”** ，然后单击“属性”。  
  
4.  在 **“WMI 控制属性”** 对话框中，单击 **“安全”** 选项卡。  
  
5.  在命名空间树中，找到并单击以下命名空间：  
  
     **Root\Microsoft\SqlServer\ComputerManagement10**  
  
6.  单击 **“安全性”** 。  
  
7.  确保要使用的帐户具有 **“启用帐户”** 权限。 此权限允许对 WMI 对象具有读取权限。  

### <a name="view-log-files"></a>查看日志文件  
 下面的过程显示如何通过已注册的服务器查看脱机日志文件。 该过程假设存在以下条件：  
  
 您要连接到的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例已在注册的服务器中注册。  
  
##### <a name="to-view-log-files-for-instances-that-are-offline"></a>查看脱机实例的日志文件  
  
1.  如果您要查看本地实例的脱机日志文件，请确保使用提升的权限启动 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 。 若要这样做，请在启动 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 时，右键单击“SQL Server Management Studio”，然后单击“以管理员身份运行” 。  
  
2.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的 **“视图”** 菜单上，单击 **“已注册的服务器”** 。  
  
3.  在控制台树中，找到您想要在其上查看脱机文件的实例。  
  
4.  执行下列操作之一：  
  
    -   如果实例位于“本地服务器组”下，则展开“本地服务器组”，展开服务器组（如果实例是组的成员），右键单击该实例，然后单击“查看 SQL Server 日志”  。  
  
    -   如果实例是中央管理服务器本身，则展开“中央管理服务器”，右键单击该实例，指向“中央管理服务器操作”，然后单击“查看 SQL Server 日志”  。  
  
    -   如果实例位于“中央管理服务器”下，则展开“中央管理服务器”，展开中央管理服务器，右键单击该实例（或展开服务器组并右键单击该实例），然后单击“查看 SQL Server 日志”  。  
  
5.  如果您要连接到本地实例，则使用当前用户凭据建立连接。  
  
     如果你要连接到远程实例，请在“日志文件查看器 - 连接为”  对话框中，执行以下操作之一：  
  
    -   若要以当前用户身份进行连接，请确保清除 **“以其他用户身份连接”** 复选框，然后单击 **“确定”** 。  
  
    -   若要以其他用户身份连接，请选中 **“以其他用户身份连接”** 复选框，然后单击 **“设置用户”** 。 出现提示后，输入用户凭据（以 *domain_name*\\*user_name* 格式输入用户名称），单击“确定”，然后再次单击“确定”以进行连接 。  
  
    > [!NOTE]  
    >  如果日志文件加载时间过长，你可以单击日志文件查看器工具栏上的“停止”  。  
  
## <a name="see-also"></a>另请参阅  
 [日志文件查看器](../../relational-databases/logs/log-file-viewer.md)  
  
