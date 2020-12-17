---
title: osql 实用工具
description: 在 SQL Server 中，使用 osql 实用工具可以输入 Transact-SQL 语句、系统过程和脚本文件。 osql 实用工具通过 ODBC 与服务器通信。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- statements [SQL Server], command prompt
- QUIT command
- Transact-SQL statements, command prompt
- EXIT command
- operating systems [SQL Server], commands
- osql utility [SQL Server]
- stored procedures [SQL Server], command prompt
- scripts [SQL Server], command prompt
- RESET command
- GO command
- command prompt utilities [SQL Server], osql
- CTRL+C command
ms.assetid: cf530d9e-0609-4528-8975-ab8e08e40b9a
author: markingmyname
ms.author: maghan
ms.manageR: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/16/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 29f495dda754d3456e8d8df8811620548426cd89
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478438"
---
# <a name="osql-utility"></a>osql 实用工具

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

使用 **osql** 实用工具可以输入 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句、系统过程和脚本文件。 此实用工具通过 ODBC 与服务器通信。  
  
> [!IMPORTANT]  
>  在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的未来版本中将删除此功能。 请避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 改用 **sqlcmd** 。 有关详细信息，请参阅 [sqlcmd Utility](../tools/sqlcmd-utility.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
osql  
[-?] |  
[-L] |  
[  
  {  
     {-Ulogin_id [-Ppassword]} | -E }  
     [-Sserver_name[\instance_name]] [-Hwksta_name] [-ddb_name]  
     [-ltime_out] [-ttime_out] [-hheaders]  
     [-scol_separator] [-wcolumn_width] [-apacket_size]  
     [-e] [-I] [-D data_source_name]  
     [-ccmd_end] [-q "query"] [-Q"query"]  
     [-n] [-merror_level] [-r {0 | 1}]  
     [-iinput_file] [-ooutput_file] [-p]  
     [-b] [-u] [-R] [-O]  
]  
```  
  
## <a name="arguments"></a>参数  
 **-?**  
 显示 **osql** 开关的语法摘要。  
  
 **-L**  
 列出在本地配置的服务器和在网络上广播的服务器的名称。  
  
> [!NOTE]  
>  鉴于网络上广播的特点， **osql** 可能不会及时接收来自所有服务器的响应。 因此，每次调用该选项所返回的服务器列表都可能不同。  
  
 **-U** _login_id_  
 用户登录 ID。 登录 ID 区分大小写。  
  
 **-P** _password_  
 用户指定的密码。 如果未使用 **-P** 选项， **osql** 将提示输入密码。 如果在命令提示符的末尾使用 **-P** 选项而不提供密码， **osql** 将使用默认密码 (NULL)。  
  
> [!IMPORTANT]  
>  不要使用空密码。 请使用强密码。 有关详细信息，请参阅 [Strong Passwords](../relational-databases/security/strong-passwords.md)。  
  
 密码是区分大小写的。  
  
 使用 OSQLPASSWORD 环境变量，可以为当前会话设置默认密码。 因此，不需要通过硬编码在批处理文件中设置密码。  
  
 如果不使用 **-P** 选项指定密码， **osql** 将首先检查 OSQLPASSWORD 变量。 如果未设置任何值，则 **osql** 将使用默认密码 (NULL)。 以下示例将在命令提示符中设置 OSQLPASSWORD 变量，然后访问 **osql** 实用工具：  
  
```  
C:\>SET OSQLPASSWORD=abracadabra  
C:\>osql   
```  
  
> [!IMPORTANT]  
>  若要屏蔽密码，请不要指定 **-P** 选项和 **-U** 选项。 相反，应在指定 **osql** 和 **-U** 选项以及其他开关（不指定 **-P**）之后，按“Enter”键，此时 **osql** 将提示你输入密码。 这种方法可以确保输入密码时对其屏蔽。  
  
 **-E**  
 使用可信连接而不请求密码。  
  
 **-S** _server\_name_[ **\\** _instance\_name_]  
 指定要连接到的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例。 指定要连接到该服务器上 *默认实例的* server_name [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 。 指定要连接到该服务器上 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的命名实例的 _server\_name_\\_instance\_name_。 如果未指定服务器， **osql** 将连接到本地计算机上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 默认实例。 从网络上的远程计算机执行 **osql** 时，此选项是必需的。  
  
 **-H** _wksta_name_  
 工作站的名称。 工作站名称存储在 **sysprocesses.hostname** 中，并由 **sp_who** 显示。 如果不指定此选项，则采用当前计算机名称。  
  
 **-d** _db_name_  
 启动 *osql* 时发出一个 USE **db_name** 语句。  
  
 **-l** _time_out_  
 指定 **osql** 登录超时之前的秒数。登录到 **osql** 的默认超时时间为 8 秒。  
  
 **-t** _time_out_  
 指定命令超时之前的秒数。如果未指定 *time_out* 值，则命令将不会超时。  
  
 **-h** _headers_  
 指定要在列标题之间打印的行数。 默认为每一组查询结果输出一次标题。 使用 -1 可指定不打印标题。 如果使用的是 -1，参数和设置之间就不得有空格（可以是 -h-1，但不能是 -h -1）。  
  
 **-s** _col_separator_  
 指定列分隔符字符，默认值为空格。 若要使用对操作系统有特殊含义的字符（例如 | ; & < >），请将该字符用双引号 (") 括起来。  
  
 **-w** _column_width_  
 允许用户设置屏幕输出的宽度。 默认为 80 个字符。 当输出行达到其最大屏幕宽度时，会拆分为多行。  
  
 **-a** _packet_size_  
 允许您请求不同大小的数据包。 *packet_size* 的有效值介于 512 和 65535 之间。 默认值 **osql** 是服务器默认值。 执行较大的脚本时，各个 GO 命令之间的 SQL 语句的数量是庞大的，因此增大数据包可以提高性能。 [!INCLUDE[msCoName](../includes/msconame-md.md)] 的测试表明大容量复制操作的最快设置通常为 8192。 可以请求更大的数据包，但如果请求不能得到批准，则 **osql** 会将此值默认为服务器的默认值。  
  
 **-e**  
 回显输入。  
  
 **-I**  
 将 QUOTED_IDENTIFIER 连接选项设置为开启。  
  
 **-D** _data_source_name_  
 连接到某个通过用于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的 ODBC 驱动程序定义的 ODBC 数据源。 **osql** 连接使用该数据源中指定的选项。  
  
> [!NOTE]  
>  此选项不适用于为其他驱动程序定义的数据源。  
  
 **-c** _cmd_end_  
 指定命令终止符。 默认情况下，可以在行中输入一个单独的 GO 来终止命令，并将该命令发送到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 。 如果要重置命令终止符，请勿使用对操作系统有特殊含义的 [!INCLUDE[tsql](../includes/tsql-md.md)] 保留字或字符，无论其前面是否有反斜杠。  
  
 **-q "** _query_ **"**  
 启动 **osql** 时执行查询，但在查询完成时不退出 **osql** 。 （注意查询语句不应包含 GO）。 如果从批处理文件中发出查询，请使用 %variables 或环境 %variables%。 例如：  
  
```  
SET table=sys.objects  
osql -E -q "select name, object_id from %table%"  
```  
  
 将查询用双引号括起来，将查询中嵌入的任何内容用单引号括起来。  
  
 **-Q"** _query_ **"**  
 执行查询并立即退出 **osql**。 将查询用双引号括起来，将查询中嵌入的任何内容用单引号括起来。  
  
 **-n**  
 从输入行中删除编号和提示符号 (>)。  
  
 **-m** _error_level_  
 自定义错误消息的显示。 显示指定的或更高严重级别的错误的消息数、状态和错误级别。 不显示低于指定级别的错误的信息。 使用 **-1** 可以指定返回所有标题及其消息，即使是信息型消息。 如果使用 **-1**，则在参数和设置之间不能有空格（可以是 **-m-1**，不能是 **-m -1**）。  
  
 **-r** { **0**| **1**}  
 将消息输出重定向到屏幕 (**stderr**)。 如果不指定参数，或指定参数为 **0**，则仅重定向严重级别为 11 或更高的错误信息。 如果指定参数为 **1**，则将重定向所有的消息输出（包括“print”）。  
  
 **-i** _input_file_  
 标识包含一批 SQL 语句或存储过程的文件。 小于 ( **\<** ) 比较运算符可以代替 **-i** 使用。  
  
 **-o** _output_file_  
 标识从 **osql** 接收输出的文件。 大于 ( **>** ) 比较运算符可以代替 **-o** 使用。  
  
 如果 *input_file* 不是 Unicode 并且未指定 **-u** ，则以 OEM 格式存储 *output_file* 。 如果 *input_file* 是 Unicode 或指定了 **-u** ，则以 Unicode 格式存储 *output_file* 。  
  
 **-p**  
 打印性能统计信息。  
  
 **-b**  
 指定发生错误时， **osql** 退出并返回一个 DOS ERRORLEVEL 值。 当 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 错误消息的严重级别为 11 或更大值时，返回给 DOS ERRORLEVE 变量的值为 1；否则返回的值为 0。 [!INCLUDE[msCoName](../includes/msconame-md.md)] MS-DOS 批处理文件可以测试 DOS ERRORLEVEL 的值并正确地处理错误。  
  
 **-u**  
 指定无论 *input_file* 为何种格式，都以 Unicode 格式存储 *output_file*。  
  
 **-R**  
 指定在将货币、日期和时间数据转换为字符数据时， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ODBC 驱动程序使用客户端设置。  
  
 **-O**  
 指定停用某些 **osql** 功能以便与 **isql** 的早期版本的行为匹配。 下列功能停用：  
  
-   EOF 批处理  
  
-   自动调整控制台宽度  
  
-   宽消息  
  
 同时还将 DOS ERRORLEVEL 的默认值设置为 -1。  
  
> [!NOTE]  
>  **-n**、 **-O** 和 **-D** 选项不再受 **osql** 支持。  
  
## <a name="remarks"></a>备注  
 **osql** 实用工具从操作系统直接启动，并且使用本文中列出的区分大小写的选项。 **osql** 启动后将接受 SQL 语句，然后以交互方式将这些语句发送到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 。 结果被格式化并在屏幕 (**stdout**) 上显示。 可使用 QUIT 或 EXIT 退出 **osql**。  
  
 如果启动 **osql** 时不指定用户名，则 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将检查并使用环境变量，如 **osqluser=(** _user_ **)** 或 **osqlserver=(** _server_ **)** 。 如果未设置环境变量，则使用工作站用户名。 如果未指定服务器，则使用工作站名称。  
  
 如果 **-U** 或 **-P** 选项都没有使用，则 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将尝试使用 [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows 身份验证模式进行连接。 身份验证根据运行 [!INCLUDE[msCoName](../includes/msconame-md.md)] osql **的用户的** Windows 帐户进行。  
  
 **osql** 实用工具使用 ODBC API。 对于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ISO 连接选项，该实用工具使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ODBC 驱动程序的默认设置。 有关详细信息，请参阅“ANSI 选项的效果”。  
  
> [!NOTE]  
>  **osql** 实用工具不支持 CLR 用户定义数据类型。 若要处理这些数据类型，必须使用 **sqlcmd** 实用工具。 有关详细信息，请参阅 [sqlcmd Utility](../tools/sqlcmd-utility.md)。  
  
## <a name="osql-commands"></a>OSQL 命令  
 除了 [!INCLUDE[tsql](../includes/tsql-md.md)] osql **中的** 语句外，还可以使用以下命令。  
  
|Command|说明|  
|-------------|-----------------|  
|GO|执行上一个 GO 命令之后输入的所有语句。|  
|RESET|清除已输入的所有语句。|  
|QUIT 或 EXIT( )|退出 **osql**。|  
|Ctrl+C|结束查询但不退出 **osql**。|  
  
> [!NOTE]  
>  !! 和 ED 命令不再受 **osql** 支持。  
  
 仅当命令终止符 GO（默认）、RESET EXIT、QUIT 和 Ctrl+C 出现在一行的开始（紧跟 **osql** 提示符）时，才会被识别。  
  
 GO 在批处理和执行任何缓存 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句结尾时会发出信号。 在每个输入行的结尾按 Enter 键时， **osql** 将缓存此行的语句。 键入 GO 后按 Enter 键时，所有当前已缓存的语句都将作为批处理发送到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。  
  
 使用当前的 **osql** 实用工具时，好像在所执行的脚本结尾处都带有隐含的 GO，因而将执行脚本中的所有语句。  
  
 键入以命令终止符开始的行可结束命令。 可以在命令终止符后输入一个整数来指定命令运行的次数。 例如，若要执行此命令 100 次，可键入：  
  
```  
SELECT x = 1  
GO 100  
```  
  
 命令执行结束之后将打印结果。 **osql** 每行的字符数不得超过 1,000 个。 长语句应当跨多行书写。  
  
 Windows 的命令撤回功能可用来撤回和修改 **osql** 语句。 键入 RESET 可以清除现有的查询缓冲区。  
  
 运行存储过程时， **osql** 在批处理中的每个结果集之间打印一个空行。 此外，如果没有应用于执行的语句，则不会出现“0 行受到影响”消息。  
  
## <a name="using-osql-interactively"></a>以交互方式使用 osql  
 若要以交互方式使用 **osql** ，请在命令提示符中键入 **osql** 命令（以及任何选项）。  
  
 通过键入类似下面的命令，可以读入一个包含由 **osql** 执行的查询的文件（例如 Stores.qry）：  
  
```  
osql -E -i stores.qry  
```  
  
 通过键入类似下面的命令，可以读入包含查询的文件（如 Titles.qry），并将结果导向其他文件：  
  
```  
osql -E -i titles.qry -o titles.res  
```  
  
> [!IMPORTANT]  
>  如果可能，请使用 **-E** 选项（信任连接）。  
  
 以交互方式使用 osql 时，可使用 :r _file\_name_ 将操作系统文件读入命令缓冲区。 这会将 *file_name* 中的 SQL 脚本作为单个批处理直接发送给服务器。  
  
> [!NOTE]  
>  使用 **osql** 时，如果批处理分隔符 GO 出现在 SQL 脚本文件中，则 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 会将其视为语法错误。  
  
## <a name="inserting-comments"></a>插入注释  
 可以在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] osql **提交给** 的 Transact-SQL 语句中包含注释。 允许使用两种类型的注释样式：`/*...*/` 和 `--`。  
  
## <a name="using-exit-to-return-results-in-osql"></a>使用 EXIT 返回 osql 中的结果  
 可以使用 SELECT 语句的结果作为 **osql** 的返回值。 如果为数值，则最后一个结果行的最后一列将转换为 4 字节的整数（长整型）。 MS-DOS 将低字节传递给父进程或操作系统错误级别。 Windows 则传递整个 4 字节整数。 语法为：  
  
```  
EXIT ( < query > )  
```  
  
 例如：  
  
```  
EXIT(SELECT @@ROWCOUNT)  
```  
  
 还可以在批处理文件中包含 EXIT 参数。 例如：  
  
```  
osql -E -Q "EXIT(SELECT COUNT(*) FROM '%1')"  
```  
  
 **osql** 实用工具将在圆括号 **()** 中输入的所有内容原样传递给服务器。 如果存储系统过程选择了一个集合并返回一个值，则仅返回选择的内容。 圆括号中无参数的 EXIT **()** 语句将执行批处理中此语句前的所有内容，然后不返回值退出。  
  
 EXIT 格式有四种：  
  
-   EXIT  
  
> [!NOTE]  
>  不执行批处理，立即退出，不返回值。  
  
-   EXIT **()**  
  
> [!NOTE]  
>  执行批处理后退出，不返回值。  
  
-   EXIT **(** _query_ **)**  
  
> [!NOTE]  
>  执行包括查询的批处理，返回查询的结果后退出。  
  
-   状态为 127 的 RAISERROR。  
  
> [!NOTE]  
>  如果在 **osql** 脚本中使用 RAISERROR，并且出现状态 127，则 **osql** 将退出，并将消息 ID 返回给客户端。 例如：  
  
```  
RAISERROR(50001, 10, 127)  
```  
  
 此错误将导致 **osql** 脚本终止，并向客户端返回消息 ID 50001。  
  
 返回值 -1 到 -99 是为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]保留的； **osql** 可定义下列值：  
  
-   -100  
  
     选择返回值前遇到错误。  
  
-   -101  
  
     选择返回值时找不到行。  
  
-   -102  
  
     选择返回值时发生转换错误。  
  
## <a name="displaying-money-and-smallmoney-data-types"></a>显示 Money 和 Smallmoney 数据类型  
 **osql** 只用两位小数位数显示 **money** 和 **smallmoney** 数据类型，但 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 用四位小数位数在内部存储值。 请看下例：  
  
```  
SELECT CAST(CAST(10.3496 AS money) AS decimal(6, 4))  
GO  
```  
  
 此语句的结果为 `10.3496`，说明该值是原样按完整的小数位存储的。  
  
## <a name="see-also"></a>另请参阅  
 [注释 (MDX)](../mdx/comment-mdx.md)   
 [--（注释）(MDX)](../mdx/comment-mdx-operator-reference.md)   
 [CAST 和 CONVERT (Transact-SQL)](../t-sql/functions/cast-and-convert-transact-sql.md)   
 [RAISERROR (Transact-SQL)](../t-sql/language-elements/raiserror-transact-sql.md)  
  
  
