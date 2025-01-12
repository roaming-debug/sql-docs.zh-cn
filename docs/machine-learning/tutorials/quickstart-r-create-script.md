---
title: 快速入门：运行 R 脚本
titleSuffix: SQL machine learning
description: 通过 SQL 机器学习运行一组简单的 R 脚本。 了解如何使用存储过程 sp_execute_external_script 执行该脚本。
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: eaecfcf9cd4944d48099a5bf1582e8144c1f62e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272988"
---
# <a name="quickstart-run-simple-r-scripts-with-sql-machine-learning"></a>快速入门：通过 SQL 机器学习运行简单的 R 脚本
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
在本快速入门中，你将在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中或[大数据群集](../../big-data-cluster/machine-learning-services.md)上运行一组简单的 R 脚本。 你将了解如何在 SQL Server 实例中使用存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 执行该脚本。
::: moniker-end
::: moniker range="=sql-server-2017"
在本快速入门中，将使用 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)运行一组简单的 R 脚本。 你将了解如何在 SQL Server 实例中使用存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 执行该脚本。
::: moniker-end
::: moniker range="=sql-server-2016"
在本快速入门中，你将使用 [SQL Server R Services](../r/sql-server-r-services.md) 运行一组简单的 R 脚本。 你将了解如何在 SQL Server 实例中使用存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 执行该脚本。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
在本快速入门中，将使用 [Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)运行一组简单的 R 脚本。 你将了解如何在数据库中使用存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 执行该脚本。
::: moniker-end

## <a name="prerequisites"></a>先决条件

若要运行本快速入门，需要具备以下先决条件。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
- SQL Server 机器学习服务。 如需安装机器学习服务，请参阅 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)或 [Linux 安装指南](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json)。 还可以[启用 SQL Server 大数据群集上的机器学习服务](../../big-data-cluster/machine-learning-services.md)。
::: moniker-end
::: moniker range="=sql-server-2017"
- SQL Server 机器学习服务。 如需安装机器学习服务，请参阅 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)。 
::: moniker-end
::: moniker range="=sql-server-2016"
- SQL Server 2016 R Services。 如需安装 R Services，请参阅 [Windows 安装指南](../install/sql-r-services-windows-install.md)。 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
- Azure SQL 托管实例机器学习服务。 有关信息，请参阅 [Azure SQL 托管实例机器学习服务概述](/azure/azure-sql/managed-instance/machine-learning-services-overview)。
::: moniker-end

- 一个用于运行包含 R 脚本的 SQL 查询的工具。 本快速入门使用 [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md)。

## <a name="run-a-simple-script"></a>运行简单脚本

若要运行 R 脚本，请将它作为参数传递给系统存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)。 此系统存储过程启动 R 运行时，将数据传递给 R，安全地管理 R 用户会话并将任何结果返回给客户端。

在下面的步骤中，你将运行此示例 R 脚本：

```r
a <- 1
b <- 2
c <- a/b
d <- a*b
print(c(c, d))
```

1. 打开 **Azure Data Studio** 并连接到你的服务器。

1. 将完整的 R 脚本传递到 `sp_execute_external_script` 存储过程。

   通过 `@script` 参数传递脚本。 `@script` 参数内的所有内容都必须是有效的 R 代码。

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

1. 计算出正确的结果，R `print` 函数将结果返回到“消息”窗口。

   结果应该如下所示。

    **结果**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>运行 Hello World 脚本

典型的示例脚本只输出字符串“Hello World”。 运行以下命令。

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'OutputDataSet<-InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

`sp_execute_external_script` 存储过程的输入包括：

| 输入 | 说明 |
|-|-|
| @language | 定义本例中要调用 R 的语言扩展 |
| @script | 定义传递给 R 运行时的命令。 必须将整个 R 脚本作为 Unicode 文本包括在此参数中。 还可将文本添加到 nvarchar 类型的变量并调用该变量 |
| @input_data_1 | 查询返回的数据传递给 R 运行时，后者将数据以数据帧的形式返回 |
|WITH RESULT SETS | 子句定义返回的数据表的架构，并将“Hello World”添加为列名称且 int 为数据类型 |

