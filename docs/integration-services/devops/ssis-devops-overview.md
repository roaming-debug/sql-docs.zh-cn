---
title: SQL Server Integration Services DevOps 概述 | Microsoft Docs
description: 了解如何使用 SSIS DevOps 工具生成 SSIS CICD。
ms.date: 12/06/2019
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 597792aa200edf6dcb9cfe49c95ab5e1befa0c55
ms.sourcegitcommit: 5f3e0eca9840db20038f0362e5d88a84ff3424af
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92343649"
---
# <a name="sql-server-integration-services-ssis-devops-tools-azure-devops-extension"></a>SQL Server Integration Services (SSIS) DevOps 工具 Azure DevOps 扩展

可从 Azure DevOps 市场获取 [SSIS DevOps 工具](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools)扩展。

如果你没有 Azure DevOps 组织，首先请注册 [Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops)，然后根据[这些步骤](/azure/devops/marketplace/overview?tabs=browser&view=azure-devops#add-an-extension)添加 SSIS DevOps 工具扩展 。

SSIS DevOps 工具包括 SSIS 生成任务、SSIS 部署发布任务和 SSIS 目录配置任务   。

- [SSIS 生成](#ssis-build-task)任务支持在项目部署模型或包部署模型中生成 dtproj 文件。

- [SSIS 部署](#ssis-deploy-task)任务支持将单个或多个 ispac 文件部署到本地 SSIS 目录，并将 Azure-SSIS IR 或 SSISDeploymentManifest 文件及其关联文件部署到本地或 Azure 文件共享。

- [SSIS 目录配置](#ssis-catalog-configuration-task)任务支持使用采用 JSON 格式的配置文件配置 SSIS 目录的文件夹/项目/环境。 此任务支持以下方案：
    - Folder
        - 创建文件夹。
        - 更新文件夹说明。
    - Project
        - 配置参数的值，同时支持文本值和引用值。
        - 添加环境引用。
    - 环境
        - 创建环境。
        - 更新环境说明。
        - 创建或更新环境变量。

## <a name="ssis-build-task"></a>SSIS 生成任务

![生成任务](media/ssis-build-task.png)

### <a name="properties"></a>属性

#### <a name="project-path"></a>项目路径

要生成的项目文件夹或文件的路径。 如果指定了文件夹路径，则 SSIS 生成任务将以递归方式搜索此文件夹下的所有 dtproj 文件并生成所有文件。

项目路径不能为空，设置为 . 以便从存储库的根文件夹中生成。

#### <a name="project-configuration"></a>项目配置

要用于生成的项目配置的名称。 如果未提供，则默认为每个 dtproj 文件中第一个的定义项目配置。

#### <a name="output-path"></a>输出路径

用于保存生成结果的单独文件夹的路径，这些结果可通过[发布生成项目任务](/azure/devops/pipelines/tasks/utility/publish-build-artifacts?view=azure-devops)发布为生成项目。

### <a name="limitations-and-known-issues"></a>限制和已知问题

- SSIS 生成任务依赖于 Visual Studio 和 SSIS 设计器，这对于生成代理是必需的。 因此，要在管道中运行 SSIS 生成任务，必须为 Microsoft 托管代理选择 vs2017-win2016，或在自托管代理上安装 Visual Studio 和 SSIS 设计器（VS2017 + SSDT2017 或 VS2019 + SSIS 项目扩展）。

- 要使用任何现成组件（包括 SSIS Azure 功能包和其他第三方组件）生成 SSIS 项目，必须在运行管道代理的计算机上安装这些现成组件。  对于 Microsoft 托管代理，用户可以添加 [PowerShell 脚本任务](/azure/devops/pipelines/tasks/utility/powershell?view=azure-devops)或[命令行脚本任务](/azure/devops/pipelines/tasks/utility/command-line?view=azure-devops)，以便在执行 SSIS 生成任务之前下载和安装组件。 下面是用于安装 Azure 功能包的示例 PowerShell 脚本： 

```powershell
wget -Uri https://download.microsoft.com/download/E/E/0/EE0CB6A0-4105-466D-A7CA-5E39FA9AB128/SsisAzureFeaturePack_2017_x86.msi -OutFile AFP.msi

start -Wait -FilePath msiexec -Args "/i AFP.msi /quiet /l* log.txt"

cat log.txt
```

- SSIS 生成任务不支持 EncryptSensitiveWithPassword 和 EncryptAllWithPassword 保护级别 。 确保基本代码中的所有 SSIS 项目都不使用这两个保护级别，否则，SSIS 生成任务在执行过程中将停止响应和超时。

## <a name="ssis-deploy-task"></a>SSIS 部署任务

![部署任务](media/ssis-deploy-task.png)

### <a name="properties"></a>属性

#### <a name="source-path"></a>源路径

要部署的源 ISPAC 或 SSISDeploymentManifest 文件的路径。 此路径可以是文件夹路径或文件路径。

#### <a name="destination-type"></a>目标类型

目标类型。 当前的 SSIS 部署任务支持两种类型：

- 文件系统：将 SSISDeploymentManifest 文件及其关联文件部署到指定的文件系统。 支持本地和 Azure 文件共享。
- SSISDB：将 ISPAC 文件部署到指定的 SSIS 目录，该目录可托管在本地 SQL Server 或 Azure-SSIS Integration Runtime 上。

#### <a name="destination-server"></a>目标服务器

目标 SQL 服务器的名称。 它可以是本地 SQL Server、Azure SQL 数据库或 Azure SQL 托管实例的名称。 仅当目标类型为 SSISDB 时，此属性才可见。

#### <a name="destination-path"></a>目标路径

要部署源文件的目标文件夹的路径。 例如：

- /SSISDB/\<folderName\>
- \\\\\<machineName\>\\\<shareFolderName\>\\\<optionalSubfolderName\>

如果文件夹和子文件夹不存在，SSIS 部署任务会进行创建。

#### <a name="authentication-type"></a>身份验证类型

用于访问指定目标服务器的身份验证类型。 仅当目标类型为 SSISDB 时，此属性才可见。 通常支持以下身份验证类型：

- Windows 身份验证
- SQL Server 身份验证
- Active Directory - 密码
- Active Directory - 集成

但特定身份验证类型是否受支持取决于目标服务器类型和代理类型。 下表列出了详细的支持矩阵。

|目标服务器类型|Microsoft 托管代理|自托管代码|
|---------|---------|---------|
|本地 SQL Server 或 VM |空值|Windows 身份验证|
|Azure SQL|SQL Server 身份验证 <br> Active Directory - 密码|SQL Server 身份验证 <br> Active Directory - 密码 <br> Active Directory - 集成|

#### <a name="domain-name"></a>域名

用于访问指定文件系统的域名。 仅当目标类型为文件系统时，此属性才可见。
如果已向运行自托管代理的用户帐户授予指定目标路径的读写权限，则可以将其留空。

#### <a name="username"></a>用户名

用于访问指定文件系统或 SSISDB 的用户名。 当目标类型为文件系统或身份验证类型为“SQL Server 身份验证”或“Active Directory - 密码”时，此属性可见。
如果目标类型为文件系统，并且已向运行自托管代理的用户帐户授予指定目标路径的读写权限，则可以将其留空。

#### <a name="password"></a>密码

用于访问指定文件系统或 SSISDB 的密码。 当目标类型为文件系统或身份验证类型为“SQL Server 身份验证”或“Active Directory - 密码”时，此属性可见。
如果目标类型为文件系统，并且已向运行自托管代理的用户帐户授予指定目标路径的读写权限，则可以将其留空。

#### <a name="overwrite-existing-projects-or-ssisdeploymentmanifest-files-of-the-same-names"></a>覆盖名称相同的现有项目或 SSISDeploymentManifest 文件

指定是否覆盖名称相同的现有项目或 SSISDeploymentManifest 文件。 如果选择“否”，SSIS 部署任务将跳过部署这些项目或文件。

#### <a name="continue-deployment-when-error-occurs"></a>发生错误时继续部署

指定是否在发生错误时继续部署剩余的项目或文件。 如果选择“否”，在发生错误时，SSIS 部署任务将立即停止。

### <a name="limitations-and-known-issues"></a>限制和已知问题

SSIS 部署任务当前不支持以下方案：

- 在 SSIS 目录中配置环境。
- 将 ispac 部署到只允许多重身份验证 (MFA) 的 Azure SQL Server 或 Azure SQL 托管实例。
- 将包部署到 MSDB 或 SSIS 包存储。

## <a name="ssis-catalog-configuration-task"></a>SSIS 目录配置任务

![目录配置任务](media/ssis-catalog-configuation-task.png)

### <a name="properties"></a>属性

#### <a name="configuration-file-source"></a>配置文件源

SSIS 目录配置 JSON 文件的源。 源可以是“文件路径”，也可以是“内联”。

有关如何[定义配置 JSON](#define-configuration-json) 的详细信息，请参阅：

- 请参阅[内联配置 JSON 示例](#a-sample-inline-configuration-json)。
- 请参阅 [JSON 架构](#json-schema)。

#### <a name="configuration-json-file-path"></a>配置 JSON 文件路径

SSIS 目录配置 JSON 文件的路径。 只有在选择“文件路径”作为配置文件源时，此属性才可见。

若要在配置 JSON 文件中使用[管道变量](/azure/devops/pipelines/process/variables)，你需要在此任务之前添加[文件转换任务](/azure/devops/pipelines/tasks/utility/file-transform?view=azure-devops)，以将配置值替换为管道变量。 有关详细信息，请参阅 [JSON 变量替换](/azure/devops/pipelines/tasks/transforms-variable-substitution?tabs=Classic&view=azure-devops#json-variable-substitution)。

#### <a name="inline-configuration-json"></a>内联配置 JSON

SSIS 目录配置的内联 JSON。 只有在选择“内联”作为配置文件源时，此属性才可见。 可以直接使用管道变量。

#### <a name="roll-back-configuration-when-error-occurs"></a>发生错误时回退配置

在发生错误时是否回退此任务所进行的配置。

#### <a name="target-server"></a>目标服务器

目标 SQL Server 的名称。 它可以是本地 SQL Server、Azure SQL 数据库或 Azure SQL 托管实例的名称。

#### <a name="authentication-type"></a>身份验证类型

用于访问指定目标服务器的身份验证类型。 通常支持以下身份验证类型：

- Windows 身份验证
- SQL Server 身份验证
- Active Directory - 密码
- Active Directory - 集成

但特定身份验证类型是否受支持取决于目标服务器类型和代理类型。 下表列出了详细的支持矩阵。

|目标服务器类型|Microsoft 托管代理|自托管代码|
|---------|---------|---------|
|本地 SQL Server 或 VM |空值|Windows 身份验证|
|Azure SQL|SQL Server 身份验证 <br> Active Directory - 密码|SQL Server 身份验证 <br> Active Directory - 密码 <br> Active Directory - 集成|

#### <a name="username"></a>用户名

用于访问目标 SQL Server 的用户名。 只有在身份验证类型为“SQL Server 身份验证”或“Active Directory - 密码”时，此属性才可见。

#### <a name="password"></a>密码

用于访问目标 SQL Server 的密码。 只有在身份验证类型为“SQL Server 身份验证”或“Active Directory - 密码”时，此属性才可见。

### <a name="define-configuration-json"></a>定义配置 JSON

配置 JSON 架构有三层：

- 目录
- 文件夹
- 项目和环境

![目录配置架构](media/catalog-configuration-schema.png)

#### <a name="a-sample-inline-configuration-json"></a>内联配置 JSON 示例

```json
{
  "folders": [
    {
      "name": "devopsdemo",
      "description": "devops demo folder",
      "projects": [
        {
          "name": "catalog devops",
          "parameters": [
            {
              "name": "password",
              "container": "Package.dtsx",
              "value": "passwd",
              "valueType": "referenced"
            },
            {
              "name": "serverName",
              "container": "catalog devops",
              "value": "localhost",
              "valueType": "literal"
            }
          ],
          "references": [
            {
              "environmentName": "test",
              "environmentFolder": "devopsdemo"
            },
            {
              "environmentName": "test",
              "environmentFolder": "."
            }
          ]
        }
      ],
      "environments": [
        {
          "name": "test",
          "description": "test",
          "variables": [
            {
              "name": "passwd",
              "type": "string",
              "description": "",
              "value": "$(SSISDBServerAdminPassword)",
              "sensitive": true
            },
            {
              "name": "serverName",
              "type": "string",
              "description": "",
              "value": "$(TargetServerName)",
              "sensitive": false
            }
          ]
        }
      ]
    }
  ]
}
```

#### <a name="json-schema"></a>JSON 架构

##### <a name="catalog-attributes"></a>目录属性

|properties  |说明  |说明  |
|---------|---------|---------|
|文件夹  |文件夹对象的数组。 每个对象都包含目录文件夹的配置信息。|有关文件夹对象的架构，请参阅“文件夹属性”。|

##### <a name="folder-attributes"></a>文件夹属性

|Property  |说明  |说明  |
|---------|---------|---------|
|name  |目录文件夹的名称。|如果文件夹不存在，将创建一个文件夹。|
|description|目录文件夹的说明。|将跳过 NULL 值。|
|projects|项目对象的数组。 每个对象都包含项目的配置信息。|有关项目对象的架构，请参阅“项目属性”。|
|environments|环境对象的数组。 每个对象都包含环境的配置信息。|有关环境对象的架构，请参阅“环境属性”。|

##### <a name="project-attributes"></a>项目属性

|Property  |说明  |说明  |
|---------|---------|---------|
|name|项目名称。 |如果父文件夹中不存在项目，则将跳过项目对象。|
|parameters|参数对象的数组。 每个对象都包含参数的配置信息。|有关参数对象的架构，请参阅“参数属性”。|
|引用|引用对象的数组。 每个对象都代表对目标项目的环境引用。|有关引用对象的架构，请参阅“引用属性”。|

##### <a name="parameter-attributes"></a>参数属性

|Property  |说明  |说明  |
|---------|---------|---------|
|name|参数的名称。|<li>参数可以是项目参数，也可以是包参数。 <li>如果没有参数，则跳过它。 <li>如果参数是连接管理器属性，则名称格式应为 CM.\<Connection Manager Name>.\<Property Name>。 |
|容器 (container)|参数的容器。|<li>如果参数是项目参数，则容器应为项目名称。 <li>如果它是包参数，则容器应为扩展名为 .dtsx 的包名称。|
|值|参数值。|<li>当 valueType 是引用对象时 ：值是对字符串类型中的环境变量的引用。 <li> 当 valueType 是文本时 ：此属性支持任何有效的布尔、数字和字符串 JSON 值  。 <li> 该值将转换为目标参数类型。 如果无法转换，则会发生错误。<li> Null 值无效。 任务将跳过此参数对象，并发出警告。|
|valueType|参数值的类型。|有效类型包括： <br> 文本：value 属性表示文本值。 <br> 引用对象：value 属性表示对环境变量的引用。|

##### <a name="reference-attributes"></a>引用属性

|Property  |说明  |说明  |
|---------|---------|---------|
|environmentFolder|环境的文件夹名称。|如果文件夹不存在，将创建一个文件夹。 <br> 值可以是“.”，它表示引用环境的项目的父文件夹。|
|environmentName|引用的环境的名称。|如果环境不存在，将创建指定的环境。|

##### <a name="environment-attributes"></a>环境属性

|Property  |说明  |说明  |
|---------|---------|---------|
|name|环境的名称。|如果环境不存在，将创建环境。|
|description|环境的说明。|将跳过 NULL 值。|
|variables|变量对象的数组。|每个对象都包含环境变量的配置信息。有关变量对象的架构，请参阅“变量属性”。|

##### <a name="variable-attributes"></a>变量属性

|properties  |说明  |说明  |
|---------|---------|---------|
|name|环境变量的名称。|如果环境变量不存在，将创建环境变量。|
|type|环境变量的数据类型。|有效类型包括： <br> *boolean* <br> byte <br> *datetime* <br> Decimal <br> *double* <br> int16 <br> int32 <br> int64 <br> sbyte <br> single <br> *string* <br> uint32 <br> uint64|
|description|环境变量的说明。|将跳过 NULL 值。|
|值|环境变量的值。|此属性支持任何有效的布尔、数字和字符串 JSON 值。<br> 该值将转换为 type 属性指定的类型。 如果转换失败，则会发生错误。<br>Null 值无效。 任务将跳过此环境变量对象，并发出警告。|
|sensitive|环境变量的值是否是敏感值。|有效输入为： <br> true <br> *false*|

## <a name="release-notes"></a>发行说明

### <a name="version-103"></a>版本 1.0.3

发行日期：2020 年 10 月 21 日

- 允许为 SSIS 部署任务和 SSIS 目录配置任务指定连接字符串后缀。

### <a name="version-102"></a>版本 1.0.2

发行日期：2020 年 5 月 26 日

- 修复了配置工作完成后 SSIS 目录配置任务在某些情况下可能失败的问题。

### <a name="version-101"></a>版本 1.0.1

发行日期：2020 年 5 月 9 日

- 解决了即使仅将单个 dtproj 文件指定为项目路径，SSIS 生成任务也始终生成整个解决方案的问题。

### <a name="version-100"></a>版本 1.0.0

发行日期：2020 年 5 月 8 日

- 正式发布 (GA) 版。
- 添加了对 .NET Framework 最低代理版本的限制。 目前 .NET Framework 的最低版本是 4.6.2。
- 优化了 SSIS 生成任务和 SSIS 部署任务的说明。

### <a name="version-020-preview"></a>版本 0.2.0 预览版

发行日期：2020 年 3 月 31 日

- 添加 SSIS 目录配置任务。

### <a name="version-013-preview"></a>版本 0.1.3 预览版

发行日期：2020 年 1 月 19 日

- 修复了一个问题，该问题会在 ispac 的原始文件名发生更改时阻止部署。

### <a name="version-012-preview"></a>版本 0.1.2 预览版

发行日期：2020 年 1 月 13 日

- 目标类型为 SSISDB 时，在 SSIS 部署任务日志中添加了更详细的异常信息。
- 修正了 SSIS 部署任务的属性目标路径帮助文本中的示例目标路径。

### <a name="version-011-preview"></a>版本 0.1.1 预览版

发行日期：2020 年 1 月 6 日

- 添加了对最低代理版本要求的限制。 目前，此产品的最低代理版本是 2.144.0。
- 修正了 SSIS 部署任务的一些不正确的显示文本。
- 完善了一些错误消息。

### <a name="version-010-preview"></a>版本 0.1.0 预览版

发行日期：2019 年 12 月 5 日

SSIS DevOps 工具的初始版本。 这是预览版。

## <a name="next-steps"></a>后续步骤

- 获取 [SSIS DevOps 扩展](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools)
