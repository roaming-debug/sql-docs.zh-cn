---
title: 使用 SSIS 从 Excel 导入数据或将数据导出到 Excel | Microsoft Docs
description: 了解如何使用 SQL Server Integration Services (SSIS) 导入或导出 Excel 数据，以及先决条件、已知问题和限制。
ms.date: 06/29/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.custom: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e497e0f2781aedc2adccc4263cc3b37e756a86da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345706"
---
# <a name="import-data-from-excel-or-export-data-to-excel-with-sql-server-integration-services-ssis"></a>使用 SQL Server Integration Services (SSIS) 从 Excel 导入数据或将数据导出到 Excel

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



本文介绍了必须提供的连接信息，以及必须配置的设置，以便从 Excel 导入数据或使用 SQL Server Integration Services (SSIS) 将数据导出到 Excel。

以下各节包含在 SSIS 中成功使用 Excel 或者了解和排除常见问题所需的信息：

1.  可以使用的[工具](#tools)。

2.  所需[文件](#files-you-need)。

3.  通过 SSIS 从 Excel 加载数据或向 Excel 加载数据时必须提供的连接信息以及必须配置的设置。
    -   [指定 Excel](#specify-excel) 作为数据源。
    -   提供 [Excel 文件和路径](#excel-file)。
    -   选择 [Excel 版本](#excel-version)。
    -   指定[第一行是否包含列名称](#first-row)。
    -   提供[包含数据的工作表或范围](#sheets-ranges)。

4.  已知问题和限制。
    -   [数据类型](#issues-types)的问题。
    -   [导入](#issues-importing)的问题。
    -   [导出](#issues-exporting)的问题。

## <a name="tools-you-can-use"></a><a name="tools"></a> 可以使用的工具

可使用以下某一工具通过 SSIS 从 Excel 导入数据或将数据导出到 Excel：

-   **SQL Server Integration Services (SSIS)** 。 使用 Excel 连接管理器创建使用 Excel 源或 Excel 目标的 SSIS 包。 （本文不介绍如何创建 SSIS 包。）

-   内置于 SSIS 的“SQL Server 导入和导出向导”  。 有关详细信息，请参阅[使用 SQL Server 导入和导出向导导入和导出数据](import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md)和[连接到 Excel 数据源（SQL Server 导入和导出向导）](import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md)。

## <a name="get-the-files-you-need-to-connect-to-excel"></a><a name="files-you-need"></a>获取连接到 Excel 所需的文件

如果没有安装 Excel 的连接组件，首先需要下载这些组件，然后才能通过 SSIS 从 Excel 导出数据或将数据导入 Excel。 默认情况下，没有安装 Excel 的连接组件。

在此处下载用于 Excel 的最新版连接组件：[Microsoft Access 数据库引擎 2016 可再发行组件](https://www.microsoft.com/download/details.aspx?id=54920)。 最新版组件可以打开 Excel 早期版本创建的文件。

### <a name="notes-about-the-download-and-installation"></a>有关下载和安装说明

-   请确保已下载 Access 数据库引擎 2016 可再发行组件，而不是 Microsoft Access 2016 Runtime   。

-   如果计算机已安装 32 位版本的 Office，则必须安装 32 位版本的组件。 还必须确保在 32 位模式下运行 SSIS 包，或运行 32 位版本的导入和导出向导。

-   如果拥有 Microsoft 365 订阅，则运行安装程序时可能会显示错误消息。 错误消息指出该下载项无法与 Office 即点即用组件并行安装。 若要绕过此错误消息，请打开命令提示符窗口并使用 `/quiet` 开关运行下载的 .EXE 文件，从而在安静模式下运行安装。 例如：

    `C:\Users\<user_name>\Downloads\AccessDatabaseEngine.exe /quiet`

    如果安装 2016 可再发行组件遇到问题，请改为从此处安装 2010 可再发行组件：[Microsoft Access 2010 可再发行组件](https://www.microsoft.com/download/details.aspx?id=13255)。 （不再发行 Excel 2013。）

## <a name="specify-excel-as-your-data-source"></a><a name="specify-excel"></a> 指定 Excel 作为数据源

第一步需要指出希望连接到 Excel。

### <a name="in-ssis"></a>在 SSIS 中
在 SSIS 中，创建 Excel 连接管理器以连接到 Excel 源或目标文件。 创建连接管理器有多种方法：

-   在“连接管理器”区域，右键单击并选择“新建连接”   。 在“添加 SSIS 连接管理器”对话框中，选择“EXCEL”，然后选择“添加”    。
 
-   在“SSIS”菜单上，选择“新建连接”   。 在“添加 SSIS 连接管理器”对话框中，选择“EXCEL”，然后选择“添加”    。

-   创建连接管理器的同时，在“Excel 源编辑器”或“Excel 目标编辑器”页配置“Excel 源”或“Excel 目标”      。

### <a name="in-the-sql-server-import-and-export-wizard"></a>在 SQL Server 导入和导出向导中
在导入和导出向导中，在“选择数据源”或“选择目标”页上，选择“数据源”列表中的“Microsoft Excel”     。

如果在数据源列表中看不到 Excel，请确保运行的是 32 位向导。 Excel 连接组件通常是 32 位文件，在 64 位向导中不可见。

## <a name="excel-file-and-file-path"></a><a name="excel-file"></a>Excel 文件和文件路径

提供的第一条信息是 Excel 文件的路径和文件名称。 在 SSIS 包的“Excel 连接管理器编辑器”中提供此信息，或在导入和导出向导的“选择数据源”页或“选择目标”页中提供    。

输入路径和文件名，格式如下所示：

-   针对本地计算机上的文件，指定 C:\\TestData.xlsx。

-   针对网络共享上的文件，指定 \\\\Sales\\Data\\TestData.xlsx。

或者，通过使用“打开”对话框，单击“浏览”定位电子表格   。  
  
> [!IMPORTANT]
> 无法连接到受密码保护的 Excel 文件。

## <a name="excel-version"></a><a name="excel-version"></a>Excel 版本

提供的第二条消息是 Excel 文件版本。 在 SSIS 包的“Excel 连接管理器编辑器”中提供此信息，或在导入和导出向导的“选择数据源”页或“选择目标”页中提供    。

选择用于创建文件的 Microsoft Excel 的版本，或另一可兼容的版本。 例如，如果安装 2016 连接组件遇到问题，可安装 2010 组件，然后选择该列表中的“Microsoft Excel 2007-2010”  。

如果只安装了旧版本的连接组件，则可能无法在此列表中选择新版本的 Excel。 “Excel 版本”列表包括 SSIS 支持的所有 Excel 版本  。 此列表中显示的项并不表示已安装必需的连接组件。 例如，即使没有安装 2016 连接组件，列表中仍会显示“Microsoft Excel 2016”  。

## <a name="first-row-has-column-names"></a><a name="first-row"></a>首行包含列名称

如果要从 Excel 导入数据，则下一步是指示数据的首行是否包含列名称。 在 SSIS 包的“Excel 连接管理器编辑器”中提供此信息，或在导入和导出向导的“选择数据源”页中提供   。

-   如果因为源数据不具有列名称而禁用此选项，则向导将使用 F1、F2 等作为列标题。
-   如果数据包含列名称，但禁用了此选项，则向导会导入列名称作为数据的首行。
-   如果数据不包含列名称，但启用了此选项，则向导会将源数据的首行用作列名称。 在此情况下，源数据的首行不再包含在数据本身当中。

如果要从 Excel 导出数据，并且启用了此选项，则导出的数据的首行包括列名称。

## <a name="worksheets-and-ranges"></a><a name="sheets-ranges"></a>工作表和范围

以下有三种可用作数据的源或目标的 Excel 对象类型：工作表、已命名范围或用其地址指定的未命名单元格范围。

-   **工作表。** 若要指定工作表，请将 `$` 字符附加到表名称的末尾，并用分隔符包围字符串 - 例如，[Sheet1$]  。 或者，在现有表和视图的列表中寻找以 `$` 字符结尾的名称。

-   **命名区域。** 若要指定命名区域，请提供区域名称 - 例如，MyDataRange  。 或者，在现有表和视图的列表中寻找不是以 `$` 字符结尾的名称。
    
-   **未命名区域。** 若要指定未命名的单元格区域，请将 $ 字符附加到表名称的末尾，添加区域说明，并用分隔符包围字符串 - 例如， **[Sheet1$ A1: B4]** 。

要选择或指定想要用作数据的源或目标的 Excel 对象类型，请执行以下操作之一：

### <a name="in-ssis"></a>在 SSIS 中

在 SSIS 中，在“Excel 源编辑器”或“Excel 目标编辑器”的“连接管理器”页上，执行以下操作之一    ：

-   要使用“工作表”或“命名区域”，请选择“表或视图”作为“数据访问模式”     。 然后，在“Excel 表的名称”列表中，选择包含工作表或命名范围  。

-   要使用用其地址指定的“未命名区域”，请选择“SQL 命令”作为“数据访问模式”    。 然后，在“SQL 命令文本”字段中，输入如下查询  ：

    ```sql
    SELECT * FROM [Sheet1$A1:B5]
    ```

### <a name="in-the-sql-server-import-and-export-wizard"></a>在 SQL Server 导入和导出向导中
在导入和导出向导中，执行以下操作之一：

-   从 Excel 导入时，执行以下操作之一  ：

    -   要使用“工作表”或“命名区域”，请在“指定表副本或查询”页上，选择“从一个或多个表或视图复制数据”     。 然后，在“选择源表和视图”页的“源”列中，选择源工作表和命名区域   。

    -   要使用用其地址指定的“未命名区域”，请在“指定表副本或查询”页上，选择“编写查询以指定要传输的数据”    。 然后，在“提供源查询”页，提供如下查询  ：

        ```sql
        SELECT * FROM [Sheet1$A1:B5]
        ```

-   导出到 Excel 时，执行以下操作之一  ：

    -   要使用“工作表”或“命名区域”，请在“选择源表和视图”页的“目标”列中，选择目标工作表和命名区域     。

    -   要使用用其地址指定的“未命名区域”，请在“选择源表和视图”页的“目标”列中输入区域，格式如下所示（不含分隔符）：`Sheet1$A1:B5`。 该向导会添加分隔符。

选择或输入要导入或导出的 Excel 对象后，还可在向导的“选择源表和视图”页上执行以下操作  ：

-   通过选择“编辑映射”查看源和目标之间的列映射  。

-   通过选择“预览”预览示例数据以确认是否需要  。

## <a name="issues-with-data-types"></a><a name="issues-types"></a>数据类型的问题

### <a name="data-types"></a>数据类型

Excel 驱动程序只识别有限的一组数据类型。 例如，所有数值列均解释为双精度 (DT_R8)，并且所有字符串列（除了 memo 列）均解释为 255 个字符的 Unicode 字符串 (DT_WSTR)。 SSIS 按如下所示方式映射 Excel 数据类型：

-   数值 - 双精度浮点 (DT_R8)

-   货币 - 货币 (DT_CY)

-   布尔 - 布尔 (DT_BOOL)

-   日期/时间 - 日期 (DT_DATE)

-   字符串 - Unicode 字符串，长度为 255 (DT_WSTR)

-   Memo - Unicode 文本流 (DT_NTEXT)

### <a name="data-type-and-length-conversions"></a>数据类型和长度转换

SSIS 不隐式转换数据类型。 因此，在将其加载到非 Excel 目标之前，可能需要使用“派生列”或“数据转换”转换来显式转换 Excel 数据，或在将它加载到 Excel 目标之前，从非 Excel 源转换数据。

此处是一些可能必需的转换的示例：  
  
-   Unicode Excel 字符串列与具有特定代码页的非 Unicode 字符串列之间的转换。
  
-   在 255 个字符的 Excel 字符串列和不同长度的字符串列之间转换。
  
-   双精度 Excel 数值列与其他类型的数值列之间的转换。

> [!TIP]
> 如果使用的是导入和导出向导，并且数据需要一些转换，则向导会配置必要的转换。 因此，即使想使用 SSIS 包时，使用导入和导出向导创建初始包也十分有用。 使用该向导帮助你创建和配置连接管理器、源、转换和目标。

## <a name="issues-with-importing"></a><a name="issues-importing"></a>导入的问题

### <a name="empty-rows"></a>空行

将工作表或命名区域指定为源时，该驱动程序都将读取从工作表或区域左上角第一个非空单元开始的连续单元块  。 因此，数据无需从行 1 开始，但在源数据中不能有空行。 例如，列标头和数据行之间不能有空行，工作表顶部的标题后不能跟空行。

如果数据上面有空行，则不能将数据作为工作表进行查询。 在 Excel 中，必须选择数据区域，然后为区域分配名称，并查询命名区域而非工作表。

### <a name="missing-values"></a>缺少值

Excel 驱动程序读取指定源中一定数量的行（默认情况下为八行）以推测每列的数据类型。 如果推测出列可能包含混合数据类型（尤其是混合了文本数据的数值数据时），驱动程序将决定采用占多数的数据类型，并对包含其他类型数据的单元返回 Null 值。 （如果各种数据类型的数量相当，则采用数值类型。）Excel 工作表中大部分单元格格式设置选项不会影响此数据类型判断。

可以通过指定导入模式将所有值导入为文本来修改 Excel 驱动程序的此行为。 若要指定导入模式，请在属性窗口中将 `IMEX=1` 添加到 Excel 连接管理器的连接字符串内的“扩展属性”值中  。 

### <a name="truncated-text"></a>截断的文本

驱动程序在确定 Excel 列是否包含文本数据时，它将基于采样的最长值来选择数据类型（字符串或 memo）。 如果驱动程序没有在其采样的行中发现任何长于 255 个字符的值，那么它会将该列视为 255 个字符的字符串的列而不是 memo 列。 因此，长度超过 255 个字符的值可能会被截断。

要从备注列导入文件但不被截断，有以下两种选择：

-   请确保至少一个示例行中的备注列包含长于 255 个字符的值

-   增加驱动程序采样的行数，以包括这样的行。 可以通过增加以下注册表项下“TypeGuessRows”的值增加采样的行数  ：

| 可再发行组件版本 | 注册表项 |
|---|---|
| Excel 2016 | HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Office\16.0\Access Connectivity Engine\Engines\Excel |
| Excel 2010 | HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Office\14.0\Access Connectivity Engine\Engines\Excel |
| | |

## <a name="issues-with-exporting"></a><a name="issues-exporting"></a>导出的问题

### <a name="create-a-new-destination-file"></a>创建新的目标文件

#### <a name="in-ssis"></a>在 SSIS 中

使用想要创建的新 Excel 文件的路径和文件名称创建 Excel 连接管理器。 然后，在“Excel 目标编辑器”，针对“Excel 工作表名称”，选择“新建”以创建目标工作表    。 此时，SSIS 创建具有指定工作表的新 Excel 文件。

#### <a name="in-the-sql-server-import-and-export-wizard"></a>在 SQL Server 导入和导出向导中

在“选择目标”页上，选择“浏览”   。 在“打开”对话框中，导航到想要在其中创建新 Excel 文件的文件夹，提供新文件名称，然后选择“打开”   。

### <a name="export-to-a-large-enough-range"></a>导出到足够大的区域

指定某个区域作为目标时，如果该区域具有的列比源数据少，则将收到错误消息  。 但是，如果指定的区域具有的行比源数据少，则向导将继续写入没有错误的行并扩展区域定义以匹配新的行数  。

### <a name="export-long-text-values"></a>导出长文本值

若要将大于 255 个字符的字符串成功地保存到 Excel 列中，驱动程序必须将该目标列的数据类型识别为 **memo** ，而不是 **string**。

-   如果现有目标表已包含数据行，则由驱动程序采样的前几行在备注列中必须包含至少一个值超过 255 个字符的实例。

## <a name="related-content"></a>相关内容

有关本文所述的组件和过程的详细信息，请参阅以下文章：

### <a name="about-ssis"></a>关于 SSIS
[Excel 连接管理器](connection-manager/excel-connection-manager.md)  
[Excel 源](data-flow/excel-source.md)  
[Excel 目标](data-flow/excel-destination.md)  
[使用 Foreach 循环容器循环遍历 Excel 文件和表](control-flow/loop-through-excel-files-and-tables-by-using-a-foreach-loop-container.md)  
[使用脚本任务处理 Excel 文件](extending-packages-scripting-task-examples/working-with-excel-files-with-the-script-task.md)

### <a name="about-the-sql-server-import-and-export-wizard"></a>关于 SQL Server 导入和导出向导
[连接到 Excel 数据源](import-export-data/connect-to-an-excel-data-source-sql-server-import-and-export-wizard.md)  
[导入和导出向导的简单示例入门](import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)

### <a name="other-articles"></a>其他文章
[将 Excel 数据导入 SQL Server 或 Azure SQL 数据库](../relational-databases/import-export/import-data-from-excel-to-sql.md)  
