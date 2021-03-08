---
title: sqlrutils 帮助程序函数
description: Sqlrutils 是 Microsoft 的一种 R 包，为 R 用户提供一种机制，将他们的 R 脚本置于 T-SQL 存储过程中、向数据库注册该存储过程并从 R 开发环境运行存储过程。 该包在 SQL Server 机器学习服务和 SQL Server 2016 R Services 中提供。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 67e14f965889cbbeecf2a9800d49b70622b88502
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186384"
---
# <a name="sqlrutils-r-package-in-sql-server-machine-learning-services"></a>Sqlrutils（SQL Server 机器学习服务中的 R 包）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Sqlrutils 是 Microsoft 的一种 R 包，为 R 用户提供一种机制，将他们的 R 脚本置于 T-SQL 存储过程中、向数据库注册该存储过程并从 R 开发环境运行存储过程。 该包在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)和 [SQL Server 2016 R Services](sql-server-r-services.md) 中提供。

通过转换 R 代码以在单个存储过程中运行，可更有效地利用 SQL Server R Services，它要求将 R 脚本作为参数嵌入 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)。 **sqlrutils** 包有助于构建此嵌入的 R 脚本并正确设置相关参数。

**Sqlrutils** 包执行以下任务：

- 将生成的 T-SQL 脚本保存为 R 数据结构内的字符串
- 可以选择为 T-SQL 脚本生成 .sql 文件，可编辑或运行此文件来创建存储过程
- 从 R 开发环境向 SQL Server 实例注册新创建的存储过程

也可通过传递格式正确的参数并处理结果，从 R 环境执行存储过程。 或者可使用 SQL Server 中的存储过程来支持常见数据库集成方案，例如 ETL、模型定型和大批量评分。

> [!NOTE]
> 若打算通过调用 *executeStoredProcedure* 函数从 R 环境运行存储过程，必须使用 ODBC 3.8 提供程序，例如适用于 SQL Server 的 ODBC Driver 13。  
  
## <a name="full-reference-documentation"></a>完整参考文档

Sqlrutils 包分布于多种 Microsoft 产品中，但不管你是在 SQL Server 还是在其他产品中获取该包，用法都是一样的。 由于函数相同，因此单个 sqlrutils 函数的文档仅发布到 Microsoft Machine Learning Server 的 [sqlrutils 包引用](/machine-learning-server/r-reference/sqlrutils/sqlrutils)下的一个位置。 如果存在任何特定于产品的行为，这些差异将在函数帮助页中注明。

## <a name="functions-list"></a>函数列表

下一节概述可从 sqlrutils 包中调用以开发包含嵌入式 R 代码的存储过程的函数。 有关每个方法或函数的参数的详细信息，请参阅包的 R 帮助：`help(package="sqlrutils")`

|函数 | 描述 |
|------|-------------|
|[executeStoredProcedure](/machine-learning-server/r-reference/sqlrutils/executestoredprocedure)| 执行 SQL 存储过程。|
|[getInputParameters](/machine-learning-server/r-reference/sqlrutils/getinputparameters)| 获取存储过程的输入参数列表。| 
|[InputData](/machine-learning-server/r-reference/sqlrutils/inputdata)| 在 SQL Server 中定义将用于 R 数据帧的数据源。 指定 data.frame（在其中存储输入数据）的名称和获取数据的查询或默认值。 仅支持简单的 SELECT 查询。 | 
|[InputParameter](/machine-learning-server/r-reference/sqlrutils/inputparameter)| 定义将嵌入 T-SQL 脚本的单个输入参数。 必须提供参数名称及其 R 数据类型。| 
|[OutputData](/machine-learning-server/r-reference/sqlrutils/outputdata)| 如果 R 函数返回包含 data.frame 的列表，则会生成所需的中间数据对象。 *OutputData* 对象用于存储从列表获取的单个 data.frame 的名称。| 
|[OutputParameter](/machine-learning-server/r-reference/sqlrutils/outputparameter) | 如果 R 函数返回列表，则会生成所需的中间数据对象。 *OutputParameter* 对象存储列表中单个成员的名称和数据类型，假定此成员 **不是** 数据帧。 |
|[registerStoredProcedure](/machine-learning-server/r-reference/sqlrutils/registerstoredprocedure) | 将存储过程注册到数据库中。|
|[setInputDataQuery](/machine-learning-server/r-reference/sqlrutils/setinputdataquery)| 将查询分配给存储过程的输入数据参数。| 
|[setInputParameterValue](/machine-learning-server/r-reference/sqlrutils/setinputparametervalue)| 将值分配给存储过程的输入参数。| 
|[StoredProcedure](/machine-learning-server/r-reference/sqlrutils/storedprocedure)| 存储过程对象。|


