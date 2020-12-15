---
title: 为本地数据库 (数据迁移助手) 标识正确的 Azure SQL 数据库 SKU |Microsoft Docs
description: 了解如何使用数据迁移助手为本地数据库标识正确的 Azure SQL 数据库 SKU
ms.custom: ''
ms.date: 05/06/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.openlocfilehash: 8d4de2aea0a9020a7f971f36dd011c435a3cec3d
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489493"
---
# <a name="identify-the-right-azure-sql-databasemanaged-instance-sku-for-your-on-premises-database"></a>确定本地数据库的正确的 Azure SQL 数据库/托管实例 SKU

将数据库迁移到云可能会很复杂，尤其是在尝试为数据库选择最佳的 Azure 数据库目标和 SKU 时。 数据库迁移助手的目标 (DMA) 旨在帮助解决这些问题，通过在用户友好的输出中提供这些 SKU 建议，使你的数据库迁移体验更简单。

本文重点介绍 DMA 的 Azure SQL 数据库 SKU 建议功能。 Azure SQL 数据库和 Azure SQL 托管实例有多个部署选项，其中包括：

- 单一数据库
- 弹性池
- 托管实例

使用 SKU 建议功能，可以根据从计算机上收集的性能计数器（ (s) 托管数据库），确定最低建议的 Azure SQL 数据库单一数据库或 Azure SQL 托管实例 SKU。 此功能提供与定价层、计算级别和最大数据大小相关的建议，以及每个月的预估成本。 它还为所有推荐的数据库提供大容量预配单数据库和托管实例的功能。

> [!NOTE]
> 此功能当前仅通过命令行界面 (CLI) 提供。

以下说明可帮助你在 Azure 中使用 DMA 来确定 SKU 建议并设置相应的单个数据库 () 或 () 。

## <a name="prerequisites"></a>必备条件

