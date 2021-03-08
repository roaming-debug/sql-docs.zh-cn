---
title: bcp 实用工具
description: 大容量复制程序 (bcp) 实用工具在 SQL Server 实例与用户指定格式的数据文件之间批量复制数据。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- bcp utility [SQL Server]
- exporting data
- row exporting [SQL Server]
- copying data [SQL Server], bcp utility
- command prompt utilities [SQL Server], bcp
- tables [SQL Server], importing data
- column importing [SQL Server]
- bcp utility [SQL Server], command options
- file exporting [SQL Server]
- bulk copy [SQL Server]
- bcp utility [SQL Server], about bcp utility
- tables [SQL Server], exporting data
- row importing [SQL Server]
- importing data, bcp utility
- file importing [SQL Server]
- column exporting [SQL Server]
ms.assetid: c0af54f5-ca4a-4995-a3a4-0ce39c30ec38
author: markingmyname
ms.author: maghan
ms.reviewer: v-daenge
ms.custom: seo-lt-2019
ms.date: 02/24/2021
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: c74a2f03c36449bc597d699edb008206c76d7448
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839009"
---
# <a name="bcp-utility"></a>bcp 实用工具

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> 有关在 Linux 上使用 bcp 的信息，请参阅[在 Linux 上安装 sqlcmd 和 bcp](../linux/sql-server-linux-setup-tools.md)。
>
> 有关将 bcp 与 Azure Synapse Analytics 配合使用的详细信息，请参阅[使用 bcp 加载数据](/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp)。

大容量复制程序实用工具 (bcp) 可以在 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例和用户指定格式的数据文件间大容量复制数据   。 使用 **bcp** 实用工具可以将大量新行导入 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 表，或将表数据导出到数据文件。 除非与 **queryout** 选项一起使用，否则使用该实用工具不需要了解 [!INCLUDE[tsql](../includes/tsql-md.md)]知识。 若要将数据导入表中，必须使用为该表创建的格式文件，或者必须了解表的结构以及对于该表中的列有效的数据类型。  

