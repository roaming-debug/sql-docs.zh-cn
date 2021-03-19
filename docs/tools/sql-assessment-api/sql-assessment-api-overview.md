---
title: SQL Server 评估 API
description: 了解 SQL 评估 API，该 API 提供了一种评估 SQL Server 配置以获得最佳做法的机制。
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 3/10/2021
ms.openlocfilehash: 998e410eba1da7dcac3071170671c192f809a89c
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104611055"
---
# <a name="sql-assessment-api"></a>SQL 评估 API

[!INCLUDE [SQL Server 2012, ASMI, SQL Server on Azure VM, SQL on Linux](../../includes/applies-to-version/sql-asmi-sqlavm-sql-linux.md)]

SQL 评估 API 提供了一种机制来评估 SQL Server 的配置，以获得最佳做法。 该 API 附带一个规则集，其中包含 SQL Server 团队建议的最佳做法规则。 随着新版本的发布，此规则集得到了增强，但同时，此 API 旨在提供高度可自定义且可扩展的解决方案。 这样用户便可以优化默认规则并创建自己的规则。

若要确保 SQL Server 配置符合建议的最佳做法，你就会发现 SQL 评估 API 非常有用。 初步评估后，可以通过定期安排评估来跟踪配置稳定性。

API 可用于访问：
 
* Azure 虚拟机上的 SQL Server

* Azure SQL 数据库托管实例

* SQL Server 2012 及更高版本

* 基于 Linux 的系统上的 SQL

API 也用于面向 Azure Data Studio (ADS) 的 SQL Server 评估扩展。

>[!NOTE]
>SQL 评估 API 提供了对各种领域的评估，但它并没有深入探讨安全性。 建议使用 [SQL 漏洞评估](../../relational-databases/security/sql-vulnerability-assessment.md)来主动提高数据库的安全性。

## <a name="rules"></a>规则

规则有时被称为“检查”，它们是在 JSON 格式的文件中定义的。 规则集格式要求指定规则集名称和版本。 使用自定义规则集时，你可轻松知道哪些建议来自哪个规则集。