- 下载并安装最新版本的 [DMA](https://aka.ms/get-dma)。 如果你已经具有该工具的早期版本，请将其打开，系统会提示你升级 DMA。
- 确保你的计算机具有 [PowerShell 版本 5.1](https://www.microsoft.com/download/details.aspx?id=54616) 或更高版本，以便运行所有脚本。 有关如何找出计算机上安装的 PowerShell 版本的信息，请参阅 [下载并安装 Windows PowerShell 5.1](/skypeforbusiness/set-up-your-computer-for-windows-powershell/download-and-install-windows-powershell-5-1)一文。
  > [!NOTE]
  > 若要收集计算机信息，数据收集脚本使用 PowerShell 6 中弃用的 Get-WmiObject cmdlet。 若要在 PowerShell 6 或7中运行此脚本，必须将 WMI cmdlet 替换为较新的 CIM cmdlet。
- 确保计算机上已安装 Azure Powershell 模块。 有关详细信息，请参阅 [安装 Azure PowerShell 模块](/powershell/azure/install-az-ps?view=azps-1.8.0&preserve-view=true)一文。
- 验证要收集性能计数器所需的 PowerShell 文件 **SkuRecommendationDataCollectionScript.ps1** 是否安装在 DMA 文件夹中。
- 确保你要在其上执行此过程的计算机对承载数据库的计算机具有管理员权限。

## <a name="collect-performance-counters"></a>收集性能计数器

此过程的第一步是收集数据库的性能计数器。 可以通过在承载数据库的计算机上运行 PowerShell 命令来收集性能计数器。 DMA 提供此 PowerShell 文件的副本，但你也可以使用自己的方法来捕获计算机中的性能计数器。

不需要为每个数据库单独执行此任务。 从计算机收集的性能计数器可用于为计算机上托管的所有数据库推荐 SKU。

1. 在 DMA 文件夹中，找到 PowerShell 文件 SkuRecommendationDataCollectionScript.ps1。 收集性能计数器需要此文件。

    ![DMA 文件夹中显示的 PowerShell 文件](../dma/media/dma-sku-recommend-data-collection-file.png)

2. 运行具有以下参数的 PowerShell 脚本：
    - **ComputerName**：托管数据库的计算机的名称。
    - **OutputFilePath**：用于保存所收集计数器的输出文件路径。
    - **CollectionTimeInSeconds**：要收集性能计数器数据的时间量。 捕获至少40分钟的性能计数器，以获得有意义的建议。 捕获持续时间越长，建议的准确性就越多。 还要确保工作负荷正在为所需的数据库运行，以实现更准确的建议。
    - **DbConnectionString**：指向要从中收集性能计数器数据的计算机上承载的 master 数据库的连接字符串。

    下面是一个示例调用：

    ```
    .\SkuRecommendationDataCollectionScript.ps1
     -ComputerName Foobar1
     -OutputFilePath D:\counters2.csv
     -CollectionTimeInSeconds 2400
     -DbConnectionString Server=localhost;Initial Catalog=master;Integrated Security=SSPI;
    ```

    执行命令后，进程会将包含性能计数器的文件输出到指定的位置。 您可以使用此文件作为过程下一部分的输入，这将为单数据库和托管实例选项提供 SKU 建议。

## <a name="use-the-dma-cli-to-get-sku-recommendations"></a>使用 DMA CLI 获取 SKU 建议

使用为此进程创建的性能计数器输出文件作为输入。

对于单一数据库选项，DMA 将为计算机上的每个数据库提供 Azure SQL 数据库单一数据库定价层、计算级别和最大数据大小的建议。 如果你的计算机上有多个数据库，则还可以指定你想要获取其建议的数据库。 DMA 还将为您提供每个数据库的每月预估成本。

对于托管实例，建议支持提升和移动方案。 因此，DMA 将为你提供有关 Azure SQL 托管实例定价层、计算级别以及计算机上数据库集的最大数据大小的建议。 同样，如果你的计算机上有多个数据库，则还可以指定你想要获取其建议的数据库。 DMA 还将为你提供托管实例的每月预估成本。

若要使用 DMA CLI 获取 SKU 建议，请在命令提示符处使用以下参数运行 dmacmd.exe：

- **/Action = SkuRecommendation**：输入此参数以执行 SKU 评估。
- **/SkuRecommendationInputDataFilePath**：在上一节中收集到的计数器文件的路径。
- **/SkuRecommendationTsvOutputResultsFilePath**：用 TSV 格式写入输出结果的路径。
- **/SkuRecommendationJsonOutputResultsFilePath**：以 JSON 格式写入输出结果的路径。
- **/SkuRecommendationHtmlResultsFilePath**：用于以 HTML 格式写入输出结果的路径。

此外，选择以下参数之一：

- 防止价格刷新
  - **/SkuRecommendationPreventPriceRefresh**：如果设置为 True，则防止发生价格刷新，并假定默认价格。 如果在脱机模式下运行，则使用。 如果不使用此参数，则必须指定以下参数以获取基于指定区域的最新价格。
- 获取最新价格
  - **/SkuRecommendationCurrencyCode**：显示价格的货币 (例如 "USD" ) 。
  - **/SkuRecommendationOfferName**：提议名称 (例如，"Bc-op-nt-azr-ms-azr-0003p" ) 。 有关详细信息，请参阅 [Microsoft Azure 产品/服务详细](https://azure.microsoft.com/support/legal/offer-details/) 信息页。
    - **/SkuRecommendationRegionName**：区域名称 (例如，"WestUS" ) 。
    - **/SkuRecommendationSubscriptionId**：订阅 ID。
    - **/AzureAuthenticationTenantId**：身份验证租户。
    - **/AzureAuthenticationClientId**：用于身份验证的 AAD 应用的客户端 ID。
    - 以下身份验证选项之一：
      - 交互
        - **AzureAuthenticationInteractiveAuthentication**：对于身份验证弹出窗口，将设置为 true。
      - 基于证书
        - **AzureAuthenticationCertificateStoreLocation**：设置为证书存储位置 (例如，"CurrentUser" ) 。
        - **AzureAuthenticationCertificateThumbprint**：设置为证书指纹。
      - 基于令牌
        - **AzureAuthenticationToken**：设置为证书标记。

> [!NOTE]
> 若要获取 ClientId 和 TenantId 以便进行交互式身份验证，需要配置新的 AAD 应用程序。 有关身份验证和获取这些凭据的详细信息，请 [参阅 Microsoft Azure 计费 Api 代码示例： RATECARD api](https://github.com/Azure-Samples/billing-dotnet-ratecard-api)中的 **步骤1：配置 AAD 租户中的本机客户端应用程序中** 的说明。

最后，有一个可选参数，可用于指定要获取其建议的数据库： 

- **/SkuRecommendationDatabasesToRecommend**：为其提出建议的数据库列表。 数据库名称区分大小写，并且必须在/SkuRecommendationDatabasesToRecommend 中找到)  (1， (2) 每个都用双引号括起来， (3) 两个名称之间用一个空格分隔 (例如，= "Database1" "Database2" "Database3" ) 。 省略此参数可确保为输入 .csv 文件中标识的所有用户数据库提供建议。  

下面是一些示例调用：

**示例1：获取具有默认价格的建议。在脱机模式下运行或没有身份验证凭据时使用。**

```
.\DmaCmd.exe /Action=SkuRecommendation
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv"
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv"
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json"
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html"
/SkuRecommendationPreventPriceRefresh=true
```

**示例2：获取指定区域最新价格的建议 (例如，"UKWest" ) 。**

```
.\DmaCmd.exe /Action=SkuRecommendation
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv"
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv"
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json"
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html"
/SkuRecommendationCurrencyCode=USD
/SkuRecommendationOfferName=MS-AZR-0044p
/SkuRecommendationRegionName=UKWest
/SkuRecommendationSubscriptionId=<Your Subscription Id>
/AzureAuthenticationInteractiveAuthentication=true
/AzureAuthenticationClientId=<Your AzureAuthenticationClientId>
/AzureAuthenticationTenantId=<Your AzureAuthenticationTenantId>
```

**示例3：获取特定数据库的建议 (例如 "TPCDS1G，EDW_3G，TPCDS10G" ) "。**

```
.\DmaCmd.exe /Action=SkuRecommendation 
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv" 
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv" 
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json" 
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html" 
/SkuRecommendationCurrencyCode=USD 
/SkuRecommendationOfferName=MS-AZR-0044p 
/SkuRecommendationRegionName=UKWest 
/SkuRecommendationSubscriptionId=<Your Subscription Id> 
/SkuRecommendationDatabasesToRecommend=“TPCDS1G” “EDW_3G” “TPCDS10G” 
/AzureAuthenticationInteractiveAuthentication=true 
/AzureAuthenticationClientId=<Your AzureAuthenticationClientId> 
/AzureAuthenticationTenantId=<Your AzureAuthenticationTenantId>
```

对于单一数据库建议，TSV 输出文件将如下所示：

![DMA 文件夹中显示的 PowerShell 单一 db 文件](../dma/media/dma-sku-recommend-single-db-recommendations.png)

对于托管实例建议，TSV 输出文件将如下所示：

![DMA 文件夹中显示的 PowerShell 托管实例文件](../dma/media/dma-sku-recommend-mi-recommendations.png)

下面是输出文件中每一列的说明。

- **DatabaseName** -数据库的名称。
- **MetricType** -推荐的性能层。
- **MetricValue** -建议的 SKU。
- **PricePerMonth** –对应 SKU 的每月估计价格。
- **RegionName** –相应 SKU 的区域名称。 
- **IsTierRecommended** -我们为每个层提供最低的 SKU 建议。 然后，应用试探法来确定数据库的正确层。 这会反映为数据库建议的层。 
- **ExclusionReasons** -如果建议使用层，则此值为空。 对于不推荐的每个层，我们提供未选择它的原因。
- **AppliedRules** -所应用规则的简短表示法。

最终建议的层 (例如， **MetricType**) 和值 (即， **MetricValue**) -发现 **ISTIERRECOMMENDED** 列的值为 TRUE，表示查询在 Azure 中运行所需的最低 SKU，这与本地数据库相似。 对于 Azure SQL 托管实例，DMA 目前支持最常用的8vcore 到 40vcore Sku 的建议。 例如，如果建议的最低 SKU 为标准层的 S4，则选择 "S3" 或以下将导致查询超时或执行失败。

HTML 文件以图形格式包含这些信息。 它提供了一个用户友好的方法，用于查看最终建议并预配过程的下一部分。 有关 HTML 输出的详细信息，请在以下部分中进行。

## <a name="provision-recommended-skus-to-azure"></a>将推荐的 Sku 预配到 Azure

只需单击几下，即可使用标识的建议在 Azure 中预配目标 Sku，你可以将数据库迁移到该目标 Sku。 可以使用 HTML 文件输入 Azure 订阅;选择数据库的定价层、计算级别和最大数据大小;并生成用于设置数据库的脚本。 可以使用 PowerShell 执行此脚本。

可以在一台计算机上执行此过程，也可以在多台计算机上执行此过程，以确定大规模的 SKU 建议。 DMA 目前通过命令行界面支持整个过程，使其成为简单且可扩展的体验。

若要输入设置信息并对建议进行更改，请按如下所示更新 HTML 文件。

**对于单一数据库建议**

![Azure SQL 数据库 SKU 建议屏幕](../dma/media/dma-sku-recommend-single-db-recommendations1.png)

1. 打开 HTML 文件，并输入以下信息：
    - **订阅 id** -要预配数据库的 Azure 订阅的订阅 id。
    - **资源组** -要将数据库部署到的资源组。 输入存在的资源组。
    - **区域** -在其中预配数据库的区域。 请确保订阅支持选择区域。
    - **服务器名称** -要将数据库部署到的 Azure SQL 数据库服务器。 如果输入的服务器名称不存在，则将创建该服务器名称。
    - **管理员用户名** -服务器管理员用户名。
    - **管理员密码** -服务器管理员密码。 密码长度必须至少为八个字符，且长度不能超过128个字符。 密码必须含以下字符类别中的三类 – 英文大写字母、英文小写字母、数字(0-9)及非字母数字字符（!、$、#、% 等）。 密码不能包含用户名中的全部或部分 (3 + 连续字母) 。

2. 查看每个数据库的建议，并根据需要修改定价层、计算级别和最大数据大小。 请确保取消选择当前不想预配的任何数据库。

3. 选择 " **生成设置脚本**"，保存脚本，然后在 PowerShell 中执行它。

    此过程应创建您在 HTML 页中选择的所有数据库。

**对于 Azure SQL 托管实例建议**

![Azure SQL MI SKU 建议屏幕](../dma/media/dma-sku-recommend-mi-recommendations1.png)

1. 打开 HTML 文件，并输入以下信息：
    - **订阅 id** -要预配数据库的 Azure 订阅的订阅 id。
    - **资源组** -要将数据库部署到的资源组。 输入存在的资源组。
    - **区域** -在其中预配数据库的区域。 请确保订阅支持选择区域。
    - **实例名称** –要将数据库迁移到的 Azure SQL 托管实例的实例。 实例名称只能包含小写字母、数字和 "-"，但不能以 "-" 开头或结尾，也不能超过63个字符。
    - **实例管理员用户名** –实例管理员用户名。 请确保登录名满足以下要求-它是 SQL 标识符，不是典型的系统名称 (如 admin、administrator、sa、root、dbmanager、loginmanager 等 ) 或内置数据库用户或角色 (如 dbo、guest、) public 等。 请确保名称中不包含空格、Unicode 字符或非字母字符，并且不以数字或符号开头。 
    - **实例管理员密码** -实例管理员密码。 密码长度必须至少为16个字符，且长度不能超过128个字符。 密码必须含以下字符类别中的三类 – 英文大写字母、英文小写字母、数字(0-9)及非字母数字字符（!、$、#、% 等）。 密码不能包含用户名中的全部或部分 (3 + 连续字母) 。
    - **Vnet 名称** –应在其下预配托管实例的 vnet 名称。 输入现有的 VNet 名称。
    - **子网名称** –应在其下预配托管实例的子网名称。 输入现有子网名称。

2. 查看每个实例的建议，并根据需要修改定价层、计算级别和最大数据大小。 尽管建议当前限制为8vcore 到 40vcore Sku，但仍有必要提供64vcore 和 80vcore Sku 的选项。 请确保取消选择当前不想预配的任何实例。

    此过程应创建您在 HTML 页中选择的所有数据库。

    > [!NOTE]
    > 在子网中创建托管实例 (第一次) 可能需要几个小时才能完成。 通过 PowerShell 运行预配脚本后，可以在 Azure 门户上检查部署的状态。

## <a name="next-step"></a>后续步骤

- 有关从 CLI 运行 DMA 的命令的完整列表，请参阅 [从命令行运行数据迁移助手](./dma-commandline.md)一文。
