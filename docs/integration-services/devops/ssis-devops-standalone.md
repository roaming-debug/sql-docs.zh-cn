---
title: 独立 SQL Server Integration Services (SSIS) DevOps 工具 | Microsoft Docs
description: 了解如何使用独立 SSIS DevOps 工具生成 SSIS CICD。
ms.date: 10/16/2020
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 65712139819c65c2fe8804cc8e0f6768025b322a
ms.sourcegitcommit: e3ab17fe9822629eda4df5fb4550df5d495f3134
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2021
ms.locfileid: "98207856"
---
# <a name="standalone-sql-server-integration-service-ssis-devops-tools-preview"></a>独立 SQL Server Integration Services (SSIS) DevOps 工具（预览）

独立 SSIS DevOps 工具提供一组执行 SSIS CICD 任务的可执行文件。 如果不依赖于 Visual Studio 或 SSIS 运行时的安装，则可以轻松将这些可执行文件与任何 CICD 平台集成。 提供的可执行文件包括：

- SSISBuild.exe：在项目部署模型或包部署模型中生成 SSIS 项目。
- SSISDeploy.exe：将 ISPAC 文件部署到 SSIS 目录，或将 DTSX 文件及其依赖项部署到文件系统。

## <a name="installation"></a>安装

需要 .NET framework 4.6.2 或更高版本。

