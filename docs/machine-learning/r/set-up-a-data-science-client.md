---
title: 设置 R 数据科学客户端
description: 在开发工作站上安装本地 R 库和工具，以便远程连接到 SQL Server。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 7f738e20a84c82879361e999ef795825c31bf311
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470768"
---
# <a name="set-up-a-data-science-client-for-r-development-on-sql-server"></a>在 SQL Server 上建立 R 开发的数据科学客户端
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

从 SQL Server 2016 及更高版本开始，如果在 [SQL Server 2016 R Services](../install/sql-r-services-windows-install.md) 或 [SQL Server 机器学习服务（数据库内）](../install/sql-machine-learning-services-windows-install.md)安装中包含 R 语言选项，则可使用 R 集成功能。 

若要为 SQL Server 开发和部署 R 解决方案，请在开发工作站上安装 [Microsoft R Client](/machine-learning-server/r-client/what-is-microsoft-r-client)，以获得 [RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler) 和其他 R 库。 RevoScaleR 库也是远程 SQL Server 实例上所需的项，负责协调两个系统之间的计算请求。 

本文介绍如何配置 R 客户端开发工作站，以便能够与启用了机器学习和 R 集成的远程 SQL Server 进行交互。 完成本文中的步骤后，你将拥有与 SQL Server 上相同的 R 库。 你还将了解如何将计算从本地 R 会话推送到 SQL Server 上的远程 R 会话。

![客户端服务器组件](media/sqlmls-r-client-revo.png "本地与远程 R 会话和库")

