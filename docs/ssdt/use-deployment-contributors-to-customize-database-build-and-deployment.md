---
title: 使用部署参与者自定义数据库部署
description: 了解如何修改数据库项目的行为。 查看生成和部署参与者的资源，并了解使用它们的示例方案。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: fe2064bb-e01e-4a12-9f12-a99aa9a5203f
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 8c512a39da9a17a15e989816234a18266b5c2dfc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066842"
---
# <a name="customize-database-build-and-deployment-by-using-build-and-deployment-contributors"></a>使用生成参与者和部署参与者来自定义数据库生成和部署

Visual Studio 提供了可用于为数据库项目修改生成和部署操作的行为的扩展点。  
  
## <a name="available-extensibility-points"></a>可用的扩展点  
可以为扩展点创建扩展，如下表所示：  
  
|**Action**|**参与者类型**|**说明**|  
|--------------|------------------------|-------------|  
|构建|BuildContributor|在验证完项目模型后生成 SQL 项目时将执行此类扩展。 生成参与者不仅可以访问已完成的模型，还可以访问生成任务的所有属性以及任何自定义参数。|  
|部署|DeploymentPlanModifier|在生成部署计划后并在执行部署计划前将 SQL 项目作为部署管道的一部分进行部署时，将执行此类扩展。 可以使用 DeploymentPlanModifier 通过添加或删除步骤来修改部署计划。 部署参与者可以访问部署计划、比较结果以及源模型和目标模型。|  
|部署|DeploymentPlanExecutor|在执行部署计划并提供对部署计划的只读访问权时，将执行此类扩展。 DeploymentPlanExectutor 将根据部署计划执行操作。|  
  
### <a name="supported-extensibility-scenarios"></a>受支持的扩展性方案  
可以实现生成或部署参与者以支持以下示例方案：  
  
-   在项目生成期间生成架构文档 - 若要支持此方案，你需要实现 [BuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.buildcontributor) 并重写 OnExecute 方法以生成架构文档。 您可以创建一个目标文件，该文件定义用于控制是否运行扩展并指定输出文件的名称的默认参数。  
  
-   在部署 SQL 项目时生成差异报告 - 若要支持此方案，你需要实现 [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor)，它可在部署 SQL 项目时生成 XML 文件。  
  
-   修改部署计划以更改移动数据的时间 - 若要支持此方案，你需要实现 [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) 并循环访问部署计划。 对于该计划中的每个 SqlTableMigrationStep，您需要检查比较结果以确定是应执行还是跳过该步骤。  
  
-   在部署 SQL 项目时将文件复制到生成的 dacpac - 若要支持此方案，你需要实现部署参与者并重写 OnEstablishDeploymentConfiguration 方法，以指定由项目系统标记为 DeploymentExtensionConfiguration 的文件。 这些文件应复制到输出文件夹并添加到生成的 dacpac 中。 也可以修改参与者以将多个文件合并为一个新文件，此文件将复制到输出文件夹并添加到部署清单中。 在部署期间，可以实现 OnApplyDeploymentConfiguration 方法以从 dacpac 中提取这些文件，并准备这些文件以便在 OnExecute 方法中使用。  
  
此外，可以公开参与者中要写入到数据库项目文件的自定义的名称/值参数对。 通过使用这些参数，可以使参与者能够提取 MSBuild 中的信息，或使参与者的最终用户能够自定义行为。 例如，您可以允许用户指定输入或输出文件的名称。  
  
## <a name="common-tasks"></a>常见任务  
  
|**常见任务**|**支持内容**|  
|--------------------|--------------------------|  
|**了解有关扩展点的详细信息：** 可以了解有关用于实现生成和部署参与者的基类的信息。|[BuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.buildcontributor)<br /><br />[DeploymentContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentcontributor)|  
|**创建示例参与者：** 了解创建生成或部署参与者所需的步骤。 如果您遵循这些演练，则将：<br /><br />-   创建可生成列出了模型中所有元素的报告的生成参与者。<br />-   创建在执行部署计划前更改该计划的部署参与者。<br />-   创建在部署 SQL 项目时生成部署报告的部署参与者。<br /><br />可以在一个程序集或多个程序集中创建所有参与者，具体取决于您希望采用什么方式向团队分发参与者。|[演练：扩展数据库项目生成以生成模型统计信息](../ssdt/walkthrough-extend-database-project-build-to-generate-model-statistics.md)<br /><br />[演练：扩展数据库项目部署以修改部署计划](../ssdt/walkthrough-extend-database-project-deployment-to-modify-the-deployment-plan.md)<br /><br />[演练：扩展数据库项目部署以分析部署计划](../ssdt/walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan.md)|  
  
## <a name="see-also"></a>另请参阅  
[定义 SQL 单元测试的自定义条件](/previous-versions/sql/sql-server-data-tools/jj860449(v=vs.103))  
