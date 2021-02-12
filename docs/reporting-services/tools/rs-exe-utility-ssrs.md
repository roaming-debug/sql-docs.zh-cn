---
title: RS.exe 实用工具 | Microsoft Docs
description: 了解处理用于自动执行报表服务器部署和管理任务的脚本的 rs.exe 实用工具的命令和语法。
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- automatic report server tasks
- rs utility
- command prompt utilities [Reporting Services]
- report servers [Reporting Services], automating tasks
- command prompt utilities [SQL Server], rs
- scripts [Reporting Services], command prompt
- deploying reports [Reporting Services]
ms.assetid: bd6f958f-cce6-4e79-8a0f-9475da2919ce
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8ca10d79178887fa4ed2f9f59c6da7e8c75057f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077928"
---
# <a name="rsexe-utility-ssrs"></a>RS.exe 实用工具 (SSRS)
  rs.exe 实用工具处理您在输入文件中提供的脚本。 使用此实用工具，可以实现报表服务器部署与管理任务的自动化。  
  
> [!NOTE]  
>  从 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]开始，配置为 SharePoint 集成模式的报表服务器以及配置为本机模式的服务器均支持 **rs** 实用工具。 以前的版本仅支持本机模式配置。  
  
## <a name="syntax"></a>语法  
  
```  
  
rs {-?}  
{-i input_file=}  
{-s serverURL}  
{-u username}  
{-p password}  
{-e endpoint}  
{-l time_out}  
{-b batchmode}  
{-v globalvars=}  
{-t trace}  
```  
  
##  <a name="file-location"></a><a name="bkmk_filelocation"></a> 文件位置  
 **RS.exe** 位于 **\Program Files\Microsoft SQL Server\110\Tools\Binn**。 可以在文件系统的任何文件夹中运行此实用工具。  
  
##  <a name="arguments"></a><a name="bkmk_arguments"></a> 参数  
 **-?**  
 （可选）显示 **rs** 参数的语法。  
  
 **-i** input_file  
 （必需）指定要执行的 .rss 文件。 此值可以是指向 .rss 文件的相对路径或完全限定路径。  
  
 **-s** serverURL  
 （必需）指定执行文件的 Web 服务器的名称和报表服务器的虚拟目录名。 以下是报表服务器 URL 的一个示例： `https://examplewebserver/reportserver`。 服务器名称开头处的前缀 http:// 或 https:// 是可选的。 如果省略前缀，报表服务器脚本主机将先尝试使用 https，并在 https 无效时使用 http。  
  
 -u [domain\\]username  
 （可选）指定用于连接到报表服务器的用户帐户。 如果省略 **-u** 和 **-p** ，则使用当前的 Windows 用户帐户。  
  
 **-p** password  
 （指定了 **-u** 时为必需）指定与 **-u** 参数一起使用的密码。 此值区分大小写。  
  
 **-e**  
 （可选）指定应对其运行脚本的 SOAP 端点。 有效值如下：  
  
-   Mgmt2010  
  
-   Mgmt2006  
  
-   Mgmt2005  
  
