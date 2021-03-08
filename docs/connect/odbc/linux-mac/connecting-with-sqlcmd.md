---
title: 使用 sqlcmd 进行连接
description: 了解如何在 Linux 和 macOS 上结合使用 sqlcmd 实用工具与 Microsoft ODBC Driver for SQL Server。
ms.custom: ''
ms.date: 02/24/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- sqlcmd
ms.assetid: 61a2ec0d-1bcb-4231-bea0-cff866c21463
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 216f78615ca049d3e97134cb14831d9a5e6afd32
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837357"
---
# <a name="connecting-with-sqlcmd"></a>使用 sqlcmd 进行连接
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

可以在 Linux 和 macOS 上结合使用 [sqlcmd](../../../tools/sqlcmd-utility.md) 实用工具与 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。
  
以下命令分别演示如何使用 Windows 身份验证 (Kerberos) 和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证：
  
```console
sqlcmd -E -Sxxx.xxx.xxx.xxx  
sqlcmd -Sxxx.xxx.xxx.xxx -Uxxx -Pxxx  
```  
  
## <a name="available-options"></a>可用选项

在当前版本中，可以使用以下选项：  
  
- -? 显示 `sqlcmd` 使用情况。  
  
- -a 请求数据包大小。  
  
- -b 如果发生错误，则终止批处理作业。  
  
- -c batch_terminator  指定批处理终止符。  
  
- -C 信任服务器证书。  

- -d database_name  启动 `sqlcmd` 时发出 `USE` database_name  语句。  

- -D 使值传递给将解释为数据源名称 (DSN) 的 `sqlcmd` -S 选项。 有关详细信息，请参阅本主题末尾的“`sqlcmd` 和 `bcp` 中的 DSN 支持”。  
  
- -e 将输入脚本写入标准输出设备 (stdout)。

- -E 使用可信连接（集成身份验证）。有关从 Linux 或 macOS 客户端建立使用集成身份验证的可信连接的详细信息，请参阅[使用集成身份验证](using-integrated-authentication.md)。

- -f codepage | i:codepage[,o:codepage] | o:codepage[,i:codepage] 指定输入和输出代码页。 代码页页码是指定已安装的 Linux 代码页的数值。
（自 17.5.1.1 起提供）

- 当连接到 SQL 数据库或 Azure Synapse Analytics 时，客户端将使用 -G 开关指定该用户使用 Azure Active Directory 身份验证来进行身份验证。 此选项设置 sqlcmd 脚本变量 SQLCMDUSEAAD = true。 -G 开关至少需要 sqlcmd 版本 17.6。 要确定你的版本，请执行 sqlcmd -?。

> [!IMPORTANT]
> `-G` 选项仅适用于 Azure SQL 数据库和 Azure Synapse Analytics。
> Linux 或 macOS 目前不支持 AAD 交互式身份验证。 AAD 集成身份验证要求 [Microsoft ODBC Driver 17 for SQL Server](../download-odbc-driver-for-sql-server.md) 版本 17.6.1 或更高版本以及正确[配置的 Kerberos 环境](using-integrated-authentication.md#configure-kerberos)。

- -h number_of_rows  指定要在列标题之间打印的行数  。  
  
- -H 指定工作站名称。  
  
- -i input_file  [,input_file  [,...]] 标识包含一批 SQL 语句或存储过程的文件。  
  
- -I 将 `SET QUOTED_IDENTIFIER` 连接选项设置为“开”。  
  
- -k 删除或替换控制字符。  
  
- -K  application\_intent   
连接到服务器时声明应用程序工作负荷类型。 目前唯一支持的值是 **ReadOnly**。 如果未指定 -K，`sqlcmd` 将不支持连接到 AlwaysOn 可用性组中的次要副本  。 有关详细信息，请参阅 [Linux 和 macOS 上的 ODBC 驱动程序 - 高可用性和灾难恢复](odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md)。  
  
> [!NOTE]  
> **-K** 在适用于 SUSE Linux 的 CTP 中不受支持。 但是，可以在传递给 `sqlcmd` 的 DSN 文件中指定 ApplicationIntent=ReadOnly 关键字  。 有关详细信息，请参阅本主题末尾的“`sqlcmd` 和 `bcp` 中的 DSN 支持”。  
  
- -l *timeout* 指定在尝试连接到服务器时 `sqlcmd` 登录超时时间（以秒为单位）。

- -m error_level 控制将哪些错误消息发送到 stdout  。  
  
- -M  multisubnet\_failover   
在连接到 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 可用性组或 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 故障转移群集实例的可用性组侦听程序时，应始终指定 **-M**。 -M 将为（当前）活动服务器提供更快的故障检测和连接速度  。 如果 -M  未指定，-M  处于关闭状态。 有关 [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)]的详细信息，请参阅 [Linux 和 macOS 上的 ODBC 驱动程序 - 高可用性和灾难恢复](odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md)。  
  