若要验证安装，可按本文所述使用内置的“RGUI”工具，也可以[将库链接到](#install-ide)你通常使用的 RStudio 或任何其他 IDE  。

> [!Note]
> 客户端库安装的替代方法是使用[独立服务器](../install/sql-machine-learning-standalone-windows-install.md)作为富客户端，一些客户更喜欢使用它来完成更深入的方案工作。 独立服务器与 SQL Server 完全分离，但由于它具有相同的 R 库，因此可以将它用作 SQL Server 数据库内分析客户端。 此外，还可以用它来完成与 SQL 无关的工作，包括从其他数据平台导入数据和对数据建模。 如果安装独立服务器，可在以下位置找到 R 可执行文件：`C:\Program Files\Microsoft SQL Server\140\R_SERVER`。 若要验证安装，请[打开 R 控制台应用](#R-tools)以使用该位置中的 R.exe 运行命令。

## <a name="commonly-used-tools"></a>常用工具

无论是没接触过 SQL 的 R 开发人员，还是没接触过 R 和数据库内分析的 SQL 开发人员，都需要使用 R 开发工具和 T-SQL 查询编辑器（例如 [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)）来行使数据库内分析的所有功能。

对于简单的 R 开发方案，可以使用 RGUI 可执行文件，该文件捆绑在 MRO 和 SQL Server 的基本 R 分发中。 本文介绍如何将 RGUI 用于本地和远程 R 会话。 为了提高工作效率，应使用功能齐全的 IDE，如 [RStudio 或 Visual Studio](#install-ide)。

SSMS 需单独下载，它用于在 SQL Server 上创建和运行存储过程，其中包括包含 R 代码的存储过程。 在开发环境中编写的几乎所有 R 代码都可以嵌入到存储过程中。 你可以逐步完成其他教程，以便了解 [SSMS 和嵌入式 R](../tutorials/r-taxi-classification-introduction.md)。

## <a name="1---install-r-packages"></a>1 - 安装 R 包

Microsoft 的 R 包提供多种产品和服务。 建议在本地工作站上安装 Microsoft R Client。 R Client 提供了 [RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler)[MicrosoftML](/machine-learning-server/r-reference/microsoftml/microsoftml-package)[SQLRUtils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) 和其他 R 包。

1. [下载 Microsoft R Client](https://aka.ms/rclient/download)。

2. 在安装向导中，接受或更改默认安装路径，接受或更改组件列表，并接受 Microsoft R Client 许可条款。

   安装完成后，欢迎屏幕将向你介绍产品和文档。

3. 创建 MKL_CBWR 系统环境变量，以确保从 Intel 数学核心函数库 (MKL) 计算得到一致的输出结果。

   + 在“控制面板”中，单击“系统和安全” > “系统” > “高级系统设置” > “环境变量”     。
   + 创建名为 MKL_CBWR 的新系统变量，并将值设置为“AUTO”   。

## <a name="2---locate-executables"></a>2 - 查找可执行文件

找到并列出安装文件夹的内容，确认已安装 R.exe、RGUI 和其他包。 

1. 在“文件资源管理器”中，打开 C:\Program Files\Microsoft\R Client\R_SERVER\bin 文件夹以确认 R.exe 的位置。

2. 打开 x64 子文件夹以确认“RGUI”  。 你将在下一个步骤中使用此工具。

3. 打开 C:\Program Files\Microsoft\R Client\R_SERVER\library 以查看与 R 客户端一起安装的包列表，包括 RevoScaleR、MicrosoftML 和其他包。


<a name="R-tools"></a>
 
## <a name="3---start-rgui"></a>3 - 启动 RGUI

在 SQL Server 中安装 R 时，将获得与 R 的任何基本安装（如 RGui、Rterm 等）标准相同的 R 工具。 这些工具是轻型的，可用于检查包和库信息、运行即席命令或脚本或逐步学习教程。 可以使用这些工具获取 R 版本信息并确认连接。

1. 打开 C:\Program Files\Microsoft\R Client\R_SERVER\bin\x64 并双击 RGui 以使用 R 命令提示符启动 R 会话  。

   从 Microsoft 程序文件夹启动 R 会话时，会自动加载多个包，包括 RevoScaleR。 

2. 在命令提示符下输入 `print(Revo.version)` 以返回 RevoScaleR 包版本信息。 应为 9.2.1 或 9.3.0 版的 RevoScaleR。

3. 在 R 提示符下输入 search()，获取已安装包的列表  。

   ![加载 R 时的版本信息](../install/media/rclient-rgui-r-prompt.png "打开 R 提示符")


## <a name="4---get-sql-permissions"></a>4 - 获取 SQL 权限

在 R Client 中，R 处理限制在两个线程和内存中数据中。 对于使用多个核心和大型数据集的可缩放处理，可以将执行（称为计算上下文）转移到远程 SQL Server 实例的数据集和计算能力  。 这是与生产 SQL Server 实例进行客户端集成的推荐方法，你将需要权限和连接信息才能有效运行。

若要连接到 SQL Server 实例以运行脚本和上传数据，必须在数据库服务器上具有有效的登录名。 可以使用 SQL 登录凭据或集成 Windows 身份验证信息。 我们通常建议你使用 Windows 集成身份验证，但在某些情况下使用 SQL 登录更为简单，尤其是当脚本包含到外部数据的连接字符串时。

用于运行代码的帐户至少必须具有从正在使用的数据库进行读取的权限，以及具有特殊权限 EXECUTE ANY EXTERNAL SCRIPT。 大多数开发人员还需要有创建存储过程的权限，以及将数据写入包含训练数据或评分数据的表中的权限。 

让数据库管理员在使用 R 的数据库中[为你的帐户配置以下权限](../security/user-permission.md)：

+ EXECUTE ANY EXTERNAL SCRIPT，以便在服务器上运行 R 脚本  。
+ **db_datareader** 特权，以便运行用于训练模型的查询。
+ **db_datawriter**，以便写入训练数据或评分数据。
+ **db_owner**，以便创建存储过程、表和函数等对象。 
  你还需要使用 **db_owner** 来创建示例数据库和测试数据库。 

如果你的代码需要使用默认情况下未随 SQL Server 安装的包，请与数据库管理员联系，将这些包随实例一起安装。 SQL Server 是一种受保护的环境，对包的安装位置有一些限制。 有关详细信息，请参阅 [在 SQL Server 上安装新的 R 包](../package-management/install-additional-r-packages-on-sql-server.md)。

## <a name="5---test-connections"></a>5 - 测试连接

作为验证步骤，请使用 RGUI 和 RevoScaleR 确认与远程服务器的连接  。 必须为[远程连接](../../database-engine/configure-windows/view-or-configure-remote-server-connection-options-sql-server.md)启用 SQL Server，且必须具有权限，包括用户登录名和要连接的数据库。 

以下步骤假定演示数据库 [NYCTaxi_Sample](../tutorials/demo-data-nyctaxi-in-sql.md) 和 Windows 身份验证。

1. 在客户端工作站上打开“RGUI”  。 例如，转到 `~\Program Files\Microsoft SQL Server\140\R_SERVER\bin\x64` 并双击“RGui.exe”以启动它  。

2. RevoScaleR 会自动加载。 通过运行以下命令来确认 RevoScaleR 是否可运行：`print(Revo.version)`

3. 输入在远程服务器上执行的演示脚本。 必须修改以下示例脚本，使其包含远程 SQL Server 实例的有效名称。 此会话以本地会话开始，但 rxSummary 函数在远程 SQL Server 实例上执行  。

   ```R
   # Define a connection. Replace server with a valid server name.
   connStr <- "Driver=SQL Server;Server=<your-server-name>;Database=NYCTaxi_Sample;Trusted_Connection=true"
  
   # Specify the input data in a SQL query.
   sampleQuery <-"SELECT DISTINCT TOP(100) tip_amount FROM [dbo].nyctaxi_sample ORDER BY tip_amount DESC;"
  
   # Define a remote compute context based on the remote server.
   cc <-RxInSqlServer(connectionString=connStr)

   # Execute the function using the remote compute context.
   rxSummary(formula = ~ ., data = RxSqlServerData(sqlQuery=sampleQuery, connectionString=connStr), computeContext=cc)
   ```

   **结果：**

   此脚本连接到远程服务器上的数据库，提供查询，创建用于远程代码执行的计算上下文 `cc` 指令，然后提供 RevoScaleR 函数 rxSummary 以返回查询结果的统计摘要  。

   ```R
     Call:
   rxSummary(formula = ~., data = RxSqlServerData(sqlQuery = sampleQuery, 
       connectionString = connStr), computeContext = cc)

   Summary Statistics Results for: ~.
   Data: RxSqlServerData(sqlQuery = sampleQuery, connectionString = connStr) (RxSqlServerData Data Source)
   Number of valid observations: 100 
  
   Name       Mean   StdDev   Min Max ValidObs MissingObs
   tip_amount 63.245 31.61087 36  180 100      0     
   ```

4. 获取并设置计算上下文。 设置好计算上下文后，它将在会话期间保持有效。 如果不确定计算是本地的还是远程的，请运行以下命令来查明。指定连接字符串的结果表示远程计算上下文。

   ```R
   # Return the current compute context.
   rxGetComputeContext()

   # Revert to a local compute context.
   rxSetComputeContext("local")
   rxGetComputeContext()

   # Switch back to remote.
   connStr <- "Driver=SQL Server;Server=<your-server-name>;Database=NYCTaxi_Sample;Trusted_Connection=true"
   cc <-RxInSqlServer(connectionString=connStr)
   rxSetComputeContext(cc)
   rxGetComputeContext()
   ```  

5. 返回有关数据源中变量的信息，包括名称和类型。

   ```R
   rxGetVarInfo(data = inDataSource)
   ```
   结果包括 23 个变量。


6. 生成散点图以了解两个变量之间是否存在依赖关系。 

   ```R
   # Set the connection string. Substitute a valid server name for the placeholder.
   connStr <- "Driver=SQL Server;Server=<your database name>;Database=NYCTaxi_Sample;Trusted_Connection=true"

   # Specify a query on the nyctaxi_sample table.
   # For variables on each axis, remove nulls. Use a WHERE clause and <> to do this.
   sampleQuery <-"SELECT DISTINCT TOP 100 * from [dbo].[nyctaxi_sample] WHERE fare_amount <> '' AND  tip_amount <> ''"
   cc <-RxInSqlServer(connectionString=connStr)

   # Generate a scatter plot.
   rxLinePlot(fare_amount ~ tip_amount, data = RxSqlServerData(sqlQuery=sampleQuery, connectionString=connStr, computeContext=cc), type="p")
   ```

   下面的屏幕截图显示了输入和散点图输出。

   ![RGUI 中的散点图](media/rclient-setup-scatterplot.png "NYC 出租车演示数据的散点图")

<a name="install-ide"></a>

## <a name="6---link-tools-to-rexe"></a>6 - 将工具链接到 R.exe

对于持续且重大的开发项目，应安装集成开发环境 (IDE)。 SQL Server 工具和内置的 R 工具不适用于大型 R 开发。 有了工作代码后，就可以将其部署为存储过程，以便在 SQL Server 上执行。

将 IDE 指向本地 R 库：基本 R、RevoScaleR 等。 在脚本执行期间，当脚本调用 SQL Server 上的远程计算上下文、访问该服务器上的数据和操作时，会在远程 SQL Server 上运行工作负载。

### <a name="rstudio"></a>RStudio

使用 [RStudio](https://www.rstudio.com/) 时，可以将环境配置为使用与远程 SQL Server 上的库和可执行文件相对应的 R 库和可执行文件。

1. 检查 SQL Server 上安装的 R 包版本。 有关详细信息，请参阅[获取 R 包信息](../package-management/r-package-information.md)。

1. 安装 Microsoft R Client 或其中一个独立服务器选项以添加 RevoScaleR 和其他 R 包，包括 SQL Server 实例使用的基本 R 分发。 选择与服务器上提供相同包版本的同一级别或更低版本（包向后兼容）。 有关版本信息，请参阅本文中的版本映射：[升级 R 和 Python 组件](../install/upgrade-r-and-python.md)。

1. 在 RStudio 中，[更新 R 路径](https://support.rstudio.com/hc/articles/200486138-Using-Different-Versions-of-R)以指向提供 RevoScaleR、Microsoft R Open 和其他 Microsoft 包的 R 环境。 

   + 对于 R Client 安装，请查找 C:\Program Files\Microsoft\R Client\R_SERVER\bin\x64
   + 对于独立服务器，请查找 C:\Program Files\Microsoft SQL Server\140\R_SERVER\Library 或 C:\Program Files\Microsoft SQL Server\130\R_SERVER\Library

1. 关闭，然后再打开 RStudio。

重新打开 RStudio 时，来自 R Client（或独立服务器）的 R 可执行文件是默认的 R 引擎。


### <a name="r-tools-for-visual-studio-rtvs"></a>针对 Visual Studio 的 R 工具 (RTVS)

如果还没有适用于 R 的首选 IDE，建议使用针对 Visual Studio 的 R 工具  。

+ [下载针对 Visual Studio 的 R 工具 (RTVS)](https://marketplace.visualstudio.com/items?itemName=MikhailArkhipov007.RTVS2019)
+ [安装说明](/visualstudio/rtvs/installing-r-tools-for-visual-studio) - RTVS 在 Visual Studio 的多个版本中均可用。
+ [针对 Visual Studio 的 R 工具入门](/visualstudio/rtvs/getting-started-with-r)

### <a name="connect-to-sql-server-from-rtvs"></a>从 RTVS 连接到 SQL Server

此示例使用安装了数据科学工作负载的 Visual Studio 2017 社区版。

1. 从“文件”菜单中，选择“新建”，然后选择“项目”    。

2. 左侧窗格包含预安装模板的列表。 单击“R”，然后选择“R 项目”   。 在“名称”框中，键入 `dbtest`，然后单击“确定”   。 

   Visual Studio 会创建一个新的项目文件夹和一个默认的脚本文件 `Script.R`。 

3. 在脚本文件的第一行键入 `.libPaths()`，然后按 Ctrl+Enter。

   当前 R 库路径应显示在“R 交互窗口”中  。 

4. 单击“R 工具”菜单并选择“Windows”以查看可以在工作区中显示的其他 R 特定窗口的列表   。
 
   + 按 Ctrl+3，查看当前库中包的帮助。
   + 按 Ctrl+8，查看“变量资源管理器”中的 R 变量  。

## <a name="next-steps"></a>后续步骤

两个不同的教程都包含这些练习，以便你可以练习将计算上下文从本地切换到远程 SQL Server 实例。

+ [教程：对 SQL Server 数据使用 RevoScaleR R 函数](../tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)
+ [数据科学端到端演练](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)