Microsoft 发布的规则集可以在 GitHub 上找到。 可以在[示例存储库](https://aka.ms/sql-assessment-api)中查看[完整规则集](https://github.com/microsoft/sql-server-samples/blob/567d49a42d4cf10e4942b19290ab80828b451b77/samples/manage/sql-assessment-api/DefaultRuleset.csv)。

## <a name="sql-assessment-cmdlets-and-associated-extensions"></a>SQL 评估 cmdlet 及相关扩展

SQL 评估 API 属于：

* [Azure Data Studio (ADS)](../../azure-data-studio/what-is-azure-data-studio.md)

    截至 2020 年 6 月的发行版及更高版本。

* [SQL Server 管理对象 (SMO)](../../relational-databases/server-management-objects-smo/installing-smo.md)

    截至 2019 年 7 月的发行版及更高版本。

* [SQL Server PowerShell 模块](../../powershell/download-sql-server-ps-module.md)

    截至 2019 年 7 月的发行版及更高版本。

开始使用 SQL 评估 API 之前，请确保：

* [安装 ADS](https://techcommunity.microsoft.com/t5/sql-server/released-sql-server-assessment-extension-for-azure-data-studio/ba-p/1470603)

* [安装 SMO](../../relational-databases/server-management-objects-smo/installing-smo.md)

* [安装 SQL Server PowerShell 模块](../../powershell/download-sql-server-ps-module.md)

SqlServer 模块提供两个新的 cmdlet 来与 SQL 评估 API 搭配使用：

* Get-SqlAssessmentItem - 提供 SQL Server 对象的可用评估检查列表 

* Invoke-SqlAssessment - 提供评估结果 

SQL 评估 API 扩展对 SMO Framework 进行补充，该扩展提供以下方法：

* **GetAssessmentItems** - 返回特定 SQL 对象的可用检查 (IEnumerable<…>)

* **GetAssessmentResults** - 对评估进行同步评估并返回结果和错误（如果有）(IEnumerable<…>)

* **GetAssessmentResultsList** - 对评估进行异步评估并返回结果和错误（如果有）(Task<…>)

## <a name="get-started-using-sql-assessment-cmdlets"></a>开始使用 SQL 评估 cmdlet

针对所选的 SQL Server 对象执行评估。 默认规则集中仅提供针对两种对象的检查：Server 和 Database（除此之外，该 API 还支持另外两种对象：Filegroup 和 AvailabilityGroup）。 如果要评估 SQL 实例及其所有数据库，应分别为每个对象运行 SQL 评估 cmdlet。 或者，也可以将要评估的对象通过变量或管道传递给 SQL 评估 cmdlet。

SqlServer 和 RegisteredServer 对象是可交换的，因此可以将任何对象传递给 SQL 评估 cmdlet。

浏览下面的示例以开始使用。

1. 获取本地默认实例的可用检查列表，以便熟悉这些检查。 在此示例中，我们通过管道将 Get-SqlInstance cmdlet 的输出传递到 Get-SqlAssessmentItem cmdlet，以将实例对象传递给它。

    ```powershell
    Get-SqlInstance -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

2. 获取实例的所有数据库的可用检查列表。 此处，我们使用 Get-Item cmdlet 和 Windows PowerShell SQL Server 提供程序实现的路径来获取数据库列表，然后将其传送到 Get-SqlDatabase cmdlet。

    ```powershell
    Get-Item SQLSERVER:\SQL\localhost\default | Get-SqlAssessmentItem
    ```

    此外，还可以使用 Get-SqlDatabase cmdlet 来执行相同的操作。

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

3. 获取实例的所有数据库的可用检查列表。 此处，我们使用 Get-Item cmdlet 和 Windows PowerShell SQL Server 提供程序实现的路径来获取数据库列表，然后将其传送到 Get-SqlDatabase cmdlet。

    ```powershell
    Get-Item SQLSERVER:\SQL\localhost\default | Get-SqlAssessmentItem
    ```

    此外，还可以使用 Get-SqlDatabase cmdlet 来执行相同的操作。

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

4. 调用实例的评估，并将结果保存到 SQL 表。 在此示例中，我们通过管道将 Get-SqlInstance cmdlet 的输出传递到 Invoke-SqlAssessment cmdlet，并通过管道将其结果传递给 Write-SqlTableData cmdlet。 在此示例中，Invoke-Assessment cmdlet 与 `-FlattenOutput` 参数一起运行。 此参数使输出适用于 Write-SqlTableData cmdlet。 如果不使用该参数，后者将引发错误。

    ```powershell
    Get-SqlInstance -ServerInstance 'localhost' |
    Invoke-SqlAssessment -FlattenOutput |
    Write-SqlTableData -ServerInstance 'localhost' -DatabaseName SQLAssessmentDemo -SchemaName Assessment -TableName Results -Force
    ```

    现在，我们来调用实例的所有数据库的评估，并将结果添加到同一个表中。

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' |
    Invoke-SqlAssessment -FlattenOutput |
    Write-SqlTableData -ServerInstance 'localhost' -DatabaseName SQLAssessmentDemo -SchemaName Assessment -TableName Results -Force
    ```

5. 请按照表中的说明和链接来进一步了解建议。

6. 根据环境和组织要求自定义规则（见下文）。

7. 安排任务或作业以定期或按需运行评估从而衡量进度。

## <a name="customizing-rules"></a>自定义规则

规则是可自定义和可扩展的。 Microsoft 的规则集旨在适用于大多数环境。 但是，一个规则集不可能适用于所有环境。 用户可以编写自己的 JSON 文件并自定义现有规则或添加新规则。 [示例存储库](https://aka.ms/sql-assessment-api)中提供了自定义示例和完整的 Microsoft 发布的规则集。 有关如何使用自定义 JSON 文件运行 SQL 评估 cmdlet 的更多详细信息，请使用 Get-Help cmdlet。

### <a name="options-available-with-rule-customization-feature"></a>规则自定义功能提供的选项

#### <a name="enablingdisabling-certain-rules-or-groups-of-rules-using-tags"></a>启用/禁用某些规则或规则组（使用标签）

当特定规则不适用于环境时，或在完成计划工作以解决问题之后，可以使这些规则处于静默状态。

#### <a name="changing-threshold-parameters"></a>更改阈值参数

特定规则会将自身阈值与当前的指标值进行比较，以发现问题。 如果默认阈值不合适，可以对其进行更改。

#### <a name="adding-more-rules-written-by-you-or-third-parties"></a>添加更多由所在组织或第三方编写的规则

通过将一个或多个 JSON 文件作为参数添加到 SQL 评估 API 调用中，可以将多个规则集组合在一起。 组织可能会编写这些文件，或从第三方获取这些文件。 例如，可以让你的 JSON 文件禁用 Microsoft 规则集中的特定规则，让行业专家提供的另一个 JSON 文件包含你认为对环境有用的规则，然后再让另一个 JSON 文件更改该 JSON 文件中的某些阈值。

>[!IMPORTANT]
>我们强烈建议不要使用来自不受信任源的规则集，除非对其全面检查以确保它们安全。

## <a name="next-steps"></a>后续步骤

* [SQL Server 管理对象 (SMO)](../../relational-databases/server-management-objects-smo/overview-smo.md)
* [PowerShell](../../powershell/download-sql-server-ps-module.md)
* [SQL 漏洞评估](../../relational-databases/security/sql-vulnerability-assessment.md)