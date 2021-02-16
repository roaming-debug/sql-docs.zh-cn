---
description: Integration Services 服务（SSIS 服务）
title: Integration Services 服务（SSIS 服务）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssiseditserverregistration.connectionproperties.f1
- sql13.swb.connecttodts.connectionproperties.f1
- sql13.swb.connection.login.dtsserver.f1
- sql13.swb.connecttodts.login.f1
- sql13.swb.connecttodtsserver.login.f1
helpviewer_keywords:
- Integration Services service, about Integration Services service
- SQL Server Integration Services service
- service [Integration Services],about Integration Services service
- service [Integration Services]
- SQL Server Integration Services, service
ms.assetid: 2c785b3b-4a0c-4df7-b5cd-23756dc87842
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e6174c866766dba8041a15387cc0d2779d5b892c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346567"
---
# <a name="integration-services-service-ssis-service"></a>Integration Services 服务（SSIS 服务）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  本节中的主题论述 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务，该服务是用于管理 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的一种 Windows 服务。 此服务不是创建、保存和运行集成服务包所必需的。 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 支持 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务以便与 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的早期版本向后兼容。  
  
 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 将对象、设置和运行数据存储在使用项目部署模型部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的项目的 SSISDB 数据库中。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库引擎的实例，它承载该数据库。 有关数据库的详细信息，请参阅 [SSIS 目录](../../integration-services/catalog/ssis-catalog.md)。 有关将项目部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的详细信息，请参阅[部署 Integration Services (SSIS) 项目和包](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)。  
  
## <a name="management-capabilities"></a>管理功能  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务是用于管理 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的 Windows 服务。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务只在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中可用。  
  
 运行 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务可以提供下列管理功能：  
  
-   启动远程和本地存储的包  
  
-   停止远程和本地运行的包  
  
-   监视远程和本地运行的包  
  
-   导入和导出包  
  
-   管理包存储  
  
-   自定义存储文件夹  
  
-   在服务停止时停止正在运行的包  
  
-   查看 Windows 事件日志  
  
-   连接到多台 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器  
  
## <a name="startup-type"></a>启动类型
 在安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件时会安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]服务。 默认情况下， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务已启动，该服务的启动类型已设置为自动。 该服务必须运行，才能监视存储在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 包存储区中的包。 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 包存储区可以是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例中的 msdb 数据库，也可以是文件系统中指定的文件夹。  
  
 如果只希望设计和执行 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包，则 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务不是必需的。 但是，若要使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]列出和监视包，则该服务是必需的。  

## <a name="manage-the-service"></a>管理服务
  
 在安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]组件时，也会安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务。 默认情况下， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务已启动，该服务的启动类型已设置为自动。 不过，若要使用该服务来管理已存储的和正在运行的 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 包，还必须安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 。  
  
> [!NOTE]
> 若要直接连接到旧 Integration Services 服务的实例，则必须使用与正在运行 Integration Services 服务的 SQL Server 版本保持一致的 SQL Server Management Studio (SSMS) 版本。 例如，要连接到在 SQL Server 2016 的实例上运行的旧 Integration Services 服务，则必须使用 SQL Server 2016 的 SSMS 版本。 [下载 SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)。
>
>   在 SSMS 的“连接到服务器”对话框中，不能输入正在运行早期版本 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的服务器的名称。 但是，若要管理存储在某远程服务器上的包，则不必连接到该远程服务器上 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的实例。 只需编辑 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的配置文件，以便 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 显示存储在远程服务器上的包。   
  
 在一台计算机上可以只安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的一个实例。 该服务并非特定于某个 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例。 可以使用正在运行该服务的计算机的名称连接到该服务。  
  
 可以使用以下任一 Microsoft 管理控制台 (MMC) 管理单元来管理 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务：SQL Server 配置管理器或服务。 若要在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中管理包，必须首先确保该服务已启动。  
  
 默认情况下， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务配置为管理 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例的 msdb 数据库中的包，该实例与 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]同时安装。 如果未同时安装 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，则 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务可配置为管理本地默认 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的 msdb 数据库中的包。 若要管理 [!INCLUDE[ssDE](../../includes/ssde-md.md)]某个命名实例或远程实例中存储的包或 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的多个实例中存储的包，则必须修改该服务的配置文件。
  
 默认情况下， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务配置为在该服务停止时停止正在运行的包。 但是， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务不会等待包停止，因此，在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务停止后，一些包可能仍在运行。  
  
 如果 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务已停止，可使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导、 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器、执行包实用工具以及 **dtexec** 命令提示实用工具 (dtexec.exe) 继续运行包。 但是，您无法监视正在运行的包。  
  
 默认情况下， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务在 NETWORK SERVICE 帐户的上下文中运行。 建议在具有有限权限的帐户（如 NETWORK SERVICE 帐户）下运行 SQL Server Integration Services 服务。 在高特权帐户下运行 SQL Server Integration Services 服务有着潜在的安全风险。 
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务将信息写入 Windows 事件日志。 您可以在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中查看服务事件。 也可以通过 Windows 事件查看器查看服务事件。  
  
