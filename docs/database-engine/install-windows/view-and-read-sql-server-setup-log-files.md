---
title: 查看和读取 SQL Server 安装程序日志文件 | Microsoft Docs
description: 本文将介绍 SQL Server 安装程序创建的日志文件。 日志文件位于带有日期和时间戳的文件夹中。
ms.custom: ''
ms.date: 09/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- viewing logs
- displaying log files
- Setup [SQL Server], logs
- installation log files [SQL Server]
- installing SQL Server, logs
- errors [SQL Server], Setup
- logs [SQL Server], Setup
ms.assetid: 9d77af64-9084-4375-908a-d90f99535062
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 4cf535ee95b91a0e9b6ed3ba2b8745478736a142
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125664"
---
# <a name="view-and-read-sql-server-setup-log-files"></a>查看和阅读 SQL Server 安装程序日志文件

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

默认情况下，SQL Server 安装程序会在 \%programfiles%\Microsoft SQL Server\\nnn\Setup Bootstrap\Log 内带有日期和时间戳的文件夹中创建日志文件，其中 nnn 是与正在安装的 SQL 版本相对应的数字 。 带有时间戳的日志文件夹的名称格式为 YYYYMMDD_hhmmss。 在无人参与模式下执行安装程序时，将在 %temp%\sqlsetup*.log 中创建日志。 日志文件夹中的所有文件将归档到各自日志文件夹的 Log\*.cab 文件中。  

   | 文件           | 路径 |
   | :------        | :----------------------------- |
   | **Summary.txt**    | \%programfiles%\Microsoft SQL Server\\nnn\Setup Bootstrap\Log |
   | Summary_\<MachineName>\_Date.txt  | \%programfiles%\Microsoft SQL Server\\nnn\Setup Bootstrap\Log\YYYYMMDD_hhmmss |
   | **Detail.txt** | \%programfiles%\Microsoft SQL Server\\nnn\Setup Bootstrap\Log\YYYYMMDD_hhmmss|
   | **数据存储** | \%programfiles%\Microsoft SQL Server\\nnn\Setup Bootstrap\Log\YYYYMMDD_hhmmss\Datastore
   | **MSI 日志文件** | \%programfiles%\Microsoft SQL Server\\nnn\Setup Bootstrap\Log\YYYYMMDD_hhmmss\\\<Name>.log|
   | **ConfigurationFile.ini** | \%programfiles%\Microsoft SQL Server\\nnn\Setup Bootstrap\Log\YYYYMMDD_hhmmss |
   | **SystemConfigurationCheck_Report.htm** | \%programfiles%\Microsoft SQL Server\\nnn\Setup Bootstrap\Log\YYYYMMDD_hhmmss |
   | **对于无人参与的安装** | %temp%\sqlsetup*.log |


 ![显示在 Setup Bootstrap 文件夹中的哪个位置查找 ConfigurationFiles.ini 文件的屏幕截图。](media/view-and-read-sql-server-setup-log-files/setup-bootstrap-example.png)

 >[!NOTE]
 > 路径 nnn 中的数字对应要安装的 SQL 版本。 上图中安装的是 SQL 2017，因此该文件夹为 140。 SQL 2016 对应的文件夹为 130，SQL 2014 对应的文件夹为 120。
  
 SQL Server 安装程序分以下三个基本阶段完成： 
  
1.  全局规则验证：验证基本系统要求
2.  组件更新：检查正在安装的媒体是否有可用的更新
3.  用户请求的操作：允许用户选择和自定义功能
  

此工作流会生成单个摘要日志，并且对于基本 SQL Server 安装，单个详细日志将与基本安装一起安装，或者对于更新（例如服务包）时，则有两个详细日志将与基本安装一起安装。 
  
此外，还有数据存储文件包含安装过程正在跟踪的所有配置对象状态的快照，并且可用于故障排除配置错误。 系统会为每个执行阶段创建 XML 转储文件，并将其保存在带时间戳的日志文件夹下的数据存储日志子文件夹中。 

以下部分介绍 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序日志文件。  
  
## <a name="summarytxt-file"></a>Summary.txt 文件 
  
### <a name="overview"></a>概述  
 此文件显示在安装过程中检测到的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件、操作系统环境、命令行参数值（如果已指定），以及执行的每个 MSI/MSP 的总体状态。
  
 本日志归纳为以下部分：
  
