---
title: 将平面文件导入 SQL |Microsoft 文档
description: 利用导入平面文件向导可以轻松地将数据从 .csv 或 .txt 文件复制到新数据库表。 本文介绍如何以及何时使用该向导。
ms.custom: ''
ms.date: 09/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: data-movement
ms.topic: conceptual
f1_keywords:
- sql13.swb.importflatfile.f1
author: yualan
ms.author: alayu
ms.reviewer: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 444489bc74106ca43d645e26564543b4452ac8ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351350"
---
# <a name="import-flat-file-to-sql-wizard"></a>将平面文件导入 SQL 向导
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
> 有关导入和导出向导的相关内容，请参阅 [SQL Server 导入和导出向导](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md)。

利用导入平面文件向导可以轻松地将数据从平面文件 (.csv, .txt) 复制到数据库中的新表。  导入平面文件向导支持以逗号分隔的文件和固定宽度格式的文件。 本概述描述了使用此向导的原因、查找此向导的方法，以及可遵循的简单示例。

## <a name="why-would-i-use-this-wizard"></a>为什么使用此向导？
创建此向导的目的在于利用称为 Program Synthesis using Examples 的智能框架改善当前的导入体验 ([PROSE](https://microsoft.github.io/prose/))。 对于没有专业领域知识的用户来说，导入数据通常是一项复杂、易于出错并且乏味的任务。 此向导尽可能简化导入流程，只需要选择输入文件和唯一的表名称，PROSE 框架就会处理剩余的事情。

PROSE 分析输入文件的数据模式，以推断列名称、类型、分隔符等信息。 此框架了解文件的结构，完成所有复杂的工作，因此用户无需再做这些工作。

若要进一步了解导入平面文件向导的用户体验改进，请观看以下视频：

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-the-new-Import-Flat-File-Wizard-in-SSMS-173/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a>先决条件
此功能适用于 SQL Server Management Studio (SSMS) v17.3 或更高版本。 请确保使用最新版本。 可以在[此处](../../ssms/download-sql-server-management-studio-ssms.md)找到最新版本。
 
## <a name="getting-started"></a><a id="started"></a>入门
要访问导入平面文件向导，请执行以下步骤：

1. 打开 SQL Server Management Studio  。
2. 连接 SQL Server 数据库引擎实例或 localhost。
3. 展开“数据库”，右键单击数据库（下方示例的测试），指向“任务”，然后单击导入数据上方的“导入平面文件”    。

![向导菜单](media/import-flat-file-wizard/import-flat-file-menu.png)

要了解有关向导不同功能的详细信息，请参考以下教程：

## <a name="tutorial"></a>教程
为了实现本教程的目的，请随意使用自己的平面文件。 另外，本教程会使用 Excel 生成的以下 CSV，你可以随意复制。 如果使用此 CSV，则将其标题命名为“example.csv”，确保在容易找到的位置（如桌面）将其另存为 csv  。

![向导 Excel](media/import-flat-file-wizard/import-flat-file-example.png)

概述：
1. [访问向导](#step-1-access-wizard-and-intro-page)
2. [指定输入文件](#step-2-specify-input-file)
3. [预览数据](#step-3-preview-data)
4. [修改列](#step-4-modify-columns)
5. [摘要](#step-5-summary)
6. [结果](#step-6-results)

### <a name="step-1-access-wizard-and-intro-page"></a>步骤 1：访问向导和简介页
访问如[此处](#started)所述的向导。

向导第一页是欢迎页。 如果不想再次看到此页面，请随意单击“不再显示此起始页”  。

![向导简介](media/import-flat-file-wizard/import-flat-file-intro.png)

### <a name="step-2-specify-input-file"></a>步骤 2：指定输入文件
单击“浏览”选择输入文件。 此向导默认搜索 .csv 和 .txt 文件。 无论文件扩展名是什么，PROSE 都将检测文件采用的是逗号分隔格式，还是固定宽度格式。

新的表名称应该是唯一的，如果不是，向导不会允许执行下一步。

![向导指定](media/import-flat-file-wizard/import-flat-file-specify.png)

### <a name="step-3-preview-data"></a>步骤 3：预览数据
在向导生成的预览中，可以查看前 50 行。 如果出现任何问题，请单击“取消”，否则将会继续进入下一页面。

![向导预览](media/import-flat-file-wizard/import-flat-file-preview.png)

### <a name="step-4-modify-columns"></a>步骤 4：修改列
向导会标识它认为正确的列名称、数据类型等。如果字段不正确，可以在此处编辑这些字段（例如，数据类型应该是 float 而不是int）。

检测到空值的列将选中“允许 Null”。 但如果你希望列中包含 null，并且不选中“允许 Null”，则可以在此处更新表定义以允许一列或所有列中包含 null。

准备就绪时，请继续。

![向导修改](media/import-flat-file-wizard/import-flat-file-modify.png)

### <a name="step-5-summary"></a>步骤 5：总结
这只是一个显示当前配置的摘要页面。 如果出现问题，可以返回到上一部分。 或者，单击“完成”尝试导入过程。

![向导摘要](media/import-flat-file-wizard/import-flat-file-summary.png)

### <a name="step-6-results"></a>步骤 6：结果
此页面指示导入是否成功。 如果出现绿色的选中标记，表示成功；反之，可能需要检查配置或者输入文件是否出错。

![向导结果](media/import-flat-file-wizard/import-flat-file-results.png)

## <a name="troubleshooting"></a>疑难解答
导入平面文件向导基于前 200 行检测数据类型。  在平面文件中的数据不符合自动检测到的数据类型的情况下，将在导入过程中出现错误。 错误消息类似于以下内容：
```
Error inserting data into table. (Microsoft.SqlServer.Prose.Import)
The given value of type String from the data source cannot be converted to type nvarchar of the specified target column. (System.Data)
String or binary data would be truncated. (System.Data)
```
缓解此错误的技术：
- 扩展[修改列步骤](#step-4-modify-columns)中的数据类型大小（如 nvarchar 列的长度）可能会补偿平面文件的其余部分中数据的变体。
- 在[修改列步骤](#step-4-modify-columns)（尤其是较小数目）中启用错误报告，将显示平面文件中的哪些行包含不适合所选数据类型的数据。 例如，在第二行出现错误的平面文件中，使用范围为 1 的错误报告运行导入将提供特定的错误消息。  直接在所在位置中检查文件可以基于标识的行中的数据对数据类型进行更有针对性的更改。

![报告结果时出错](media/import-flat-file-wizard/import-flat-file-error.png)

```
Error inserting data into table occured while inserting rows 1 - 2. (Microsoft.SqlServer.Prose.Import)
The given value of type String from the data source cannot be converted to type float of the specified target column. (System.Data)
Failed to convert parameter value from a String to a Double. (System.Data)
```


## <a name="learn-more"></a>了解详细信息

了解有关向导的详细信息。
 
- **了解有关导入其他数据源的详细信息。** 如果要导入除平面文件外的其他内容，请参阅 [SQL Server 导入和导出向导](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md)。
- **了解有关连接平面文件数据源的详细信息。** 如果想要寻找有关连接平面文件数据源的详细信息，请参阅[连接到平面文件数据源](../../integration-services/import-export-data/connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard.md)。
- **了解有关 PROSE 的详细信息。** 如果想要寻找向导使用的智能框架的概述，请参阅 [PROSE SDK](https://microsoft.github.io/prose/)。