## <a name="how-to-use-sqlrutils"></a>如何使用 sqlrutils

sqlrutils 包函数必须在具有 R 的 SQL Server 机器学习的计算机上运行。如果正在使用客户端工作站，请设置远程计算上下文以将执行转移到 SQL Server。 使用此包的工作流包括以下步骤：

+ 定义存储过程参数（输入、输出或两者） 
+ 生成并注册存储过程    
+ 执行该存储过程  

在 R 会话中，通过键入 `library(sqlrutils)`，从命令行加载 sqlrutils。

> [!Note]
> 如果将计算上下文更改为 SQL Server 并在该计算上下文中执行代码，则可以在不具有 SQL Server 的计算机上（例如，在 R 客户端实例上）加载此包。


### <a name="define-stored-procedure-parameters-and-inputs"></a>定义存储过程参数和输入

`StoredProcedure` 是用于构建存储过程的主构造函数。 此构造函数生成 *SQL Server 存储过程* 对象，并选择性地创建包含查询的文本文件，其中此查询可使用 T-SQL 命令生成存储过程。 

或者， *StoredProcedure* 函数也可向指定的实例和数据库注册存储过程。

+ 使用 `func` 参数指定有效的 R 函数。 该函数使用的所有变量必须在函数中定义或作为输入参数提供。 这些参数最多可包含一个数据帧。

+ R 函数必须返回数据帧、已命名列表或 NULL。 如果该函数返回列表，则列表最多可以包含一个 data.frame。

+ 使用参数 `spName` 指定要创建的存储过程的名称。

+ 可通过使用以下帮助器函数创建的对象，在可选输入和输出参数中传递： `setInputData`、 `setInputParameter`和 `setOutputParameter`。

+  可选择使用 `filePath` 提供要创建的 .sql 文件的路径和名称。 可在 SQL Server 实例上运行此文件，使用 T-SQL 生成存储过程。

+ 若要定义将保存存储过程的服务器和数据库，可使用参数 `dbName` 和  `connectionString`。

+ 若要获取用于创建特定 *StoredProcedure* 对象的 *InputData* 和 *InputParameter* 对象的列表，可调用 `getInputParameters`。 

+ 若要向指定数据库注册存储过程，可使用 `registerStoredProcedure`。

存储过程对象通常没有任何关联数据或值，除非指定了默认值。 执行存储过程之前，不会检索数据。 

### <a name="specify-inputs-and-execute"></a>指定输入并执行

+ 使用 `setInputDataQuery` 将查询分配到 *InputParameter* 对象。 例如，如果已在 R 中创建存储过程对象，可使用 `setInputDataQuery` 将参数传递到 *StoredProcedure* ，以使用所需输入执行存储过程。

+ 使用 `setInputValue` 将特定值分配到存储为 *InputParameter* 对象的参数。 然后将参数对象及其值分配传递到 *StoredProcedure* ，以使用设定值执行存储过程。

+ 使用 `executeStoredProcedure` 执行定义为 *StoredProcedure* 对象的存储过程。 仅当从 R 代码执行存储过程时，调用此函数。 使用 T-SQL 从 SQL Server 运行存储过程时，请勿使用它。

> [!NOTE]
> *executeStoredProcedure* 函数需要 ODBC 3.8 提供程序，例如适用于 SQL Server 的 ODBC Driver 13。  

## <a name="see-also"></a>另请参阅

[如何使用 sqlrutils 创建存储过程](/machine-learning-server/r-reference/sqlrutils/how-to-create-a-stored-procedure-from-r)