-   执行的总体摘要  
-   运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的计算机的属性和配置  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品功能  
-   安装版本和安装包属性的说明
-   安装过程中提供的运行时输入设置  
-   配置文件的位置  
-   执行结果的详细信息  
-   全局规则  
-   特定于安装方案的规则  
-   失败的规则  
-   规则报表文件的位置


  >[!NOTE]
  > 请注意，修补时可能会存在多个子文件夹（一个用于每个要修补的实例，一个用于共享功能），其中包含一组类似的文件（即 %programfiles%\MicrosoftSQL Server\130\Setup Bootstrap\Log\<YYYYMMDD_HHMM>\MSSQLSERVER）。 
  
### <a name="location"></a>位置  
 Summary.txt 位于 %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\nnn\Setup Bootstrap\Log\\。
  
 若要找到摘要文本文件中的错误，请使用“error”或“failed”关键字搜索该文件。
  
## <a name="summary_machinename_yyyymmdd_hhmmsstxt-file"></a>Summary_\<MachineName>_YYYYMMDD_HHMMss.txt 文件
  
### <a name="overview"></a>概述  
 summary_engine 基本文件类似于摘要文件，是在主工作流中生成的。
  
### <a name="location"></a>位置  
 Summary_\<MachineName>_YYYYMMDD_HHMMss.txt 文件位于 %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\nnn\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\\。
  
  
## <a name="detailtxt-file"></a>Detail.txt 文件
  
### <a name="overview"></a>概述
 Detail.txt 是针对主工作流（如安装或升级）生成的，它提供有关执行的详细信息。 文件中的日志根据调用安装的每个操作的时间生成。 文本文件显示操作执行的顺序及其依赖项。  
  
### <a name="location"></a>位置  
 Detail.txt 文件位于 %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\nnn\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\Detail.txt。  
  
 如果在安装过程中发生错误，系统会将异常或错误记录在该文件的末尾。 若要查找该文件中的错误，请首先检查文件末尾，然后在文件中搜索“error”或“exception”关键字
    
## <a name="msi-log-files"></a>MSI 日志文件
  
### <a name="overview"></a>概述  
 MSI 日志文件提供安装包进程的详细信息。 它们是在安装指定的包的过程中由 MSIEXEC 生成的。  
  
 MSI 日志文件的类型：
  
-   \<Feature>_\<Architecture>\_\<Interaction>.log   
-   \<Feature>_\<Architecture>\_\<Language>\_\<Interaction>.log   
-   \<Feature>_\<Architecture>\_\<Interaction>\_\<workflow>.log  
  
### <a name="location"></a>位置  
 MSI 日志文件位于 %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\nnn\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\\<Name\>.log。  
  
 该文件的末尾是有关执行的摘要，其中包括成功状态或失败状态以及属性。 若要查找 MSI 文件中的错误，请搜索“value 3”并查看前后文本。  
  
## <a name="configurationfileini-file"></a>ConfigurationFile.ini 文件
  
### <a name="overview"></a>概述  
 本配置文件包含安装过程中提供的输入设置。 该文件可用于在无需手动输入设置的情况下重新启动安装。 但是，帐户的密码、PID 和某些参数不保存在该配置文件中。 可以将这些设置添加到该文件中，也可通过使用命令行或安装程序用户界面提供这些设置。 有关详细信息，请参阅 [使用配置文件安装 SQL Server 2016](./install-sql-server-using-a-configuration-file.md)。  
  
### <a name="location"></a>位置  
 ConfigurationFile.ini 位于 %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\nnn\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\\。  
  
## <a name="systemconfigurationcheck_reporthtm-file"></a>SystemConfigurationCheck_Report.htm 文件
  
### <a name="overview"></a>概述  
 系统配置检查报表包含有关每个执行规则的简短说明，以及执行状态。
  
### <a name="location"></a>位置  
SystemConfigurationCheck_Report.htm 位于 %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\nnn\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\\。

[!INCLUDE[get-help-options](../../includes/paragraph-content/get-help-options.md)]
  
## <a name="see-also"></a>另请参阅  
 [安装 SQL Server 2017](../../database-engine/install-windows/install-sql-server.md)