## <a name="set-the-properties-of-the-service"></a>设置服务的属性
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务管理并监视 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的包。 首次安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 时，[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务会启动，启动类型设置为自动。  
  
 安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务后，可以使用 SQL Server 配置管理器或 Services MMC 管理单元设置其属性。  
  
 若要配置该服务的其他重要功能（包括在何处存储和管理包），则必须修改服务的配置文件。
  
### <a name="to-set-properties-of-the-integration-services-service-by-using-sql-server-configuration-manager"></a>使用 SQL Server 配置管理器设置 Integration Services 服务的属性  
  
1.  在 **“开始”** 菜单中，依次指向 **“所有程序”** 、 **“Microsoft SQL Server”** 和 **“配置工具”** ，然后单击 **“SQL Server 配置管理器”** 。  
  
2.  在“SQL Server 配置管理器”管理单元中，在服务列表中找到 **SQL Server Integration Services**，右键单击 **SQL Server Integration Services**，然后单击“属性”。  
  
3.  在 **“SQL Server Integration Services 属性”** 对话框中，可以执行下列操作：  
  
    -   单击 **“登录”** 选项卡以查看诸如帐户名等登录信息。  
  
    -   单击 **“服务”** 选项卡以查看有关服务的信息（例如，主机计算机的名称），并指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的启动模式。  
  
        > [!NOTE]  
        >  “高级”选项卡不包含 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的信息。  
  
4.  单击“确定”。   
  
5.  在“文件”菜单上，单击“退出”以关闭“SQL Server 配置管理器”管理单元。  
  
### <a name="to-set-properties-of-the-integration-services-service-by-using-services"></a>使用“服务”设置 Integration Services 服务的属性  
  
1.  在 **“控制面板”** 中，如果使用的是经典视图，请单击 **“管理工具”**；如果使用的是分类视图，请单击 **“性能和维护”** ，再单击 **“管理工具”**。  
  
2.  单击 **“服务”**。  
  
3.  在“服务”管理单元中，在服务列表中找到 **SQL Server Integration Services**，右键单击 **SQL Server Integration Services**，再单击“属性”。  
  
4.  在 **“SQL Server Integration Services 属性”** 对话框中，可以执行下列操作：  
  
    -   单击“常规”选项卡。若要启用该服务，请选择手动或自动启动类型。 若要禁用该服务，请选择 **“启动类型”** 框中的“禁用”。 选择“禁用”不会停止当前正在运行的服务。  
  
         如果该服务已经启用，则可以单击 **“停止”** 停止该服务，或单击 **“启动”** 启动该服务。  
  
    -   单击 **“登录”** 选项卡可查看或编辑登录信息。  
  
    -   单击 **“恢复”** 选项卡可查看服务失败时计算机的默认反应。 您可以修改这些选项以满足您环境的要求。  
  
    -   单击 **“依赖项”** 选项卡可查看依赖服务的列表。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务没有任何依赖项。  
  
5.  单击“确定”。   
  
6.  另外，如果启动类型为“手动”或“自动”，还可以右键单击 **SQL Server Integration Services**，然后单击“启动”、“停止”或“重新启动”。  
  
7.  在“文件”菜单上，单击“退出”关闭“服务”管理单元。  

## <a name="grant-permissions-to-the-service"></a>向服务授予权限
  在以前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，在您安装了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 后，默认情况下 Users 组中的所有用户都已对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务具有访问权限。 在您安装当前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]时，用户无权访问 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务。 该服务默认是安全的。 在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 后，管理员必须授予对服务的访问权限。  
  