> [!NOTE]  
> **-M** 在适用于 SUSE Linux 的 CTP 中不受支持。 但是，可以在传递给 `sqlcmd` 的 DSN 文件中指定 MultiSubnetFailover=Yes 关键字  。 有关详细信息，请参阅本主题末尾的“`sqlcmd` 和 `bcp` 中的 DSN 支持”。  
  
- -N 加密连接。  
  
- -o output_file 标识从 `sqlcmd` 接收输出的文件  。  
  
- -p 打印每个结果集的性能统计信息。  
  
- -P 指定用户密码。  
  
- -q commandline_query  在启动 `sqlcmd` 时执行查询，但是在查询结束运行时不退出。  

- -Q commandline_query  在 `sqlcmd` 启动时执行查询。 查询结束时，`sqlcmd` 将退出。  

- -r 将错误消息重定向到 stderr。

- -R 使驱动程序使用客户端区域设置来将货币以及日期和时间数据转换为字符数据。 当前仅使用 en_US（美国英语）格式设置。
  
- -s column_separator_char  指定列分隔符字符。  

- -S [*protocol*:] *server*[ **,** _port_]  
指定要连接的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，如果使用的是 -D，则指定 DSN。 Linux 和 macOS 上的 ODBC 驱动程序需要 -S。 请注意，tcp  是唯一有效的协议。  
  
- -t query_timeout 指定命令（或 SQL 语句）超时之前的秒数  。  
  
- -u 指定以 Unicode 格式存储 output_file，无需考虑 input_file 的格式。  
  
- -U login_id  指定用户登录 ID。  
  
- -V error_severity_level 控制用于设置 ERRORLEVEL 变量的严重性级别  。  
  
- -w column_width  指定用于输出的屏幕宽度。  
  
- -W 删除列的尾随空格。  
  
- -x 禁用变量替换。  
  
- -X 禁用命令、启动脚本和环境变量。  
  
- -y variable_length_type_display_width  设置 `sqlcmd` 脚本变量 `SQLCMDMAXFIXEDTYPEWIDTH`。
  
- -Y fixed_length_type_display_width  设置 `sqlcmd` 脚本变量 `SQLCMDMAXVARTYPEWIDTH`。

- -z password  更改密码。  
  
- -Z password  更改密码并退出。  


## <a name="available-commands"></a>可用命令

在当前版本中，可以使用以下命令：  
  
-   [:]!!  
  
-   :Connect  
  
-   :Error  
  
-   [:]EXIT  
  
-   GO [*count*]  
  
-   :Help  
  
-   :List  
  
-   :Listvar  
  
-   :On Error  
  
-   :Out  
  
-   :Perftrace  
  
-   [:]QUIT  
  
-   :r  
  
-   :RESET  
  
-   :setvar  
  
## <a name="unavailable-options"></a>不可用选项
在当前版本中，以下选项不可用：  

