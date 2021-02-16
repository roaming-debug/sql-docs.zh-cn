---
title: Azure Data Lake Analytics 任务 | Microsoft Docs
description: 使用 Data Lake Analytics 任务，可以将 U-SQL 作业提交到 Azure Data Lake Analytics 服务。
ms.custom: ''
ms.date: 06/27/2019
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSTASK.F1
- SQL14.DTS.DESIGNER.AFPADLSTASK.F1
author: yanancai
ms.author: yanacai
ms.reviewer: maghan
ms.openlocfilehash: 2bc717a0dc59c0d880e7095e18a0c97432519e56
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346571"
---
# <a name="azure-data-lake-analytics-task"></a>Azure Data Lake Analytics 任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



使用 Data Lake Analytics 任务，可以将 U-SQL 作业提交到 Azure Data Lake Analytics 服务。 此任务是[适用于 Azure 的 SQL Server Integration Services (SSIS) 功能包](../../integration-services/azure-feature-pack-for-integration-services-ssis.md)的组成部分。

有关一般背景信息，请参阅 [Azure Data Lake Analytics](https://azure.microsoft.com/services/data-lake-analytics/)。

## <a name="configure-the-task"></a>配置任务

若要向包添加 Data Lake Analytics 任务，请将任务从 SSIS 工具箱拖到设计器画布中。 然后，双击任务，或右键单击任务并选择“编辑”  。 此时，“Azure Data Lake Analytics 任务编辑器”  对话框打开。 可以通过 SSIS 设计器或以编程方式来设置属性。

## <a name="general-page-configuration"></a>“常规”页配置

在“常规”  页上，可以配置任务，并提供任务提交的 U-SQL 脚本。 若要详细了解 U-SQL 语言，请参阅 [U-SQL 语言参考](/u-sql/)。

### <a name="basic-configuration"></a>基本配置

可以指定任务的名称和说明。

### <a name="u-sql-configuration"></a>U-SQL 配置

U-SQL 配置包含以下两个设置：SourceType  和基于 SourceType  值的动态选项。 

SourceType  指定的是 U-SQL 脚本源。 脚本在 SSIS 包执行期间提交到 Data Lake Analytics 帐户。 下面列出了此属性的选项：

|值|说明|  
|-----------|-----------------|  
|DirectInput|通过内联编辑器指定 U-SQL 脚本。 选择此值将显示动态选项 USQLStatement  。|  
|**文件连接**|指定包含 U-SQL 脚本的本地 .usql 文件。 选择此选项将显示动态选项 FileConnection  。|  
|**变量**|指定包含 U-SQL 脚本的 SSIS 变量。 选择此值将显示动态选项 **SourceVariable**。|
| &nbsp; | &nbsp; |

基于 SourceType 的动态选项  指定的是，U-SQL 查询的脚本内容。 

|SourceType|动态选项|  
|-----------|-----------------|  
|SourceType = DirectInput|直接在选项框中键入要提交的 U-SQL 查询，或选择浏览按钮 (...) 以在“输入 U-SQL 查询”  对话框中键入 U-SQL 查询。|  
|SourceType = FileConnection|选择现有文件连接管理器，或选择“<新建连接...>”  以新建文件连接。 若要了解相关信息，请参阅[文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)和[文件连接管理器编辑器](../connection-manager/file-connection-manager.md)。|  
|SourceType = 变量|选择现有变量，或选择“\<**New variable...**>”以创建新变量。 若要了解相关信息，请参阅 [Integration Services &#40;SSIS&#41; 变量](../../integration-services/integration-services-ssis-variables.md)和[添加变量](../integration-services-ssis-variables.md)。|
| &nbsp; | &nbsp; |


### <a name="job-configuration"></a>作业配置
作业配置指定 U-SQL 作业提交属性。

- **AzureDataLakeAnalyticsConnection：** 指定将 U-SQL 脚本提交到的 Data Lake Analytics 帐户。 从已定义的连接管理器的列表中选择连接。 要创建新连接，请选择“<新建连接>”  。 若要了解相关信息，请参阅 [Azure Data Lake Analytics 连接管理器](../../integration-services/connection-manager/azure-data-lake-analytics-connection-manager.md).

- JobName：  指定 U-SQL 作业的名称。 
- **AnalyticsUnits：** 指定 U-SQL 作业的分析单位数。
- **Priority：** 指定 U-SQL 作业的优先级。 可以将此属性设置为介于 0 和 1000 之间的值。 数字越小，优先级越高。
- **RuntimeVersion：** 指定 U-SQL 作业的 Data Lake Analytics 运行时版本。 默认情况下，此选项设置为“默认”。 通常无需更改此属性。
- **Synchronous：** 指定任务是否等待作业执行完成的布尔值。 如果将此值设置为 true，任务在作业完成后标记为“成功”  。 如果将此值设置为 false，任务在作业通过准备阶段后标记为“成功”  。

  |值|说明|
  |-----------|-----------------|
  |True|任务结果基于 U-SQL 作业执行结果。 作业成功先于任务成功。 作业失败先于任务失败。 任务成功或失败先于任务完成。|
  |False|任务结果基于 U-SQL 作业提交和准备结果。 作业提交成功并通过准备阶段先于任务成功。 作业提交失败或未通过准备阶段先于任务失败。 任务成功或失败先于任务完成。|
  | &nbsp; | &nbsp; |

- **TimeOut：** 指定作业执行的超时时间（以秒为单位）。 如果作业超时，就会被取消并标记为“失败”。 如果“Synchronous”  设置为“false”，便无法设置此属性。

## <a name="parameter-mapping-page-configuration"></a>“参数映射”页配置

在“Azure Data Lake Analytics 任务编辑器”  对话框的“参数映射”  页中，可以将变量映射到 U-SQL 脚本中的参数（U-SQL 变量）。

- **变量名：** 通过选择“添加”  添加参数映射后，从列表中选择系统变量或用户定义的变量。 也可以选择“<新建变量...>”  ，以通过“添加变量”  对话框来添加新变量。 若要了解相关信息，请参阅 [Integration Services &#40;SSIS&#41; 变量](../../integration-services/integration-services-ssis-variables.md)。  

- 参数名称：  提供 U-SQL 脚本中的参数/变量名称。 请确保参数名以 \@ 符号开头（如 \@Param1）。 

以下是如何将参数传递到 U-SQL 脚本的示例。

示例 U-SQL 脚本
```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    TO @out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

请注意，输入和输出路径是在 **in\@** 和 **out\@** 参数中进行定义。 U-SQL 脚本中的 **in\@** 和 **out\@** 参数值是通过“参数映射”配置动态传递。

|变量名称|参数名称|
|-------------|--------------|
|用户：Variable1|\@in|
|用户：Variable2|\@out| 
| &nbsp; | &nbsp; |

## <a name="expression-page-configuration"></a>“表达式”页配置

可以将“常规”页配置中的所有属性都分配为属性表达式，从而启用在运行时动态更新属性。 若要了解相关信息，请参阅[在包中使用属性表达式](../../integration-services/expressions/use-property-expressions-in-packages.md)。

## <a name="see-also"></a>另请参阅
- [Azure Data Lake Analytics Connection Manager](../../integration-services/connection-manager/azure-data-lake-analytics-connection-manager.md)
- [Azure Data Lake Store 文件系统任务](../../integration-services/control-flow/azure-data-lake-store-file-system-task.md)
- [Azure Data Lake Store 连接管理器](../../integration-services/connection-manager/azure-data-lake-store-connection-manager.md)