命令输出以下文本：

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>使用输入和输出

默认情况下，`sp_execute_external_script` 接受单个数据集作为输入，通常以有效的 SQL 查询的形式提供。 然后，它返回单个 R 数据帧作为输出。

现在，使用 `sp_execute_external_script` 的默认输入和输出变量：InputDataSet 和 OutputDataSet 。

1. 创建一个小型测试数据表。

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    
    INSERT INTO RTestData
    VALUES (1);
    
    INSERT INTO RTestData
    VALUES (10);
    
    INSERT INTO RTestData
    VALUES (100);
    GO
    ```

1. 使用 `SELECT` 语句来查询表。
  
    ```sql
    SELECT *
    FROM RTestData
    ```

    **结果**

    ![RTestData 表的内容](./media/select-rtestdata.png)

1. 运行以下 R 脚本。 它使用 `SELECT` 语句从表中检索数据，通过 R 运行时传递数据，并以数据帧的形式返回数据。 `WITH RESULT SETS` 子句为 SQL 定义返回的数据表的架构，并添加了列名称“NewColName”。

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **结果**

    ![从表返回数据的 R 脚本的输出](./media/r-output-rtestdata.png)

1. 现在，更改输入变量和输出变量的名称。 默认的输入和输出变量名称是 InputDataSet 和 OutputDataSet，而此脚本会将名称更改为 SQL_in 和 SQL_out   ：

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N' SQL_out <- SQL_in;'
        , @input_data_1 = N' SELECT 12 as Col;'
        , @input_data_1_name = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    请注意 R 区分大小写。 R 脚本中使用的输入和输出变量（SQL_out、SQL_in）需要匹配使用 `@input_data_1_name` 和 `@output_data_1_name` 定义的名称，包括大小写 。

   > [!TIP]
   > 只能将一个输入数据集作为参数传递，并且只能返回一个数据集。 但是，可以从 R 代码内部调用其他数据集，并且可以返回除数据集之外的其他类型的输出。 也可向任何参数添加 OUTPUT 关键字，让该参数随结果一起返回。

1. 还可以仅使用没有输入数据的 R 脚本（`@input_data_1` 设置为空白）生成值。

   以下脚本输出文本“hello”和“world”。

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    mytextvariable <- c("hello", " ", "world");
    OutputDataSet <- as.data.frame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **结果**

    ![使用 @script 作为输入的查询结果](./media/r-data-generated-output.png)

## <a name="check-r-version"></a>检查 R 版本

若要查看已安装的 R 的版本，请运行以下脚本。

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(version)';
GO
```

R `print` 函数将该版本返回到“消息”窗口。 在下面的示例输出中，可以看到本例中安装的是 R 版本 3.4.4。

**结果**

```text
STDOUT message(s) from external script:
                   _
platform       x86_64-w64-mingw32
arch           x86_64
os             mingw32
system         x86_64, mingw32
status
major          3
minor          4.4
year           2018
month          03
day            15
svn rev        74408
language       R
version.string R version 3.4.4 (2018-03-15)
nickname       Someone to Lean On
```

## <a name="list-r-packages"></a>列出 R 包
::: moniker range=">=sql-server-2017"
Microsoft 提供了许多随机器学习服务预安装的 R 包。
::: moniker-end
::: moniker range="=sql-server-2016"
Microsoft 提供了许多随 R Services 预安装的 R 包。
::: moniker-end

若要查看已安装的 R 包列表（包括版本、依赖项、许可证和库路径的信息），请运行以下脚本。

```SQL
EXEC sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            , LibPath NVARCHAR(2000)
            ));
```

输出来自 R 中的 `installed.packages()`，并作为结果集返回。

**结果**

![R 中安装的包](./media/rsql-installed-packages.png) 

## <a name="next-steps"></a>后续步骤

若要了解在通过 SQL 机器学习使用 R 时如何使用数据结构，请按照此快速入门操作：

> [!div class="nextstepaction"]
> [通过 SQL 机器学习使用 R 时处理数据类型和对象](quickstart-r-data-types-and-objects.md)