### <a name="to-grant-access-to-the-integration-services-service"></a>授予对 Integration Services 服务的访问权限  
  
1.  运行 Dcomcnfg.exe。 Dcomcnfg.exe 提供用于修改注册表中的某些设置的用户界面。  
  
2.  在“组件服务”对话框中，展开“组件服务 > 计算机 > 我的电脑 > DCOM 配置”节点。  
  
3.  右键单击“Microsoft SQL Server Integration Services 13.0” ，然后单击“属性” 。  
  
4.  在 **“安全性”** 选项卡上，在 **“启动和激活权限”** 区域中单击 **“编辑”** 。  
  
5.  添加用户并分配适当的权限，然后单击“确定”。  
  
6.  对于访问权限重复步骤 4 和 5。  
  
7.  重新启动 SQL Server Management Studio。  
  
8.  重新启动 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务。  

### <a name="event-logged-when-permissions-are-missing"></a>缺少权限时记录的事件

如果 SQL Server 代理的服务帐户没有 Integration Services DCOM [启动和激活权限]，则 SQL Server 代理执行 SSIS 包作业时，会将以下事件添加到系统事件日志中：

```
Log Name: System
Source: **Microsoft-Windows-DistributedCOM**
Date: 1/9/2019 5:42:13 PM
Event ID: **10016**
Task Category: None
Level: Error
Keywords: Classic
User: NT SERVICE\SQLSERVERAGENT
Computer: testmachine
Description:
The application-specific permission settings do not grant Local Activation permission for the COM Server application with CLSID
{xxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
and APPID
{xxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
to the user NT SERVICE\SQLSERVERAGENT SID (S-1-5-80-344959196-2060754871-2302487193-2804545603-1466107430) from address LocalHost (Using LRPC) running in the application container Unavailable SID (Unavailable). This security permission can be modified using the Component Services administrative tool.
```

## <a name="configure-the-service"></a>配置服务
 
安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]时，安装过程会创建并安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的配置文件。 此配置文件包含以下设置：  
  
-   服务停止时将向包发送停止命令。  
  
-   在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 的对象资源管理器中为 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 显示的根文件夹是 MSDB 和“文件系统”文件夹。  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务所管理的文件系统中的包位于 %ProgramFiles%\Microsoft SQL Server\130\DTS\Packages 中。  
  
 此配置文件还指定哪个 msdb 数据库包含将由 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务管理的包。 默认情况下， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务配置为管理 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例的 msdb 数据库中的包，该实例与 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]同时安装。 如果未同时安装 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，则 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务可配置为管理本地默认 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的 msdb 数据库中的包。  
  
### <a name="default-configuration-file-example"></a>默认配置文件示例  
 下面的示例显示了指定以下设置的默认配置文件：  
  
-   当 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务停止时包停止运行。  
  
-   包存储的根文件夹在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中为 MSDB 和“文件系统”。  
  
-   该服务管理存储在本地默认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的 msdb 数据库中的包。  
  
-   该服务管理存储在文件系统的“包”文件夹中的包。  
  
 **默认配置文件示例**  
  