从[下载中心](https://aka.ms/AA9xp65)下载最新安装程序，然后通过向导或命令行安装：

- 通过向导安装

双击要安装的 .exe 文件，然后指定用于提取可执行文件和依赖项文件的文件夹。

![安装位置](media/installation-location.png)

- 通过命令行安装

```
SSISDevOpsTools.exe /Q /C /T:<full path>
```

![安装命令行](media/installation-command-line.png)


## <a name="ssisbuildexe"></a>SSISBuild.exe

**语法**

```
SSISBuild.exe -project|-p:<dtproj file path> [-configuration|-c:<configuration name>] [-projectPassword|-pp:<project password>] [-stripSensitive|-ss] [-output|-o:<output path>] [-log|-l:<log level>[;<log path>]] [-quiet|-q] [-help|-h|-?]
```

**参数**

|参数|说明|
|---------|---------|
|-project \|-p:\<dtproj file path>|要生成的 dtproj 文件的文件路径。|
|-configuration\|-c:\<configuration name>|要用于生成的项目配置的名称。 如果未提供，则默认为 dtproj 文件中第一个定义的项目配置。|
|-projectPassword\|-pp:\<project password>|SSIS 项目及其包的密码。 仅当 SSIS 项目和包的保护级别为 EncryptSensitiveWithPassword 或 EncryptAllWithPassword 时，此参数才有效。 对于包部署模型，所有包必须共享由此参数指定的相同密码。|
|-stripSensitive\|-ss|将 SSIS 项目的保护级别转换为 DontSaveSensitve。 当保护级别为 EncryptSensitiveWithPassword 或 EncryptAllWithPassword 时，必须正确设置参数 -projectPassword。 此选项仅对项目部署模型有效。|
|-output\|-o:\<output path>|生成项目的输出路径。 此参数的值将覆盖项目配置中的默认输出路径。|
|-log\|-l:\<log level>[;\<log path>]|日志相关设置。 <li>日志级别：只有具有相同或更高日志记录级别的日志将被写入日志文件。 有四个日志记录级别（从低到高）：DIAG、INFO、WRN、ERR。 如果未指定日志记录级别，则默认日志记录级别为 INFO。 <li> 日志路径：要保存日志的文件路径。 如果未指定路径，则不会生成日志文件。|
|-quiet\|-q|不要在标准输出中显示任何日志。|
|-help\|-h\|-?|显示此命令行实用工具的详细使用情况信息。|

**示例**

- 使用第一个定义的项目配置生成 dtproj，而不使用密码进行加密：    
    ```
    SSISBuild.exe -p:"C:\projects\demo\demo.dtproj"
    ```

- 使用配置“DevConfiguration”生成 dtproj，使用密码进行加密，并将生成工件输出到特定文件夹：
    ```
    SSISBuild.exe -p:C:\projects\demo\demo.dtproj -c:DevConfiguration -pp:encryptionpassword -o:D:\folder
    ```

- 使用配置“DevConfiguration”生成 dtproj，使用密码进行加密，将其敏感数据条带化，并记录级别 DIAG：
    ```
    SSISBuild.exe -p:C:\projects\demo\demo.dtproj -c:DevConfiguration -pp:encryptionpassword -ss -l:diag
    ```

## <a name="ssisdeployexe"></a>SSISDeploy.exe

**语法**

```
SSISDeploy.exe -source|-s:<source path> -destination|-d:<type>;<path>[;server] [-authType|-at:<auth type name>] [-connectionStringSuffix|-css:<connection string suffix>] [-projectPassword|-pp:<project password>] [-username|-u:<username>] [-password|-p:<password>] [-log|-l:<log level>[;<log path>]] [-quiet|-q] [-help|-h|-?]
```

**参数**

|参数|说明|
|---------|---------|
|-source\|-s:\<source path>|要部署的项目的本地文件路径。 允许使用 ISPAC、DTSX、DTSX 文件夹路径和 SSISDeploymentManfiest。|
|-destination\|-d:\<type>;\<path>[;server]|目标类型、目标文件夹路径，以及要将源文件部署到的 SSIS 目录的服务器名称。 目前支持以下两种目标类型： <li> CATALOG：将单个或多个 ISPAC 文件部署到指定 SSIS 目录。 CATALOG 目标的路径应采用以下格式： <br> /SSISDB/\<folder name>[/\<project name>] <br> 可选 <project name> 仅在源指定单个 ISPAC 文件路径时有效。 必须为 CATALOG 目标指定服务器名称。 <li> FILE：将单个或多个 SSISDeploymentManifest 文件中指定的 SSIS 包或文件部署到文件系统的指定路径。 文件目标的路径可以是本地文件夹路径，也可以是网络文件夹路径，格式如下： <br>\\\\\<machine name>\\\<folder name>[\\\<sub folder name>\...]|
|-authType\|-at:\<auth type name>|用于访问 SQL Server 的身份验证类型。 对于 CATALOG 目标是必需的。 支持以下类型： <li> WIN：Windows 身份验证 <li> SQL：SQL Server 身份验证 <li> ADPWD：Active Directory - 密码 <li> ADINT：Active Directory - 集成|
|-connectionStringSuffix\|-css:\<connection string suffix> |连接字符串的后缀，用于连接到 SSIS 目录。|
|-projectPassword\|-pp:\<project password> |用于对 ISPAC 或 DTSX 文件进行解密的密码。|
|-username\|-u:\<username>  |用于访问指定 SSIS 目录或文件系统的用户名。 允许用于文件系统访问的域名前缀。|
|-password\|-p:\<password>  |用于访问指定 SSIS 目录或文件系统的密码。|
|-log\|-l:\<log level>[;\<log path>] |用于运行此实用工具的日志相关设置。 <li> 日志级别：只有具有相同或更高日志记录级别的日志将被写入日志文件。 有四个日志记录级别（从低到高）：DIAG、INFO、WRN、ERR。 如果未指定日志记录级别，则默认日志记录级别为 INFO。 <li> 日志路径：要保存日志的文件路径。 如果未指定路径，则不会生成日志文件。|
|-quiet\|-q |不要在标准输出中显示日志。|
|-help\|-h\|-?  |显示此命令行实用工具的详细使用情况信息。|

**示例**

- 使用 Windows 身份验证将不使用密码加密的单个 ISPAC 部署到 SSIS 目录。
    ```
    SSISDeploy.exe -s:D:\myfolder\demo.ispac -d:catalog;/SSISDB/destfolder;myssisserver -at:win
    ```

- 使用 SQL 身份验证将使用密码加密的单个 ISPAC 部署到 SSIS 目录，并重命名项目名称。
    ```
    SSISDeploy.exe -s:D:\myfolder\test.ispac -d:catalog;/SSISDB/folder/testproj;myssisserver -at:sql -u:sqlusername -p:sqlpassword -pp:encryptionpassword
    ```

- 将单个 SSISDeploymentManifest 及其关联文件部署到 Azure 文件共享。
    ```
    SSISDeploy.exe -s:D:\myfolder\mypackage.SSISDeploymentManifest -d:file;\\myssisshare.file.core.windows.net\destfolder -u:Azure\myssisshare -p:storagekey
    ```

- 将 DTSX 文件的文件夹部署到本地文件系统。
    ```
    SSISDeploy.exe -s:D:\myfolder -d:file;\\myssisshare\destfolder
    ```

## <a name="release-notes"></a>发行说明

### <a name="version-012-preview"></a>版本 0.1.2 预览版

发行日期：2021 年 1 月 14 日

- 修复了以下问题：SSIS 项目文件中的包参数元数据与 SSIS 包不匹配时，SSISBuild.exe 无法生成项目且出现 NullReference 异常。
- 修复了以下问题：当已使用 EncryptSensitiveWithUserKey 对包含包的 SSIS 项目进行加密并且该包包含具有敏感数据的 CM 时，尽管已使用 SSISDeploy.exe 将包成功部署到 SSISDB，但包无法执行且出现以“无法解密受保护的 XML 节点”开头的错误。

### <a name="version-011-preview"></a>版本 0.1.1 预览版

发行日期：2020 年 11 月 11 日

- 解决了问题：将 ispac 部署到 SSIS 目录时 SSISDeploy.exe 无法加载程序集。

### <a name="version-010-preview"></a>版本 0.1.0 预览版

发行日期：2020 年 10 月 16 日

独立 SSIS DevOps 工具的初始预览版。

## <a name="next-steps"></a>后续步骤

- 获取[独立 SSIS DevOps 工具](https://aka.ms/AA9xp65)
- 若有任何疑问，请访问 [Q&A](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools&ssr=false#qna)。