-   Exec2005  
  
 如果未指定值，则使用 Mgmt2005 端点。 有关 SOAP 端点的详细信息，请参阅 [Report Server Web Service Endpoints](../../reporting-services/report-server-web-service/methods/report-server-web-service-endpoints.md)。  
  
 **-l** time_out  
 （可选）指定与服务器的连接超时之前等待的时间，以秒为单位。默认值为 60 秒。 如果未指定超时值，则使用默认值。 **0** 值指定连接从不超时。  
  
 **-b**  
 （可选）指定脚本文件中的命令以批处理方式运行。 如有任何命令失败，则回滚批处理。 某些命令无法以批处理方式运行，这些命令将按常规方式运行。 仅当脚本中产生异常并且未在脚本中得到处理时，才会导致回滚。 如果脚本处理了异常，并从 **Main** 正常返回，则将提交批处理。 如果省略此参数，则命令将不以批处理方式运行。 有关详细信息，请参阅 [Batching Methods](../../reporting-services/report-server-web-service-net-framework-soap-headers/batching-methods.md)。  
  
 **-v** globalvar  
 （可选）指定脚本中使用的全局变量。 如果脚本使用全局变量，则必须指定此参数。 指定的值必须对 .rss 文件中定义的全局变量有效。 必须为每个 –v 参数指定一个全局变量。  
  
 **-v** 参数在命令行上指定，可用来为运行时在脚本中定义的全局变量设置值。 例如，如果脚本中包含一个名为 *parentFolder* 的变量，则可以在命令行上为该文件夹指定一个名称：  
  
 `rs.exe -i myScriptFile.rss -s https://myServer/reportserver -v parentFolder="Financial Reports"`  
  
 全局变量以给定的名称命名，并设置为提供的值。 例如， **-v a=**"**1**" **-v b=**"**2**" 将生成一个名为 **a** 且值为 "**1**" 的变量，以及一个值为 " **2** " 的变量 **b**。  
  
 全局变量可用于脚本中的所有函数。 反斜杠与英文引号连用 ( **\\"** ) 将解释为一个英文双引号。 仅当字符串中包含空格时才需要使用英文引号。 变量名必须对 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]有效；变量名必须以字母字符或下划线字符开头，且必须包含字母字符、数字或下划线字符。 不能将保留字用作变量名。 有关使用全局变量的详细信息，请参阅[表达式中的内置集合（报表生成器和 SSRS）](../../reporting-services/report-design/built-in-collections-in-expressions-report-builder.md)。  
  
 **-t**  
 （可选）将错误信息输出到跟踪日志中。 此参数不带值。 有关详细信息，请参阅 [Report Server Service Trace Log](../../reporting-services/report-server/report-server-service-trace-log.md)。  
  
##  <a name="permissions"></a><a name="bkmk_permissions"></a> 权限  
 若要运行该工具，必须拥有与运行脚本的报表服务器实例连接的权限。 可以运行脚本来更改本地计算机或远程计算机。 若要更改远程计算机上安装的报表服务器，请在 **-s** 参数中指定远程计算机。  
  
##  <a name="examples"></a><a name="bkmk_examples"></a> 示例  
 下面的示例说明如何指定包含 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET 脚本的脚本文件以及要执行的 Web 服务方法。  
  
```  
rs -i c:\scriptfiles\script_copycontent.rss -s https://localhost/reportserver  
```  
  
 有关详细示例，请参阅 [用于在报表服务器之间复制内容的示例 Reporting Services rs.exe 脚本](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md)。  
  
 有关其他示例，请参阅 [运行 Reporting Services 脚本文件](../../reporting-services/tools/run-a-reporting-services-script-file.md)  
  
## <a name="remarks"></a>备注  
 可以定义脚本来设置系统属性，发布报表，等等。 所创建的脚本可以包含 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] API 的任何方法。 有关可以使用的方法和属性的详细信息，请参阅 [Report Server Web Service](../../reporting-services/report-server-web-service/report-server-web-service.md)。  
  
 必须用 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET 代码编写脚本，并存储在文件扩展名为 .rss 的 Unicode 或 UTF-8 文本文件中。 不能使用 **rs** 实用工具调试脚本。 若要调试脚本，请在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 中运行代码。  
  
> [!TIP]  
>  有关详细示例，请参阅 [用于在报表服务器之间复制内容的示例 Reporting Services rs.exe 脚本](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md)。  
  
## <a name="see-also"></a>另请参阅  
- [运行 Reporting Services 脚本文件](../../reporting-services/tools/run-a-reporting-services-script-file.md)   
- [为部署和管理任务编写脚本](../../reporting-services/tools/script-deployment-and-administrative-tasks.md)   
- [使用 rs.exe 实用工具和 Web 服务编写脚本](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md)   
- [报表服务器命令提示实用工具 (SSRS)](../../reporting-services/tools/report-server-command-prompt-utilities-ssrs.md)  
  
  