```xml
\<?xml version="1.0" encoding="utf-8"?>  
\<DtsServiceConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <StopExecutingPackagesOnShutdown>true</StopExecutingPackagesOnShutdown>  
  <TopLevelFolders>  
    \<Folder xsi:type="SqlServerFolder">  
      <Name>MSDB</Name>  
      <ServerName>.</ServerName>  
    </Folder>  
    \<Folder xsi:type="FileSystemFolder">  
      <Name>File System</Name>  
      <StorePath>..\Packages</StorePath>  
    </Folder>  
  </TopLevelFolders>    
</DtsServiceConfiguration>  
```  
  
### <a name="modify-the-configuration-file"></a>修改配置文件  
 可以通过修改配置文件来达到以下目的：允许包在服务停止时继续运行；在对象资源管理器中显示其他根文件夹；或者指定文件系统中的一个不同文件夹或其他文件夹由 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务进行管理。 例如，可以创建 **SqlServerFolder** 类型的其他根文件夹来管理其他 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的 msdb 数据库中的包。  
  
> [!NOTE]  
>  某些字符在文件夹名称中无效。 文件夹名称的有效字符由 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 类 **System.IO.Path** 和 **GetInvalidFilenameChars** 字段决定。 **GetInvalidFilenameChars** 字段提供了不能在传递给 **Path** 类成员的路径字符串参数中指定的特定于平台的字符数组。 无效的字符集会因文件系统的不同而不同。 通常，无效字符为引号 (")、小于号 (<) 字符和竖线 (|) 字符。  
  
 但是，您必须修改配置文件，才能管理存储在 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的某个命名实例或远程实例中的包。 如果不更新配置文件，则无法在 **中使用** 对象资源管理器 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 来查看存储在该命名实例或远程实例的 msdb 数据库中的包。 如果尝试使用 **对象资源管理器** 查看这些包，将收到以下错误消息：  
  
 `Failed to retrieve data for this request. (Microsoft.SqlServer.SmoEnum)`  
  
 `The SQL Server specified in Integration Services service configuration is not present or is not available. This might occur when there is no default instance of SQL Server on the computer. For more information, see the topic "Configuring the Integration Services Service" in SQL Server 2008 Books Online.`  
  
 `Login Timeout Expired`  
  
 `An error has occurred while establishing a connection to the server. When connecting to SQL Server 2008, this failure may be caused by the fact that under the default settings SQL Server does not allow remote connections.`  
  
 `Named Pipes Provider: Could not open a connection to SQL Server [2]. (MsDtsSvr).`  
  
 若要修改 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的配置文件，可使用文本编辑器。  
  
> [!IMPORTANT]  
>  修改了服务配置文件后，必须重新启动该服务才能使用更新后的服务配置。  
  
### <a name="modified-configuration-file-example"></a>经过修改的配置文件示例  
 下面的示例显示了经过修改的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]配置文件。 此文件用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 命名实例 `InstanceName` ，该实例在 `ServerName`服务器上。  
  
 **经过修改的 SQL Server 命名实例配置文件的示例**  
  
```xml
\<?xml version="1.0" encoding="utf-8"?>  
\<DtsServiceConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <StopExecutingPackagesOnShutdown>true</StopExecutingPackagesOnShutdown>  
  <TopLevelFolders>  
    \<Folder xsi:type="SqlServerFolder">  
      <Name>MSDB</Name>  
      <ServerName>ServerName\InstanceName</ServerName>  
    </Folder>  
    \<Folder xsi:type="FileSystemFolder">  
      <Name>File System</Name>  
      <StorePath>..\Packages</StorePath>  
    </Folder>  
  </TopLevelFolders>    
</DtsServiceConfiguration>  
```  
  