- -A 使用专用管理员连接 (DAC) 登录到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 有关如何建立专用管理员连接 (DAC) 的信息，请参阅[编程指南](programming-guidelines.md)。  
  
- -L 列出本地配置的服务器计算机和在网络上播发的服务器计算机的名称。  
  
- -v 创建可用于 `sqlcmd` 脚本中的 `sqlcmd` 脚本变量。  
  
可以使用以下替代方法：将参数放入一个文件中，然后可以将其附加到另一个文件。 这将有助于你使用参数文件来替换值。 例如，创建一个名为 `a.sql` 的文件（参数文件），其中包含以下内容：
  
```console
    :setvar ColumnName object_id  
    :setvar TableName sys.objects  
```
  
然后创建一个名为 `b.sql` 的文件，其中包含用于替换的参数：  
  
```sql
    select $(ColumnName) from $(TableName)  
```

在命令行下，使用以下命令将 `a.sql` 和 `b.sql` 合并为 `c.sql`：  
  
```console
    cat a.sql > c.sql 
  
    cat b.sql >> c.sql  
```
  
运行 `sqlcmd` 并将 `c.sql` 用作输入文件：  
  
```console
    sqlcmd -S<...> -P<..> -U<..> -I c.sql  
```

## <a name="unavailable-commands"></a>不可用的命令

在当前版本中，以下命令不可用：  
  
-   :ED  
  
-   :ServerList  
  
-   :XML  
  
## <a name="dsn-support-in-sqlcmd-and-bcp"></a>sqlcmd 和 bcp 中的 DSN 支持

如果指定 `-D`，则可以在 sqlcmd 或 bcp `-S` 选项（或 sqlcmd :Connect 命令）中指定数据源名称 (DSN) 而不是服务器名称。 `-D` 使 sqlcmd 或 bcp 通过 `-S` 选项连接到 DSN 中指定的服务器。  
  
系统 DSN 存储在 ODBC SysConfigDir 目录的 `odbc.ini` 文件（即标准安装上的 `/etc/odbc.ini`）中。 用户 DSN 存储在用户主目录 (`~/.odbc.ini`) 的 `.odbc.ini` 中。

在 Windows 系统中，系统和用户 DSN 存储在注册表中，通过 odbcad32.exe 进行管理。 Bcp 和 sqlcmd 不支持文件 DSN。

有关驱动程序支持的条目列表，请参阅 [DSN 和连接字符串关键字和属性](../dsn-connection-string-attribute.md)。

在 DSN 中，只有 DRIVER 项是必需的；但若要连接到远程服务器，`sqlcmd` 或 `bcp` 需要 SERVER 元素中的值。 如果 SERVER 元素为空或不存在于 DSN 中，`sqlcmd` 和 `bcp` 将尝试连接到本地系统上的默认实例。

在 Windows 系统上使用 bcp 时，[!INCLUDE [sssql17-md](../../../includes/sssql17-md.md)] 及更早版本需要 SQL Native Client 11 驱动程序 (sqlncli11.dll)，而 [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] 及更高版本需要 Microsoft ODBC Driver 17 for SQL Server 驱动程序 (msodbcsql17.dll)。  

如果在 DSN 和 `sqlcmd` 或 `bcp` 命令行中指定了相同的选项，命令行选项将替代 DSN 中使用的值。 例如，如果 DSN 具有 DATABASE 项且 `sqlcmd` 命令行包含 -d，将使用传递给 -d 的值   。 如果在 DSN 中指定了 Trusted_Connection=yes  ，便会使用 Kerberos 身份验证，并忽略用户名 (-U  ) 和密码 (-P  )（若有）。

通过定义别名 `alias isql="sqlcmd -D"`，可将调用 `isql` 的现有脚本修改为使用 `sqlcmd`。  

## <a name="see-also"></a>另请参阅  
[使用 bcp 连接  ](connecting-with-bcp.md)  
[发行说明](release-notes-tools.md)