![主题链接图标](../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") 有关 bcp 语法中使用的语法约定，请参阅 [Transact-SQL 语法约定 (Transact-SQL)](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。  

> [!NOTE]
> 如果使用 **bcp** 备份数据，请创建一个格式化文件来记录数据格式。 **bcp** 数据文件 **不包括** 任何架构或格式信息，因此如果已删除表或视图并且不具备格式化文件，则可能无法导入数据。

## <a name="download-the-latest-version-of-bcp-utility"></a>下载最新版本的 bcp 实用工具

[![下载 bcp for x64](../ssdt/media/download.png) 下载 Microsoft Command Line Utilities 15 for SQL Server (x64)](https://go.microsoft.com/fwlink/?linkid=2142258)
<br>[![下载 bcp for x86](../ssdt/media/download.png) 下载 Microsoft Command Line Utilities 15 for SQL Server (x86)](https://go.microsoft.com/fwlink/?linkid=2142257)

命令行工具为公开发行版 (GA)，但与 [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] 的安装程序包一起发布。

### <a name="version-information"></a>版本信息

版本号：15.0.2 <br>
生成号：15.0.2000.5<br>
发行日期：2020 年 9 月 11 日

新版本的 SQLCMD 支持 Azure AD 身份验证，包括对 SQL 数据库、Azure Synapse Analytics 和 Always Encrypted 功能的多重身份验证 (MFA) 支持。
新 BCP 支持 Azure AD 身份验证，包括对 SQL 数据库和 Azure Synapse Analytics 的多重身份验证 (MFA) 支持。

### <a name="system-requirements"></a>系统要求

Windows 10、Windows 7、Windows 8、Windows 8.1、Windows Server 2008、Windows Server 2008 R2、Windows Server 2008 R2 SP1、Windows Server 2012、Windows Server 2012 R2、Windows Server 2016、Windows Server 2019

此组件要求安装 [Windows Installer 4.5](https://windows-installer.soft32.com/) 和 [Microsoft ODBC Driver 17 for SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md)。

若要查看 BCP 版本，请执行 `bcp /v` 命令，并确认使用的是 15.0.2000.5 或更高版本。

<table><th>语法</th><tr><td><pre>
bcp [<a href="#db_name">database_name.</a>] <a href="#schema">schema</a>.{<a href="#tbl_name">table_name</a> | <a href="#vw_name">view_name</a> | <a href="#query">"query"</a>}
    {<a href="#in">in</a> <a href="#data_file">data_file</a> | <a href="#out">out</a> <a href="#data_file">data_file</a> | <a href="#qry_out">queryout</a> <a href="#data_file">data_file</a> | <a href="#format">format</a> <a href="#format">nul</a>}
<a>                                                                                                         </a>
    [<a href="#a">-a packet_size</a>]
    [<a href="#b">-b batch_size</a>]
    [<a href="#c">-c</a>]
    [<a href="#C">-C { ACP | OEM | RAW | code_page } </a>]
    [<a href="#d">-d database_name</a>]
    [<a href="#D">-D</a>]
    [<a href="#e">-e err_file</a>]
    [<a href="#E">-E</a>]
    [<a href="#f">-f format_file</a>]
    [<a href="#F">-F first_row</a>]
    [<a href="#G">-G Azure Active Directory Authentication</a>]
    [<a href="#h">-h"hint [,...n]"</a>]
    [<a href="#i">-i input_file</a>]
    [<a href="#k">-k</a>]
    [<a href="#K">-K application_intent</a>]
    [<a href="#l">-l login_timeout</a>]
    [<a href="#L">-L last_row</a>]
    [<a href="#m">-m max_errors</a>]
    [<a href="#n">-n</a>]
    [<a href="#N">-N</a>]
    [<a href="#o">-o output_file</a>]
    [<a href="#P">-P password</a>]
    [<a href="#q">-q</a>]
    [<a href="#r">-r row_term</a>]
    [<a href="#R">-R</a>]
    [<a href="#S">-S [server_name[\instance_name]</a>]
    [<a href="#t">-t field_term</a>]
    [<a href="#T">-T</a>]
    [<a href="#U">-U login_id</a>]
    [<a href="#v">-v</a>]
    [<a href="#V">-V (80 | 90 | 100 | 110 | 120 | 130 ) </a>]
    [<a href="#w">-w</a>]
    [<a href="#x">-x</a>]
</pre></td></tr></table>

## <a name="arguments"></a>参数

 _**data\_file**_<a name="data_file"></a>  
 数据文件的完整路径。 将数据批量导入 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]时，数据文件将包含要复制到指定的表或视图中的数据。 从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]中批量导出数据时，数据文件将包含从表或视图中复制的数据。 路径可以有 1 到 255 个字符。 数据文件最多可包含 2^63 - 1 行。  

 _**database\_name**_<a name="db_name"></a>  
 指定的表或视图所在数据库的名称。 如果未指定，则使用用户的默认数据库。  

 也可以使用 d- 显式指定数据库名称。  

 **in** *data_file* \| **out** *data_file* \| **queryout** *data_file* \| **format nul**  
 指定大容量复制的方向，具体如下：  
  
- **in**<a name="in"></a> 从文件复制到数据库表或视图。  
  
- **out**<a name="out"></a> 从数据库表或视图复制到文件。 如果指定了现有文件，则该文件将被覆盖。 提取数据时，bcp 实用工具将空字符串表示为 null，而将 null 字符串表示为空字符串。  
  
- **queryout**<a name="qry_out"></a> 从查询中复制，仅当从查询大容量复制数据时才必须指定此选项。  
  
- **format**<a name="format"></a> 根据指定的选项（ **-n**、 **-c**、 **-w** 或 **-N**）以及表或视图的分隔符创建格式化文件。 大容量复制数据时，bcp 命令可以引用一个格式化文件，从而避免以交互方式重复输入格式信息。 **format** 选项要求指定 **-f** 选项；创建 XML 格式化文件时还需要指定 **-x** 选项。 有关详细信息，请参阅 [创建格式化文件 (SQL Server)](../relational-databases/import-export/create-a-format-file-sql-server.md)。 必须将 **nul** 指定为值 (**format nul**)。  
  
 _**owner**_<a name="schema"></a>  
 表或视图的所有者的名称。 如果执行该操作的用户拥有指定的表或视图，则 *owner* 是可选的。 如果未指定 *owner* ，并且执行该操作的用户不是指定的表或视图的所有者，则 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将返回错误消息，而且该操作将取消。  
  
**"** _**query**_ **"** <a name="query"></a> 是一个返回结果集的 [!INCLUDE[tsql](../includes/tsql-md.md)] 查询。 如果该查询返回多个结果集，则只将第一个结果集复制到数据文件，而忽略其余的结果集。 将查询用双引号括起来，将查询中嵌入的任何内容用单引号括起来。 从查询大容量复制数据时，也必须指定 **queryout** 。  
  
 只要在执行 bcp 语句之前存储过程内引用的所有表均存在，查询就可以引用该存储过程。 例如，如果存储过程生成一个临时表，则 **bcp** 语句便会失败，因为该临时表只在运行时可用，而在语句执行时不可用。 在这种情况下，应考虑将存储过程的结果插入表中，然后使用 **bcp** 将数据从表复制到数据文件中。  
  
 _**table\_name**_<a name="tbl_name"></a>  
 将数据导入 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**in**) 时为目标表名称，将数据从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 导出时 (**out**) 为源表名称。  
  
 _**view\_name**_<a name="vw_name"></a>   
 将数据复制到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**in**) 时为目标视图名称，从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中复制数据时 (**out**) 为源视图名称。 只有其中所有列都引用同一个表的视图才能用作目标视图。 有关将数据复制到视图的限制的详细信息，请参阅[《INSERT &#40;Transact-SQL&#41;》](../t-sql/statements/insert-transact-sql.md)。  
  
 **-a** _**packet\_size**_<a name="a"></a>  
 指定服务器发出或接收的每个网络数据包的字节数。 可以使用 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] （或 **sp_configure** 系统存储过程）来设置服务器配置选项。 但是，可以使用此选项逐个替代服务器配置选项。 packet_size 的取值范围为 4096 到 65535 字节，默认为 4096 字节。  
  
 增大数据包可以提高大容量复制操作的性能。 如果无法得到请求的较大数据包，则使用默认值。 **bcp** 实用工具生成的性能统计信息可以显示所用的数据包大小。  
  
 **-b** _**batch\_size**_<a name="b"></a>  
 指定每批导入数据的行数。 每个批次均作为一个单独的事务进行导入并记录，在提交之前会导入整批。 默认情况下，数据文件中的所有行均作为一个批次导入。 若要将行分为多个批次进行操作，请指定小于数据文件中的行数的 *batch_size* 。 如果任何批次的事务失败，则将只回滚当前批次中的插入。 已经由已提交事务导入的批次不会受到将来失败的影响。  
  
 不要将此选项与 **-h "** ROWS_PER_BATCH **=** _bb_ **"** 选项一起使用。  
 
 **-c**<a name="c"></a>  
 使用字符数据类型执行该操作。 此选项不提示输入每个字段；它使用 **char** 作为存储类型，没有前缀；使用 **\t** （制表符）作为字段分隔符，使用 **\r\n** （换行符）作为行终止符。 **-c** 与 **-w** 不兼容。  
  
 有关详细信息，请参阅[使用字符格式导入或导出数据 (SQL Server)](../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)。  
  
 **-C** { **ACP** \| **OEM** \| **RAW** \| *code_page* }<a name="C"></a>   
 指定该数据文件中数据的代码页。 仅当数据含有字符值大于 127 或小于 32 的 *code_page* 、 **code_page**, **varcode_page** 列时， **code_page** columns with code_pageacter values greater than 127 or less than 32.  
  
> [!NOTE]
> 我们建议你为格式文件中的每个列指定一个排序规则名称，除非你希望 65001 选项优先于排序规则/代码页规范。
  
|代码页值|说明|  
|---------------------|-----------------|  
|ACP|[!INCLUDE[vcpransi](../includes/vcpransi-md.md)]/Microsoft Windows (ISO 1252)。|  
|OEM|客户端使用的默认代码页。 未指定 **-C** 时使用的默认代码页。|  
|RAW|不进行代码页间的转换。 因为不进行转换，所以这是最快的选项。|  
|*code_page*|特定的代码页编号，例如 850。<br /><br /> 低于 13 ([!INCLUDE[sssql15-md](../includes/sssql16-md.md)]) 的版本不支持代码页 65001（UTF-8 编码）。 版本 13 和后续版本可将 UTF-8 编码导入以前版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。|  
|||
  
 **-d** _**database\_name**_<a name="d"></a>   
 指定要连接到的数据库。 默认情况下，bcp.exe 连接到用户的默认数据库。 如果指定了 -d database_name 和包含三部分的名称（database_name.schema.table，作为第一个参数传递给 bcp.exe），则将发生错误，因为该数据库名称不能指定两次。 如果 database_name 以连字符 (-) 或正斜杠 (/) 开头，则不要在 -d 与数据库名称之间添加空格。  

**-D**<a name="D"></a>  
使值传递给将解释为数据源名称 (DSN) 的 `bcp` `-S` 选项。 DSN 可以用于嵌入驱动程序选项，以简化命令行，强制执行其他无法从命令行（例如 MultiSubnetFailover）访问的驱动程序选项，或帮助防止敏感凭据被视为命令行参数。 有关详细信息，请参阅[使用 sqlcmd 进行连接](../connect/odbc/linux-mac/connecting-with-sqlcmd.md)中的“sqlcmd 和 bcp 中的 DSN 支持”。


 **-e** _**err\_file**_<a name="e"></a>  
 指定错误文件的完整路径，此文件用于存储 **bcp** 实用工具无法从文件传输到数据库的所有行。 **bcp** 命令产生的错误消息将被发送到用户的工作站。 如果不使用此选项，则不会创建错误文件。  
  
 如果 *err_file* 以连字符 (-) 或正斜杠 (/) 开头，则不要在 **-e** 与 *err_file* 值之间包含空格。  
  
**-E**<a name="E"></a>

指定导入数据文件中的标识值用于标识列。 如果未指定 **-E** ，则将忽略要导入的数据文件中此列的标识值，而且 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将根据创建表期间指定的种子值和增量值自动分配唯一值。  有关详细信息，请参阅 [DBCC CHECKIDENT](../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)。
  
 如果数据文件不包含表或视图中的标识列的值，则可使用格式化文件指定，在导入数据时应跳过表或视图中的标识列； [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将自动为该列分配唯一值。
  
 **-E** 选项有一个特殊的权限要求。 有关详细信息，请参阅本主题后面的“[备注](#remarks)”。  
   
 **-f** _**format\_file**_<a name="f"></a>  
 指定格式化文件的完整路径。 此选项的含义取决于使用它的环境，具体如下：  
  
- 如果 **-f** 与 **format** 选项一起使用，则将为指定的表或视图创建指定的 *format_file* 。 若要创建 XML 格式化文件，请同时指定 **-x** 选项。 有关详细信息，请参阅 [创建格式化文件 (SQL Server)](../relational-databases/import-export/create-a-format-file-sql-server.md)。  
  
- 如果与 **in** 或 **out** 选项一起使用，则 **-f** 需要一个现有的格式化文件。  
  
    > [!NOTE]
    > 与 **in** 或 **out** 选项一起使用时，格式化文件是可选的。 如果没有 **-f** 选项，则在未指定 **-n**、 **-c**、 **-w** 或 - **N** 时，该命令将提示输入格式信息，并允许你将响应保存在格式化文件（默认文件名为 Bcp.fmt）中。
  
 如果 *format_file* 以连字符 (-) 或正斜杠 (/) 开头，则不要在 **-f** 与 *format_file* 值之间包含空格。  
  
**-F** _**first\_row**_<a name="F"></a>  
 指定要从表中导出或从数据文件导入的第一行的编号。 此参数的值应大于 (>) 0，小于 (<) 或等于 (=) 总行数。 如果未指定此参数，则默认为文件的第一行。  
  
 *first_row* 可以是一个最大为 2^63-1 的正整数值。 **-F** *first_row* 的值从 1 开始。  

**-G**<a name="G"></a>

 当连接到 Azure SQL 数据库或 Azure Synapse Analytics 时，客户端将使用此开关指定该用户使用 Azure Active Directory 身份验证来进行身份验证。 -G 开关需要[版本 14.0.3008.27 或更高版本](https://go.microsoft.com/fwlink/?LinkID=825643)。 要确定你的版本，请执行 bcp -v。 有关详细信息，请参阅[将 Azure Active Directory 身份验证用于 SQL 数据库或 Azure Synapse Analytics 身份验证](/azure/sql-database/sql-database-aad-authentication)。 

> [!IMPORTANT]
> -G 选项仅适用于 Azure SQL 数据库和 Azure Synapse Analytics。
> Linux 或 macOS 目前不支持 AAD 交互式身份验证。 AAD 集成身份验证要求 [Microsoft ODBC Driver 17 for SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md) 版本 17.6.1 或更高版本以及正确[配置的 Kerberos 环境](../connect/odbc/linux-mac/using-integrated-authentication.md#configure-kerberos)。

> [!TIP]
> 若要检查 BCP 版本是否包括对 Azure Active Directory 身份验证 (AAD) 类型 **bcp --** (bcp\<space>\<dash>\<dash>) 并验证可用参数列表中是否显示 -G。

- **Azure Active Directory 用户名和密码：** 

    当你想要使用 Azure Active Directory 用户名和密码时，可以提供 **-G** 选项，也可以通过提供 **-U** 选项和 **-P** 选项来使用用户名和密码。 

    下面的示例使用 Azure AD 用户名和密码导出数据，其中用户和密码是 AAD 凭据。 该示例从 Azure 服务器 `aadserver.database.windows.net` 的数据库 `testdb` 导出表 `bcptest`，并将数据存储在文件 `c:\last\data1.dat` 中：

    ```cmd
    bcp bcptest out "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U alice@aadtest.onmicrosoft.com -P xxxxx
    ```

    下面的示例使用 Azure AD 用户名和密码导入数据，其中用户和密码是 AAD 凭据。 该示例使用 Azure AD 用户/密码将数据从文件 `c:\last\data1.dat` 导入到 Azure 服务器 `aadserver.database.windows.net` 上的数据库 `testdb` 的表 `bcptest` 中：

    ```cmd
    bcp bcptest in "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U alice@aadtest.onmicrosoft.com -P xxxxx
    ```

- **Azure Active Directory 集成**

    要进行 Azure Active Directory 集成身份验证，可提供 -G 选项而无需用户名或密码。 此配置假定当前的 Windows 用户帐户（运行 bcp 命令的帐户）与 Azure AD 联合：

    下面的示例使用 Azure AD 集成帐户导出数据。 该示例使用 Azure 服务器 `aadserver.database.windows.net` 中的 Azure AD 集成身份验证从数据库 `testdb` 导出表 `bcptest`，并将数据存储在文件 `c:\last\data2.dat` 中：

    ```cmd
    bcp bcptest out "c:\last\data2.dat" -S aadserver.database.windows.net -d testdb -G -c -t
    ```

    以下示例使用 Azure AD 集成身份验证导入数据。该示例使用 Azure AD 集成身份验证，将数据从文件 `c:\last\data2.txt` 导入到 Azure 服务器 `aadserver.database.windows.net` 上的数据库 `testdb` 的表 `bcptest` 中：

    ```cmd
    bcp bcptest in "c:\last\data2.dat" -S aadserver.database.windows.net -d testdb -G -c -t
    ```

- **Azure Active Directory 交互式身份验证**  

   使用面向 Azure SQL 数据库和 Azure Synapse Analytics 的 Azure AD 交互式身份验证，可以使用支持多重身份验证的交互式方法。 有关详细信息，请参阅 [Active Directory 交互式身份验证](../ssdt/azure-active-directory.md#active-directory-interactive-authentication)。

   Azure AD 交互式身份验证需要 bcp [版本 15.0.1000.34](#download-the-latest-version-of-bcp-utility) 或更高版本，以及 [ODBC 版本 17.2 或更高版本](../connect/odbc/download-odbc-driver-for-sql-server.md)。  

   若要启用交互身份验证，请仅提供用户名 (-U) 的 -G 选项，而不提供密码。

   下面的示例使用 Azure AD 交互模式导出数据，该数据指示用户名，其中的用户代表 AAD 帐户。 这与上一节中使用的示例相同：Azure Active Directory 用户名和密码。  

   交互模式需要手动输入密码，或者对于启用了多重身份验证的帐户，请完成配置的 MFA 身份验证方法。

   ```cmd
   bcp bcptest out "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U alice@aadtest.onmicrosoft.com
   ```

   如果 Azure AD 用户是使用 Windows 帐户的域联合用户，则命令行中所需的用户名包含其域帐户（例如 joe@contoso.com，见下文）：

   ```cmd
   bcp bcptest out "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U joe@contoso.com
   ```

   如果来宾用户在特定 Azure AD 中存在，并且属于 SQL 数据库中存在的包含执行 bcp 命令的数据库权限的组，则将使用其来宾用户别名（例如， *keith0@adventureworks.com* ）。
  
**-h** _**"load hints**_[ ,... *n*] **"** <a name="h"></a> 指定向表或视图中批量导入数据时要用到的一个或多个提示。  
  
* **ORDER**(**_column_[ASC | DESC] [** , **..._n_])**  
数据文件中的数据排序次序。 如果根据表中的聚集索引（如果有）对要导入的数据排序，则可提高批量导入的性能。 如果数据文件以不同的次序（即不同于聚集索引键的次序）排序，或者表中不存在任何聚集索引，则将忽略 ORDER 子句。 提供的列名必须是目标表中有效的列名。 默认情况下， **bcp** 假定数据文件没有排序。 对于经过优化的批量导入， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 还将验证导入的数据是否已排序。  
  
* **ROWS_PER_BATCH** **=** _**bb**_  
每批数据的行数（即 *bb*）。 在未指定 **-b** 时使用，这将导致整个数据文件作为单个事务发送到服务器。 服务器根据 bb值优化大容量加载。 默认情况下，ROWS_PER_BATCH 是未知的。  
  
* **KILOBYTES_PER_BATCH** **=** _**cc**_  
每批的以千字节计数的近似数据量（即 *cc*）。 默认情况下，KILOBYTES_PER_BATCH 是未知的。  
  
* **TABLOCK**  
指定在大容量加载操作期间获取大容量更新表级别的锁；否则，获取行级别的锁。 由于在大容量复制操作期间拥有锁可以减少表中的锁争夺，所以此提示可显著提高性能。 如果表没有索引并且指定了 **TABLOCK** ，则该表可以同时由多个客户端加载。 默认情况下，锁定行为由表选项 table lock on bulk load 决定。  
  
  > [!NOTE]
  > 如果目标表是聚集列存储索引，则多个并发客户端不需要 TABLOCK 提示就能加载，因为将在索引中为每个并发线程分配单独的行组，并在该行组中加载数据。 有关详细信息，请参阅列存储索引概念主题，
  
  **CHECK_CONSTRAINTS**  
  指定在批量导入操作期间，必须检查所有对目标表或视图的约束。 如果没有 CHECK_CONSTRAINTS 提示，则忽略所有 CHECK 和 FOREIGN KEY 约束；操作完成后，对表的约束将被标记为不可信。  
  
  > [!NOTE]
  > 始终强制使用 UNIQUE、PRIMARY KEY 和 NOT NULL 约束。
  
  在某些时候，需要检查整个表的约束。 如果在批量导入操作之前表为非空状态，则重新验证约束的开销可能超过将 CHECK 约束应用于增量数据的开销。 因此，建议您在正常情况下，在进行增量式批量导入时启用约束检查。  
  
  当输入数据包含违反约束的行时，您可能希望禁用约束（默认行为）。 如果禁用 CHECK 约束，您可以导入数据，然后使用 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句删除无效数据。  
  
  > [!NOTE]
  > bcp 现在会强制执行数据验证和数据检查，这样，在对数据文件中的无效数据执行脚本时，可能会导致脚本失败。

  > [!NOTE]
  > **-m** *max_errors* 开关不适用于约束检查。
  
* **FIRE_TRIGGERS**  
与 **in** 参数一同指定，在目标表中定义的任何插入触发器都将在大容量复制操作期间运行。 如果未指定 FIRE_TRIGGERS，将不运行任何插入触发器。 对于 **out**、 **queryout** 和 **format** 参数，将忽略 FIRE_TRIGGERS。  
  
**-i** _**input\_file**_<a name="i"></a>  
指定响应文件的名称，其中包含在交互模式（未指定 **-n**、 **-c**、 **-w** 或 **-N** ）下执行大容量复制时，对每个数据字段的命令提示问题所做出的响应。  
  
如果 *input_file* 以连字符 (-) 或正斜杠 (/) 开头，则不要在 **-i** 与 *input_file* 值之间包含空格。  
  
**-k**<a name="k"></a>  
指定在操作过程中空列应该保留 null 值，而不是所插入列的任何默认值。 有关详细信息，请参阅[在批量导入期间保留 Null 或使用默认值 (SQL Server)](../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)。  
  
**-K** _**application\_intent**_<a name="K"></a>   
连接到服务器时声明应用程序工作负荷类型。 唯一可能的值是 **ReadOnly**。 如果未指定 **-K** ，bcp 实用工具将不支持连接到 Always On 可用性组中的辅助副本。 有关详细信息，请参阅[活动次要副本：可读次要副本（Always On 可用性组）](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)。  
  
**-l** _**login\_timeout**_<a name="l"></a>  
指定登录超时。 -l 选项指定在尝试连接到服务器时登录 SQL Server 的超时时间（以秒为单位）。 默认登录超时值为 15 秒。 登录超时必须是介于 0 和 65534 之间的数字。 如果提供的值不是数值或不在此范围内，bcp 将生成错误消息。 值 0 指定无限超时。
  
**-L** _**last\_row**_<a name="L"></a>  
指定要从表中导出或从数据文件中导入的最后一行的编号。 此参数的值应大于 (>) 0，小于 (<) 或等于 (=) 最后一行的编号。 如果未指定此参数，则默认为文件的最后一行。  
  
*last_row* 可以是一个最大为 2^63-1 的正整数值。  
  
**-m** _**max\_errors**_<a name="m"></a>  
指定取消 **bcp** 操作之前可能出现的语法错误的最大数目。 语法错误是指将数据转换为目标数据类型时的错误。 *max_errors* 总数不包括只能在服务器中检测到的错误，如约束冲突。  
  
 无法由 **bcp** 实用工具复制的行将被忽略，并计为一个错误。 如果未包括此选项，则默认值为 10。  
  
> [!NOTE]
> **-m** 选项也不适用于转换 **money** 或 **bigint** 数据类型。
  
**-n**<a name="n"></a>  
使用数据的本机（数据库）数据类型执行大容量复制操作。 此选项不提示输入每个字段，它将使用本机值。  
  
有关详细信息，请参阅[使用本机格式导入或导出数据 (SQL Server)](../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)。  
  
**-N**<a name="N"></a>  
执行大容量复制操作时，对非字符数据使用本机（数据库）数据类型的数据，对字符数据使用 Unicode 字符。 此选项是 **-w** 选项的一个替代选项，并具有更高的性能。此选项主要用于通过数据文件将数据从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的一个实例传送到另一个实例。 此选项不提示输入每个字段。 如果要传送包含 ANSI 扩展字符的数据，并希望利用本机模式的性能优势，则可使用此选项。  
  
 有关详细信息信息，请参阅 [使用 Unicode 本机格式导入或导出数据 (SQL Server)](../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)。  
  
 通过使用 bcp.exe 和 **-N** 来导出数据后又将数据导入到同一表架构时，如果存在固定长度的非 Unicode 字符列（例如 **char(10)** ），系统可能会显示截断警告。  
  
 可忽略该警告。 解决此警告的一个方法是使用 **-n** 来替代 **-N**。  
  
 **-o** _**output\_file**_<a name="o"></a>  
 指定文件名称，该文件用于接收从命令提示符重定向来的输出。  
  
 如果 *output_file* 以连字符 (-) 或正斜杠 (/) 开头，则不要在 **-o** 与 *output_file* 值之间包含空格。  
  
 **-P** _**password**_<a name="P"></a>  
 指定登录 ID 的密码。 如果未使用此选项， **bcp** 命令将提示输入密码。 如果在命令提示符的末尾使用此选项，但不提供密码，则 **bcp** 将使用默认密码 (NULL)。  
  
> [!IMPORTANT]
> [!INCLUDE[ssNoteStrongPass](../includes/ssnotestrongpass-md.md)]
  
 若要屏蔽密码，请不要指定 **-P** 选项和 **-U** 选项。 而应在指定 **bcp** 以及 **-U** 选项和其他开关（不指定 **-P**）之后按 Enter，这时命令会提示输入密码。 这种方法可以确保输入密码时对其屏蔽。  
  
 如果 password 以连字符 (-) 或正斜杠 (/) 开头，则不要在 **-P** 与 password 值之间添加空格。  
  
 **-q**<a name="q"></a>  
 在 **bcp** 实用工具和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]实例之间的连接中，执行 SET QUOTED_IDENTIFIERS ON 语句。 使用此选项可以指定包含空格或单引号的数据库、所有者、表或视图的名称。 将由三部分组成的整个表名或视图名用英文双引号 ("") 引起来。  
  
 必须使用 -q 选项，才能指定包含空格或单引号的数据库名称。  
  
 **-q** 不适用于传递到 **-d** 的值。  
  
 有关详细信息，请参阅本主题后面的 [备注](#remarks)。  
  
 **-r** _**row\_term**_<a name="r"></a>  
 指定行终止符。 默认的行终止符是 **\n** （换行符）。 使用此参数可替代默认行终止符。 有关详细信息，请参阅 [指定字段终止符和行终止符 (SQL Server)](../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)。  
  
 如果您在 bcp.exe 命令中以十六进制表示法指定行终止符，则该值将在 0x00 处截断。 例如，如果您指定 0x410041，则将使用 0x41。  
  
 如果 *row_term* 以连字符 (-) 或正斜杠 (/) 开头，则不要在 **-r** 与 *row_term* 值之间包含空格。  
  
 **-R**<a name="R"></a>  
 指定使用客户端计算机区域设置中定义的区域格式，将货币、日期和时间数据大容量复制到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中。 默认情况下，将忽略区域设置。  
  
 **-S** _**server\_name**_ [\\_**instance\_name**_]<a name="S"></a> 指定要连接的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的实例。 如果未指定服务器，则 **bcp** 实用工具将连接到本地计算机上的默认 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例。 如果从网络或本地命名实例上的远程计算机中运行 **bcp** 命令，则必须使用此选项。 若要连接到服务器上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 默认实例，请仅指定 *server_name*。 要连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的命名实例，请指定 server\_name\\instance\_name。  
  
 **-t** _**field\_term**_<a name="t"></a>  
 指定字段终止符。 默认的字段终止符是 **\t** （制表符）。 使用此参数可以替代默认字段终止符。 有关详细信息，请参阅 [指定字段终止符和行终止符 (SQL Server)](../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)。  
  
 如果您在 bcp.exe 命令中以十六进制表示法指定字段终止符，则该值将在 0x00 处截断。 例如，如果您指定 0x410041，则将使用 0x41。  
  
 如果 *field_term* 以连字符 (-) 或正斜杠 (/) 开头，则不要在 **-t** 与 *field_term* 值之间包含空格。  
  
 **-T**<a name="T"></a>  
 指定 **bcp** 实用工具通过使用集成安全性的受信任连接连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 。 不需要网络用户的安全凭据 login_id 和 password。 如果未指定 **-T** ，则需要指定 **-U** 和 **-P** 才能成功登录。

> [!IMPORTANT]
> 如果 **bcp** 实用工具通过使用集成安全性的可信连接连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]，则使用的是 **-T** 选项（可信连接），而不是用户名和密码的组合。  当 bcp 实用工具连接到 SQL 数据库或 Azure Synapse Analytics 时，不支持使用 Windows 身份验证或 Azure Active Directory 身份验证。 使用 **-U** 和 **-P** 选项。 
  
 **-U** _**login\_id**_<a name="U"></a>  
 指定用于连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的登录 ID。  
  
> [!IMPORTANT]
> 如果 **bcp** 实用工具通过使用集成安全性的可信连接连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]，则使用的是 **-T** 选项（可信连接），而不是用户名和密码的组合。  当 bcp 实用工具连接到 SQL 数据库或 Azure Synapse Analytics 时，不支持使用 Windows 身份验证或 Azure Active Directory 身份验证。 使用 **-U** 和 **-P** 选项。
  
 **-v**<a name="v"></a>  
 报告 **bcp** 实用工具的版本号和版权信息。  
  
 **-V** (**80** \| **90** \| **100** \| **110** \| **120** \| **130**)<a name="V"></a>  
 使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]早期版本中的数据类型执行大容量复制操作。 此选项并不提示输入每个字段，它将使用默认值。  
  
 **80** = [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]  
  
 **90** = [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]  
  
 **100** = [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] 和 [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]  
  
 **110** = [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]  
  
 **120** = [!INCLUDE[ssSQL14](../includes/sssql14-md.md)]  
  
 **130** = [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]  
  
 例如，若要为 [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]不支持、但是在较高版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]中引入的类型生成数据，请使用 -V80 选项。  
  
 有关详细信息，请参阅 [导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据](../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)。  
  
 **-w**<a name="w"></a>  
 使用 Unicode 字符执行大容量复制操作。 此选项不提示输入每个字段；它使用 **nchar** 作为存储类型，不带前缀；使用 **\t** （制表符）作为字段分隔符，使用 **\n** （换行符）作为行终止符。 **-w** 与 **-c** 不兼容。  
  
 有关详细信息，请参阅 [使用 Unicode 字符格式导入或导出数据 (SQL Server)](../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)。  
  
 **-x**<a name="x"></a>  
 结合使用该格式和 -f format_file 选项，可生成基于 XML 的格式化文件，而不是默认的非 XML 格式化文件。 在导入或导出数据时， **-x** 不起作用。 如果不与格式和 -f format_file 一起使用，则将生成错误。  

## <a name="remarks"></a>备注<a name="remarks"></a>

- 安装 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[sssql19-md.md](../includes/sssql19-md.md)] 工具时，将安装 bcp 13.0 客户端。 如果为多个版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 安装了工具，则你使用的可能是早期版本的 bcp 客户端，而不是 bcp 13.0 客户端，具体取决于 PATH 环境变量的值的顺序 。 此环境变量定义 Windows 用于搜索可执行文件的目录集。 若要确定当前所使用的版本，请在 Windows 命令提示符下运行 bcp /v 或 bcp -v 命令 。 有关如何在 PATH 环境变量中设置命令路径的信息，请参阅[环境变量](/windows/win32/shell/user-environment-variables)或在 Windows 帮助中搜索环境变量。

    为确保运行的是最新版本的 bcp 实用工具，需要删除所有旧版本的 bcp 实用工具。

    若要确定 bcp 实用工具的所有版本的安装位置，请在命令提示符下键入以下内容：

    ```cmd
    where bcp.exe
    ```

- bcp 实用工具还可以与 [Microsoft SQL Server 2016 功能包](https://www.microsoft.com/download/details.aspx?id=56833)分开下载。  选择 `ENU\x64\MsSqlCmdLnUtils.msi` 或 `ENU\x86\MsSqlCmdLnUtils.msi`。

- 只有当 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 工具和 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Native Client 一起安装后，才支持 XML 格式化文件。

- 有关在何处查找或如何运行 **bcp** 实用工具的信息以及有关命令提示实用工具语法约定的信息，请参阅 [《Command Prompt Utility Reference &#40;Database Engine&#41;》](../tools/command-prompt-utility-reference-database-engine.md)（命令提示实用工具参考（数据库引擎））。

- 有关准备数据以进行批量导入或导出操作的信息，请参阅 [准备用于批量导出或导入的数据 (SQL Server)](../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)知识。

- 有关何时在事务日志中记录由批量导入执行的行插入操作的信息，请参阅 [《Prerequisites for Minimal Logging in Bulk Import》](../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md)（批量导入的最小日志记录的先决条件）。

- [使用其他特殊字符](/windows-server/administration/windows-commands/set_1#remarks)

    字符 <、>、|、&、^ 是特殊的命令 shell 字符，在字符串中使用这些字符时，必须在它们前面加上转义符 (^) 或用引号将其引起来（例如“StringContaining&Symbol”）。 如果使用引号将包含某个特殊字符的字符串引起来，则引号将设置为环境变量值的一部分。

## <a name="native-data-file-support"></a>本机数据文件支持

 在 [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] 中，bcp 实用工具支持与 [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)] 及更高版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本兼容的本机数据文件。

## <a name="computed-columns-and-timestamp-columns"></a>计算列和 timestamp 列

 数据文件中针对计算列或 **timestamp** 列导入的值将被忽略， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将自动分配该列的值。 如果数据文件不包含表中的计算列或 **timestamp** 列的值，则可使用格式化文件指定应在导入数据时忽略表中的计算列或 **timestamp** 列； [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将自动分配该列的值。  
  
 计算列和 **timestamp** 列会照常从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 大容量复制到数据文件中。  

## <a name="specifying-identifiers-that-contain-spaces-or-quotation-marks"></a>指定包含空格或引号的标识符

 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 标识符可以包含嵌入的空格和引号等字符。 此类标识符必须按以下方式处理：  
  
- 如果在命令指示符处指定的标识符或文件名包含空格或引号，则需用英文双引号 ("") 将该标识符引起来。  

    例如，下面的 `bcp out` 命令创建了一个名为 `Currency Types.dat`的数据文件：  

    ```cmd
    bcp AdventureWorks2012.Sales.Currency out "Currency Types.dat" -T -c  
    ```
  
- 若要指定包含空格或引号的数据库名称，必须使用 **-q** 选项。  
  
- 对于包含嵌入空格或引号的所有者、表或视图的名称，可以执行以下任一操作：  
  
    - 指定 **-q** 选项，或者  
  
    - 将所有者、表或视图的名称括在方括号 ([]) 中，并用引号引起来。  

## <a name="data-validation"></a>数据验证

 bcp 现在会强制执行数据验证和数据检查，这样，在对数据文件中的无效数据执行脚本时，可能会导致脚本失败。 例如， **bcp** 现在可以验证：  
  
- float 或 real 数据类型的本机表示形式是否有效。  
  
- Unicode 数据的字节数是否为偶数。  
  
 可以在早期版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中批量导入的无效数据类型现在可能无法加载。在早期版本中，仅当客户端尝试访问无效数据时才出现失败。 在大容量加载后查询数据时，添加的验证可最大限度地减少警告。  

## <a name="bulk-exporting-or-importing-sqlxml-documents"></a>批量导出或导入 SQLXML 文档

若要批量导出或导入 SQLXML 数据，请在格式化文件中使用下列数据类型之一。  
  
|数据类型|效果|  
|---------------|------------|  
|SQLCHAR 或 SQLVARYCHAR|在客户端代码页或排序规则隐含的代码页中发送数据。 其效果和指定 **-c** 开关而不指定格式化文件相同。|  
|SQLNCHAR 或 SQLNVARCHAR|以 Unicode 格式发送数据。 其效果和指定 **-w** 开关而不指定格式化文件相同。|  
|SQLBINARY 或 SQLVARYBIN|不经任何转换即发送数据。|  

## <a name="permissions"></a>权限

 **bcp out** 操作要求对源表有 SELECT 权限。  
  
 **bcp in** 操作要求至少对目标表有 SELECT/INSERT 权限。 此外，如果下列任一条件成立，则要求拥有 ALTER TABLE 权限：  
  
- 存在约束，但没有指定 CHECK_CONSTRAINTS 提示。  
  
    > [!NOTE]
    > 禁用约束是默认行为。 若要显式启用约束，请使用 **-h** 选项和 CHECK_CONSTRAINTS 提示。
  
- 存在触发器，但没有指定 FIRE_TRIGGER 提示。  
  
    > [!NOTE]
    > 默认情况下，不激发触发器。 若要显式激发触发器，请使用 **-h** 选项和 FIRE_TRIGGERS 提示。
  
- 可以使用 **-E** 选项从数据文件导入标识值。  
  
> [!NOTE]
> 要求对目标表具有 ALTER TABLE 权限是 [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]的新要求。 如果用户帐户不具有对目标表的 ALTER TABLE 权限，这项新要求有可能导致不强制使用触发器和约束检查的 **bcp** 脚本失败。

## <a name="character-mode--c-and-native-mode--n-best-practices"></a>字符模式 (-c) 和本机模式 (-n) 最佳做法

 本节提供与字符模式 (-c) 和本机模式 (-n) 有关的一些建议。  
  
- （管理员/用户）应尽可能使用本机格式 (-n) 以避免分隔符问题。 使用本机格式可以使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]进行导出和导入。 如果数据将导入到非 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库，则使用 -c 或 -w 选项从[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 导出数据。  
  
- （管理员）在使用 BCP OUT 时验证数据。 例如，在您使用 BCP OUT、BCP IN，然后又使用 BCP OUT 时，请验证数据正确导出，并且终止符值未用作某个数据值的一部分。 请考虑使用随机的十六进制值覆盖默认的终止符（使用 -t 和 -r 选项），以便避免终止符值和数据值之间的冲突。  
  
- （用户）使用长且唯一的终止符（任意字节或字符序列）可以最大程度减少与实际字符串值冲突的可能性。 这可以通过使用 -t 和 -r 选项实现。  

## <a name="examples"></a>示例

本部分包含以下示例：

A. 标识 **bcp** 实用工具版本

B. 将表行复制到数据文件中（使用可信连接）

C. 将表行复制到数据文件中（使用混合模式身份验证）

D. 将文件中的数据复制到表中

E. 将特定的列复制到数据文件中

F. 将特定的行复制到数据文件中

G. 将查询中的数据复制到数据文件中

H. 创建格式化文件

I. 使用格式化文件进行 **bcp** 批量导入

J. 指定代码页

### <a name="example-test-conditions"></a>**示例测试条件**

以下示例使用 SQL Server（从 2016 开始）和 Azure SQL 数据库的 `WideWorldImporters` 示例数据库。  可以通过 [https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0) 下载 `WideWorldImporters` 。  有关用于还原示例数据库的语法，请参阅 [RESTORE (Transact-SQL)](../t-sql/statements/restore-statements-transact-sql.md) 。  除了另行指定的位置，该示例假定你使用 Windows 身份验证，并且与运行 **bcp** 命令所针对的服务器实例之间具有可信连接。  会在许多示例中使用一个名为 `D:\BCP` 的目录。

下面的脚本创建 `WideWorldImporters.Warehouse.StockItemTransactions` 表的空副本，然后添加主键约束。  在 SQL Server Management Studio (SSMS) 中运行以下 T-SQL 脚本

```sql  
USE WideWorldImporters;  
GO  

SET NOCOUNT ON;

IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'Warehouse.StockItemTransactions_bcp')
BEGIN
    SELECT * INTO WideWorldImporters.Warehouse.StockItemTransactions_bcp
    FROM WideWorldImporters.Warehouse.StockItemTransactions  
    WHERE 1 = 2;  

    ALTER TABLE Warehouse.StockItemTransactions_bcp 
    ADD CONSTRAINT PK_Warehouse_StockItemTransactions_bcp PRIMARY KEY NONCLUSTERED 
    (StockItemTransactionID ASC);
END
```

> [!NOTE]
> 按需截断 `StockItemTransactions_bcp` 表。
>
> TRUNCATE TABLE WideWorldImporters.Warehouse.StockItemTransactions_bcp;

### <a name="a--identify-bcp-utility-version"></a>A.  标识 **bcp** 实用工具版本

在命令提示符处输入以下命令：

```cmd
bcp -v
```
  
### <a name="b-copying-table-rows-into-a-data-file-with-a-trusted-connection"></a>B. 将表行复制到数据文件中（使用可信连接）

以下示例阐释了 `WideWorldImporters.Warehouse.StockItemTransactions` 表中的 out 选项。

- **基本** 此示例创建一个名为 `StockItemTransactions_character.bcp` 的数据文件，并使用字符格式将表数据复制到该文件中。

  在命令提示符处输入以下命令：

  ```cmd
  bcp WideWorldImporters.Warehouse.StockItemTransactions out D:\BCP\StockItemTransactions_character.bcp -c -T
  ```

- “Expanded”。此示例创建一个名为 `StockItemTransactions_native.bcp` 的数据文件，并使用“本机”格式将表数据复制到该文件中 。  此示例还指定最大语法错误数、一个错误文件和一个输出文件。

    在命令提示符处输入以下命令：

    ```cmd
    bcp WideWorldImporters.Warehouse.StockItemTransactions OUT D:\BCP\StockItemTransactions_native.bcp -m 1 -n -e D:\BCP\Error_out.log -o D:\BCP\Output_out.log -S -T
    ```

查看 `Error_out.log` 和 `Output_out.log`。  `Error_out.log` 应为空白。  比较 `StockItemTransactions_character.bcp` 与 `StockItemTransactions_native.bcp`之间的文件大小。

### <a name="c-copying-table-rows-into-a-data-file-with-mixed-mode-authentication"></a>C. 将表行复制到数据文件中（使用混合模式身份验证）

以下示例阐释了 **表中的** out `WideWorldImporters.Warehouse.StockItemTransactions` 选项。  此示例创建一个名为 `StockItemTransactions_character.bcp` 的数据文件，并使用 **字符** 格式将表数据复制到该文件中。  
  
 该示例假定你使用混合模式身份验证，必须使用 **-U** 开关指定登录 ID。 并且，除非你连接到本地计算机上 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的默认实例，否则请使用 **-S** 开关指定系统名称和实例名称（可选）。  

在命令提示符处输入以下命令：\(系统将提示你输入密码。\)

```cmd
bcp WideWorldImporters.Warehouse.StockItemTransactions out D:\BCP\StockItemTransactions_character.bcp -c -U<login_id> -S<server_name\instance_name>
```

### <a name="d-copying-data-from-a-file-to-a-table"></a>D. 将文件中的数据复制到表中

以下示例使用上面创建的文件说明 **表中的** in `WideWorldImporters.Warehouse.StockItemTransactions_bcp` 选项。

- **基本** 此示例使用以前创建的 `StockItemTransactions_character.bcp` 数据文件。

  在命令提示符处输入以下命令：

  ```cmd
  bcp WideWorldImporters.Warehouse.StockItemTransactions_bcp IN D:\BCP\StockItemTransactions_character.bcp -c -T
  ```

- **展开** 此示例使用以前创建的 `StockItemTransactions_native.bcp` 数据文件。  此示例还使用提示 **TABLOCK**，指定最大语法错误数、一个错误文件和一个输出文件。
  
在命令提示符处输入以下命令：

```cmd
bcp WideWorldImporters.Warehouse.StockItemTransactions_bcp IN D:\BCP\StockItemTransactions_native.bcp -b 5000 -h "TABLOCK" -m 1 -n -e D:\BCP\Error_in.log -o D:\BCP\Output_in.log -S -T
```

  查看 `Error_in.log` 和 `Output_in.log`。

### <a name="e-copying-a-specific-column-into-a-data-file"></a>E. 将特定的列复制到数据文件中

若要复制特定列，可以使用 **queryout** 选项。  下面的示例仅将 `StockItemTransactionID` 表中的 `Warehouse.StockItemTransactions` 列复制到数据文件中。
  
在命令提示符处输入以下命令：

```cmd
bcp "SELECT StockItemTransactionID FROM WideWorldImporters.Warehouse.StockItemTransactions WITH (NOLOCK)" queryout D:\BCP\StockItemTransactionID_c.bcp -c -T
```

### <a name="f-copying-a-specific-row-into-a-data-file"></a>F. 将特定的行复制到数据文件中

若要复制特定行，可以使用 **queryout** 选项。 以下示例仅将名为 `Amy Trefl` 的人员行从 `WideWorldImporters.Application.People` 表复制到数据文件 `Amy_Trefl_c.bcp`中。  注意： **-d** 开关用于标识数据库。
  
在命令提示符处输入以下命令：

```cmd
bcp "SELECT * from Application.People WHERE FullName = 'Amy Trefl'" queryout D:\BCP\Amy_Trefl_c.bcp -d WideWorldImporters -c -T
```

### <a name="g-copying-data-from-a-query-to-a-data-file"></a>G. 将查询中的数据复制到数据文件中

若要将 Transact-SQL 语句的结果集复制到数据文件中，请使用 **queryout** 选项。  下面的示例将 `WideWorldImporters.Application.People` 表中的姓名复制到 `People.txt` 数据文件中；这些姓名按全名排序。  注意：-t 开关用于创建逗号分隔文件。

在命令提示符处输入以下命令：

```cmd
bcp "SELECT FullName, PreferredName FROM WideWorldImporters.Application.People ORDER BY FullName" queryout D:\BCP\People.txt -t, -c -T
```

### <a name="h-creating-format-files"></a>H. 创建格式化文件

下面的示例为 `Warehouse.StockItemTransactions` 数据库中的 `WideWorldImporters` 表创建三个不同格式文件。  查看创建的每个文件的内容。

在命令提示符处输入以下命令：

```cmd
REM non-XML character format
bcp WideWorldImporters.Warehouse.StockItemTransactions format nul -f D:\BCP\StockItemTransactions_c.fmt -c -T 

REM non-XML native format
bcp WideWorldImporters.Warehouse.StockItemTransactions format nul -f D:\BCP\StockItemTransactions_n.fmt -n -T

REM XML character format
bcp WideWorldImporters.Warehouse.StockItemTransactions format nul -f D:\BCP\StockItemTransactions_c.xml -x -c -T
```

> [!NOTE]
> 若要使用 **-x** 开关，则必须使用 **bcp** 9.0 客户端。 有关如何使用 **bcp** 9.0 客户端的信息，请参阅“[备注](#remarks)”。
  
 有关详细信息，请参阅[非 XML 格式化文件 (SQL Server)](../relational-databases/import-export/non-xml-format-files-sql-server.md) 和[XML 格式化文件 (SQL Server)](../relational-databases/import-export/xml-format-files-sql-server.md)。
  
### <a name="i-using-a-format-file-to-bulk-import-with-bcp"></a>I. 使用格式化文件进行 bcp 批量导入

向 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的实例中导入数据时，若要使用以前创建的格式化文件，请同时使用 **-f** 开关和 **in** 选项。  例如，以下命令通过使用以前创建的格式化文件 ( `StockItemTransactions_character.bcp`)，将数据文件 `Warehouse.StockItemTransactions_bcp` 的内容大容量复制到 `StockItemTransactions_c.xml`表的副本中。  注意： **-L** 开关用于仅导入前 100 个记录。

在命令提示符处输入以下命令：

```cmd
bcp WideWorldImporters.Warehouse.StockItemTransactions_bcp in D:\BCP\StockItemTransactions_character.bcp -L 100 -f D:\BCP\StockItemTransactions_c.xml -T
```

> [!NOTE]
> 如果数据文件字段和表中的列不同（例如，在编号、排序或数据类型方面），则可使用格式化文件。 有关详细信息，请参阅 [用来导入或导出数据的格式化文件 (SQL Server)](../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)。  

### <a name="j-specifying-a-code-page"></a>J. 指定代码页

以下不完整的代码示例显示了指定代码页 65001 时使用的 bcp import 语句：

```cmd
bcp.exe MyTable in "D:\data.csv" -T -c -C 65001 -t , ...  
```

## <a name="additional-examples"></a>其他示例

|以下主题包含有关使用 bcp的示例： |
|---|
|用于批量导入或导出的数据格式 (SQL Server)<br />&emsp;&#9679;&emsp;[使用本机格式导入或导出数据 (SQL Server)](../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[使用字符格式导入或导出数据 (SQL Server)](../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[使用 Unicode 本机格式导入或导出数据 (SQL Server)](../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[使用 Unicode 字符格式导入或导出数据 (SQL Server)](../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)<br /><br />[指定字段终止符和行终止符 (SQL Server)](../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)<br /><br />[在批量导入期间保留 Null 或使用默认值 (SQL Server)](../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)<br /><br />[大容量导入数据时保留标识值 (SQL Server)](../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)<br /><br />用于导入或导出数据的格式化文件 (SQL Server)）<br />&emsp;&#9679;&emsp;[创建格式化文件 (SQL Server)](../relational-databases/import-export/create-a-format-file-sql-server.md)<br />&emsp;&#9679;&emsp;[使用格式化文件批量导入数据 (SQL Server)](../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)<br />&emsp;&#9679;&emsp;[使用格式化文件跳过表列 (SQL Server)](../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)<br />&emsp;&#9679;&emsp;[使用格式化文件跳过数据字段 (SQL Server)](../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)<br />&emsp;&#9679;&emsp;[使用格式化文件将表列映射到数据文件字段 (SQL Server)](../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)<br /><br />[批量导入和导出 XML 文档的示例 (SQL Server)](../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)<br /><p>  </p>|

## <a name="considerations-and-limitations"></a>注意事项和限制

- BCP 实用工具有一个限制，即错误消息仅显示 512 字节字符。 仅显示错误消息的前 512 个字节。

## <a name="next-steps"></a>后续步骤

- [准备用于批量导出或导入的数据 (SQL Server)](../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)
- [BULK INSERT (Transact-SQL)](../t-sql/statements/bulk-insert-transact-sql.md)
- [OPENROWSET (Transact-SQL)](../t-sql/functions/openrowset-transact-sql.md)
- [SET QUOTED_IDENTIFIER (Transact-SQL)](../t-sql/statements/set-quoted-identifier-transact-sql.md)
- [sp_configure &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_tableoption (Transact-SQL)](../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)
- [用来导入或导出数据的格式化文件 (SQL Server)](../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