### <a name="modify-the-configuration-file-location"></a>修改配置文件的位置  
 注册表项 **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS\ServiceConfigFile** 指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务使用的配置文件的位置和名称。 该注册表项的默认值是 **C:\Program Files\Microsoft SQL Server\130\DTS\Binn\MsDtsSrvr.ini.xml**。 可以更新该注册表项的值，以使配置文件使用其他名称和位置。 请注意，路径中的版本号（SQL Server [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)] 为 120、[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 为 130 等等）因 SQL Server 版本而异。
  
> [!CAUTION]  
>  如果注册表编辑不当，可能会导致严重问题并需要重新安装操作系统。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 不能保证可以解决因注册表编辑不当而导致的问题。 编辑注册表之前，请备份所有重要数据。 有关如何备份、还原和编辑注册表的信息，请参阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 知识库文章 [Microsoft Windows 注册表说明](https://support.microsoft.com/kb/256986)。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务在启动时加载配置文件。 对注册表项进行任何更改都需要重新启动服务。  

## <a name="connect-to-the-local-service"></a>连接到本地服务
  在您连接到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务之前，管理员必须授予您该服务的访问权限。 
  
### <a name="to-connect-to-the-integration-services-service"></a>连接到 Integration Services 服务  
  
1.  打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
2.  在 **“视图”** 菜单上，单击 **“对象资源管理器”** 。  
  
3.  在对象资源管理器工具栏上，单击 **“连接”**，然后单击 **“Integration Services”**。  
  
4.  在 **“连接到服务器”** 对话框中，提供服务器名。 可以使用句点 (.)、(local) 或 **localhost** 来指示本地服务器。  
  
5.  单击“连接”。  

## <a name="connect-to-a-remote-ssis-server"></a>连接到远程 SSIS 服务器
  
 若要从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 或其他管理应用程序连接到远程服务器上的 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 实例，应用程序的用户需要拥有服务器上的一组特定权限。  
  
> [!IMPORTANT]
> 若要直接连接到旧 Integration Services 服务的实例，则必须使用与正在运行 Integration Services 服务的 SQL Server 版本保持一致的 SQL Server Management Studio (SSMS) 版本。 例如，要连接到在 SQL Server 2016 的实例上运行的旧 Integration Services 服务，则必须使用 SQL Server 2016 的 SSMS 版本。 [下载 SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)。
>
>  若要管理存储在某远程服务器上的包，您不必连接到该远程服务器上 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的实例。 只需编辑 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的配置文件，以便 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 显示存储在远程服务器上的包。
  
### <a name="connecting-to-integration-services-on-a-remote-server"></a>连接到远程服务器上的 Integration Services  
  
#### <a name="to-connect-to-integration-services-on-a-remote-server"></a>连接到远程服务器上的 Integration Services  
  
1.  打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
2.  选择 **“文件”** 菜单上的 **“连接对象资源管理器”** 以显示 **“连接到服务器”** 对话框。  
  
3.  在 **“服务器类型”** 列表中选择 **Integration Services** 。  
  
4.  在“服务器名称”文本框中键入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的名称。  
  
    > [!NOTE]  
    >  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务是不特定于实例的。 通过使用正运行 Integration Services 服务的计算机的名称连接到该服务。  
  
5.  单击“连接”。  
  
> [!NOTE]  
>  **“查找服务器”** 对话框中不显示 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的远程实例。 此外， **“连接到服务器”** 对话框中的 **“连接选项”** 选项卡上的选项不适用于 **连接（该选项卡在单击** “选项” [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 按钮后显示）。  
  
### <a name="eliminating-the-access-is-denied-error"></a>消除“拒绝访问”错误  
 当没有足够权限的用户尝试连接到远程服务器上的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 实例时，服务器将以“拒绝访问”错误消息进行响应。 确保用户具有所需的 DCOM 权限可避免出现此错误消息。  
  
#### <a name="to-configure-rights-for-remote-users-on-windows-server-2003-or-windows-xp"></a>在 Windows Server 2003 或 Windows XP 上配置远程用户的权限  
  
1.  如果用户不是本地 Administrators 组的成员，请将用户添加至 Distributed COM Users 组。 可以从“管理工具”菜单访问“计算机管理”MMC 管理单元完成此操作。  
  
2.  打开“控制面板”，双击“管理工具”，然后双击“组件服务”以启动组件服务 MMC 管理单元。  
  
3.  展开控制台左侧窗格中的 **“组件服务”** 节点。 展开 **“计算机”** 节点，展开 **“我的电脑”**，然后单击 **“DCOM 配置”** 节点。  
  
4.  选中 **“DCOM 配置”** 节点，然后在可以配置的应用程序列表中选择“SQL Server Integration Services 11.0”。  
  
5.  右键单击 SQL Server Integration Services 11.0，然后选择“属性”。  
  
6.  在 **“SQL Server Integration Services 11.0 属性”** 对话框中，选择 **“安全性”** 选项卡。  
  
7.  在 **“启动和激活权限”** 下，选择 **“自定义”**，然后单击 **“编辑”** 以打开 **“启动权限”** 对话框。  
  
8.  在 **“启动权限”** 对话框中，添加或删除用户，并为适当的用户和组分配相应的权限。 可用的权限为“本地启动”、“远程启动”、“本地激活”和“远程激活”。 启动权限可授予或拒绝启动和停止服务的权限；激活权限可授予或拒绝连接到服务的权限。  
  
9. 单击“确定”关闭对话框。  
  
10. 在 **“访问权限”** 下，重复步骤 7 和步骤 8 以为适当的用户和组分配适当的权限。  
  
11. 关闭 MMC 管理单元。  
  
12. 重新启动 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务。  
  
#### <a name="to-configure-rights-for-remote-users-on-windows-2000-with-the-latest-service-packs"></a>在带有最新 Service Pack 的 Windows 2000 上为远程用户配置权限  
  
1.  在命令提示符下运行 **dcomcnfg.exe** 。  
  
2.  在 **“分布式 COM 配置属性”** 对话框的 **“应用程序”** 页上，选择“SQL Server Integration Services 11.0”，再单击 **“属性”**。  
  
3.  选择 **“安全性”** 页。  
  
4.  使用两个单独的对话框来分别配置 **“访问权限”** 和 **“启动权限”**。 您无法区分远程访问和本地访问 - 访问权限包括了本地访问和远程访问，启动权限也包括了本地启动和远程启动。  
  
5.  关闭对话框和 **dcomcnfg.exe**。  
  
6.  重新启动 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务。  
  
### <a name="connecting-by-using-a-local-account"></a>使用本地帐户连接

如果您是在客户端计算机上使用本地 Windows 帐户工作，则只有远程计算机上存在与客户端计算机上的本地帐户的帐户名和密码相同并且具有相应权限的本地帐户时，才可以连接到远程计算机上的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务。  
  
### <a name="ssis-windows-service-doesnt-support-delegation"></a>SSIS Windows 服务不支持委派

SSIS 不支持委派凭据，有时也称为双跃点。 在这种情况下，你在客户端计算机上工作，SSIS 安装在第二台计算机上，而 SQL Server 安装在第三台计算机上。 虽然 SSMS 可成功地将你的凭据从客户端计算机传递到第二台计算机（SSIS 在其上运行），但是 SSIS 不能将凭据从第二台计算机委派到第三台计算机（SQL Server 在其上运行）。

## <a name="configure-the-firewall"></a>配置防火墙
  
 Windows 防火墙系统可帮助防止通过网络连接对计算机资源进行未经授权的访问。 若要通过此防火墙访问 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ，您必须将该防火墙配置为允许访问。  
  
> [!IMPORTANT]  
>  若要管理存储在某远程服务器上的包，您不必连接到该远程服务器上 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的实例。 只需编辑 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务的配置文件，以便 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 显示存储在远程服务器上的包。
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务使用 DCOM 协议。
  
 有很多可用的防火墙系统。 如果运行的防火墙不是 Windows 防火墙，请参阅防火墙文档，获取特定于所用系统的信息。  
  
 如果防火墙支持应用程序级筛选，则可以使用 Windows 提供的用户界面来指定允许通过该防火墙的例外项，如程序和服务。 否则，必须将 DCOM 配置为使用一组有限的 TCP 端口。 上面提供的 Microsoft 网站链接包含有关如何指定要使用的 TCP 端口的信息。  
  
 Integration Services 服务使用端口 135，该端口不能更改。 只有打开 TCP 端口 135 才能访问服务控制管理器 (SCM)。 SCM 执行以下任务：启动和停止 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务，以及将控制请求传输到运行的服务。  
  
 以下部分中的信息特定于 Windows 防火墙。 可以通过在命令提示符处运行命令或在“Windows 防火墙”对话框中设置属性来配置 Windows 防火墙系统。  
  
 有关默认 Windows 防火墙设置的详细信息以及有关影响数据库引擎、Analysis Services、Reporting Services 和 Integration Services 的 TCP 端口的说明，请参阅 [配置 Windows 防火墙以允许 SQL Server 访问](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)。  
  
### <a name="configuring-a-windows-firewall"></a>配置 Windows 防火墙  
 您可以使用下面的命令来打开 TCP 端口 135、向例外列表中添加 MsDtsSrvr.exe 以及指定防火墙不阻止的范围。  
  
#### <a name="to-configure-a-windows-firewall-using-the-command-prompt-window"></a>使用命令提示符窗口配置 Windows 防火墙  
  
1.  运行以下命令：

    ```dos
    netsh firewall add portopening protocol=TCP port=135 name="RPC (TCP/135)" mode=ENABLE scope=SUBNET
    ```
  
2.  运行以下命令：

    ```dos
    netsh firewall add allowedprogram program="%ProgramFiles%\Microsoft SQL Server\100\DTS\Binn\MsDtsSrvr.exe" name="SSIS Service" scope=SUBNET
    ```
  
    > [!NOTE]  
    >  若要打开所有计算机的防火墙，同时打开 Internet 上的计算机的防火墙，请用 scope=ALL 代替 scope=SUBNET。  
  
 下面的步骤描述了如何使用 Windows 用户界面来打开 TCP 端口 135，向例外列表中添加 MsDtsSrvr.exe 并指定防火墙不阻止的范围。  
  
#### <a name="to-configure-a-firewall-using-the-windows-firewall-dialog-box"></a>使用“Windows 防火墙”对话框配置防火墙  
  
1.  在“控制面板”中，双击“Windows 防火墙”。  
  
2.  在 **“Windows 防火墙”** 对话框中，单击 **“例外”** 选项卡，再单击 **“添加程序”**。  
  
3.  在“添加程序”对话框中单击“浏览”，导航到 Program Files\Microsoft SQL Server\100\DTS\Binn 文件夹，单击 MsDtsSrvr.exe，然后单击“打开”。 单击 **“确定”** 关闭 **“添加程序”** 对话框。  
  
4.  在 **“例外”** 选项卡上，单击 **“添加端口”**。  
  
5.  在“添加端口”对话框中，键入 **RPC(TCP/135)** 或在“名称”框中键入另一个描述性名称，在“端口号”框中键入 **135**，然后选择“TCP”。  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务始终使用端口 135。 您不能指定其他端口。  
  
6.  在 **“添加端口”** 对话框中，可以选择单击 **“更改范围”** 来修改默认的作用范围。  
  
7.  在“更改范围”对话框中，选择“我的网络（仅子网）”或键入自定义列表，然后单击“确定”。  
  
8.  若要关闭 **“添加端口”** 对话框，请单击 **“确定”**。  
  
9. 若要关闭 **“Windows 防火墙”** 对话框，请单击 **“确定”**。  
  
    > [!NOTE]  
    >  为了配置 Windows 防火墙，此过程使用“控制面板”中的“Windows 防火墙”项。 **“Windows 防火墙”** 项仅可为当前网络位置配置文件配置防火墙。 不过，也可使用 **netsh** 命令行工具或名为高级安全 Windows 防火墙的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 管理控制台 (MMC) 管理单元来配置 Windows 防火墙。 有关这些工具的详细信息，请参阅 [配置 Windows 防火墙以允许 SQL Server 访问](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)。  
