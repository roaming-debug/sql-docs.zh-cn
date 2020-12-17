---
title: 通过使用 R 代码分析函数提高性能
description: 通过使用 R 分析函数收集有用的信息以提高性能并在 SQL Server 上更快获得 R 计算结果。 “rprof”函数收集并返回有关内部函数调用的信息。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 53c3e7f55fa21d033bfda3f2e660bd1b9a92991f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470738"
---
# <a name="use-r-code-profiling-functions-to-improve-performance"></a>使用 R 代码分析函数提高性能
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文介绍 R 包提供的性能工具，这些工具可获取有关内部函数调用的信息。 你可以使用此信息来提高代码的性能。

> [!TIP]
> 本文提供入门的基本资源。 要查看专家指导，建议参阅[由 Hadley Wickham 编写的《Advanced R》](http://adv-r.had.co.nz)中的“性能”部分。

## <a name="using-rprof"></a>使用 RPROF

[rprof](https://www.rdocumentation.org/packages/utils/versions/3.5.1/topics/Rprof) 是默认加载的基础包 [utils](https://www.rdocumentation.org/packages/utils/versions/3.5.1) 中的一个函数。 

通常，*rprof* 函数会以指定的间隔将调用堆栈写入到一个文件中。 然后，可以使用 [summaryRprof](https://www.rdocumentation.org/packages/utils/versions/3.5.1/topics/summaryRprof) 函数来处理输出文件。 *rprof* 的一个优点是它会执行采样，从而降低监视对性能造成的影响。

要在代码中使用 R 分析，请调用此函数并指定其参数，包括将写入的日志文件的位置名称。 可以在你的代码中打开或关闭分析。 以下语法说明了基本用法： 

```R
# Specify profiling output file.
varOutputFile <- "C:/TEMP/run001.log")
Rprof(varOutputFile)

# Turn off profiling
Rprof(NULL)
    
# Restart profiling
Rprof(append=TRUE)
```

> [!NOTE]
> 若要使用此函数，需要在运行代码的计算机上安装 Windows Perl。 因此，建议你在开发过程中在 R 环境中分析代码，然后将调试后的代码部署到 SQL Server。  


## <a name="r-system-functions"></a>R 系统函数

R 语言包括了许多用于返回系统函数的内容的基础包。 例如，你可以在 R 代码中使用 `Sys.timezone` 来获取当前时区，或者使用 `Sys.Time` 来从 R 获取系统时间。 

若要获取有关各个 R 系统函数的信息，请在 R 命令提示符下键入函数名称作为 R `help()` 函数的参数。

```R
help("Sys.time")
```

## <a name="debugging-and-profiling-in-r"></a>在 R 中进行调试和分析

默认情况下安装的 Microsoft R Open 的文档包括了一个有关为 R 语言开发扩展的手册，其中详细讨论了[分析和调试](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Debugging)。

## <a name="next-steps"></a>后续步骤

+ 有关在 SQL Server 中优化 R 脚本的详细信息，请参阅 [R 的性能优化和数据优化](r-and-data-optimization-r-services.md)。
+ 有关在 SQL Server 中进行性能优化的详细信息，请参阅 [SQL Server 数据库引擎和 Azure SQL 数据库的性能中心](/sql/relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database)。
+ 有关 utils 包的详细信息，请参阅 [R Utils 包](https://www.rdocumentation.org/packages/utils/versions/3.5.1)。
+ 有关 R 编程的深入讨论，请参阅 [Hadley Wickham 编著的“高级 R”](http://adv-r.had.co.nz)。
