---
title: dwloader Command-Line 加载程序
description: dwloader 是并行数据仓库 (PDW) 命令行工具，它将表行批量加载到现有表中。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 3635aff3c3dad371c969acd3d72b2fb738748ecc
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489687"
---
# <a name="dwloader-command-line-loader-for-parallel-data-warehouse"></a>并行数据仓库的 dwloader Command-Line 加载程序
**dwloader** 是并行数据仓库 (PDW) 命令行工具，它将表行批量加载到现有表中。 加载行时，可以将所有行添加到表的末尾 (*追加模式* 或 *fastappend 模式*) ，追加新行并更新 (*upsert 模式下* 的现有行) ，或在加载前删除所有现有行，然后将所有行插入到空表 (*重载模式*) 。  
  
**用于加载数据的进程**  
  
1.  准备源数据。  
  
    使用自己的 ETL 过程创建要加载的源数据。 必须将源数据的格式设置为与目标表的架构相匹配。 将源数据存储到一个或多个文本文件中，并将文本文件复制到加载服务器上的同一目录中。 有关加载服务器的信息，请参阅 [获取和配置加载服务器](acquire-and-configure-loading-server.md)  
  
2.  准备加载选项。  
  
    确定将使用的加载选项。 将加载选项存储在配置文件中。 将配置文件复制到加载服务器上的本地位置。 本主题介绍了 **dwloader** 配置选项。  
  
3.  准备加载失败选项。  
  
    决定 **dwloader** 如何处理未能加载的行。 若要执行加载， **dwloader** 首先将数据加载到临时表中，然后将数据传输到目标表。 当加载器将数据加载到临时表中时，它将跟踪未能加载的行数。 例如，未正确格式化的行将无法加载。 Failed 行将复制到拒绝文件。 默认情况下，负载在第一次拒绝后中止，除非你指定不同的拒绝阈值。  
  
4.  安装 **dwloader**。  
  
    如果尚未安装 dwloader，请将其安装到加载服务器。 

<!-- MISSING LINK
    See [Install dwloader Command-Line Loader &#40;SQL Server PDW&#41;](install-dwloader-command-line-loader-sql-server-pdw.md). 
--> 
  
5.  运行 **dwloader**。  
  
    登录到加载服务器，然后用适当的命令行选项运行可执行文件 **dwloader.exe** 。  
  
6.  验证结果。  
  
    您可以检查失败的行文件 (通过-R) 指定，以查看是否有任何行加载失败。 如果此文件为空，则所有行都已成功加载。 **dwloader** 是事务性的，因此，如果任何步骤失败 (不) 拒绝的行，则所有步骤将回滚到它们的初始状态。  
  
<!-- 
![Topic link icon](media/topic-link.gif "Topic_Link")[Syntax Conventions](syntax-conventions-sql-server-pdw.md)  
-->  


## <a name="syntax"></a>语法  
  
```  
dwloader.exe { -h }  
  
dwloader.exe   
    {  
        { -U login_name  -P password  }  
        | -W  
    }  
    [ -f parameter_file ]  
    [ -S target_appliance ]  
    { -T target_database_name . [ schema ] . table_name }   
    { -i source_data_location } [ <source_data_options> ]  
    { -R load_failure_file_name } [ <load_failure_options> ]  
    [ <loading_options> ]  
}  
  
<source_data_options> ::=  
{  
    [ -fh number_header_rows ]  
    [ < variable_length_column_options > | < fixed_width_column_options > ]  
    [ -D { mdy | myd | ymd | ydm | dmy | dym | custom_date_format } ]  
    [ -dt datetime_format_file ]  
}  
  
<variable_length_column_options> ::=  
{  
    [ -e character_encoding ]  
    -r row_delimiter   
    [ -s string_delimiter ]  
    -t field_delimiter   
}  
  
<fixed_width_column_options> ::=  
{  
    -w fixed_width_config_file   
    [ -e character_encoding ]   
    -r row_delimiter   
}  
  
<load_failure_options> ::=  
{  
    [ -rt { value | percentage } ]  
    [ -rv reject_value ]  
    [ -rs reject_sample_value ]  
}  
  
<loading_options> ::=  
{  
    [ -d staging_database_name ]  
    [ -M { append | fastappend | upsert -K merge_column [ ,...n ] | reload } ]  
    [ -b batchsize ]   
    [ -c ]  
    [ -E ]  
    [ -m ]  
    [ -N ]  
    [ -se ]
    [ -l ]   
}  
```  
  
## <a name="arguments"></a>自变量  
-h  
显示有关使用加载程序的简单帮助信息。 仅当未指定其他命令行参数时，才会显示帮助。  
  
**-U** *login_name*  
有效的 SQL Server 身份验证登录名，具有适当的权限来执行负载。  
  
-P password  
SQL Server Authentication *login_name* 的密码。  
  
**-W**  
使用 Windows 身份验证。  (无需 *login_name* 或 *密码* 。 )  

<!-- MISSING LINK
For information about configuring Windows Authentication, see [Security - Configure Domain Trusts](security-configure-domain-trusts.md).  
-->
  
**-f** *parameter_file_name*  
使用参数文件 *parameter_file_name* 来代替命令行参数。 *parameter_file_name* 可以包含除 *user_name* 和 *password* 以外的任何命令行参数。 如果在命令行和参数文件中指定了参数，则命令行将覆盖 file 参数。  
  
参数文件包含一个参数， **-** 每行不含前缀。  
  
示例:  
  
`rt=percentage`  
  
`rv=25`  
  
**-S** *target_appliance*  
指定将接收已加载数据的 SQL Server PDW 设备。  
  
*对于 "未实现连接*"，请将 *target_appliance* 指定为 <的设备名称>-SQLCTL01。 若要配置此命名连接，请参阅 [配置无限的网络适配器](configure-infiniband-network-adapters.md)。  
  
对于以太网连接， *target_appliance* 是控制节点群集的 IP 地址。  
  
如果省略，则 dwloader 默认为安装 dwloader 时指定的值。 

<!-- MISSING LINK
For more information about this install option, see [Install dwloader Command-Line Loader](install-dwloader.md).  
-->
  
**-T** *target_database_name。*[*架构*]。*table_name*  
目标表的由三部分构成的名称。  
  
**-I** *source_data_location*  
要加载的一个或多个源文件的位置。 每个源文件都必须是使用 gzip 进行压缩的文本文件或文本文件。 只有一个源文件可以压缩到每个 gzip 文件中。  
  
设置源文件的格式：  
  
-   必须根据加载选项设置源文件的格式。  
  
-   源文件中的每行都包含一个表行的数据。 源数据必须与目标表的架构匹配。 列顺序和数据类型也必须匹配。 该行中的每个字段都表示目标表中的列。  
  
-   默认情况下，字段是可变长度并由分隔符分隔。 若要指定分隔符类型，请使用 <variable_length_column_options> 的命令行选项。 若要指定固定长度字段，请使用 <fixed_width_column_options> 的命令行选项。  
  
指定源数据位置：  
  
-   源数据位置可以是网络路径，也可以是加载服务器上的目录的本地路径。  
  
-   若要指定目录中的所有文件，请输入目录路径，后面跟有 * 通配符。  加载程序不会加载源数据位置中的任何子目录中的文件。 当某个目录存在于 gzip 文件中时，加载程序错误。  
  
-   若要指定目录中的某些文件，请使用字符和 * 通配符的组合。  
  
若要加载包含一个命令的多个文件：  
  
-   所有文件都必须位于同一目录中。  
  
-   文件必须是所有文本文件、所有 gzip 文件，或是文本和 gzip 文件的组合。  
  
-   所有文件都不能包含标头信息。  
  
-   所有文件都必须使用相同的字符编码类型。 请参阅-e 选项。  
  
-   所有文件都必须加载到同一个表中。  
  
-   所有文件将被连接起来，并将其加载，就好像它们是一个文件，被拒绝的行会转向单个拒绝文件。  
  
示例:  
  
-   -i \\ \loadserver\loads\daily \\ *. gz  
  
-   -i \\ \loadserver\loads\daily \\ * .txt  
  
-   -i \\ \loadserver\loads\daily\monday. *  
  
-   -i \\\loadserver\loads\daily\monday.txt  
  
-   -i \\ \loadserver\loads\daily\\*  
  
**-R** *load_failure_file_name*  
如果存在加载失败， **dwloader** 将存储未能加载的行，并且失败说明中名为 *load_failure_file_name* 的文件中的失败信息。 如果此文件已存在，dwloader 将覆盖现有文件。 *load_failure_file_name* 是在第一次失败时创建的。 如果所有行都成功加载，则不会创建 *load_failure_file_name* 。  
  
**-fh** *number_header_rows*  
*Source_data_file_name* 开始时要忽略 (行) 行数。 默认值为 0。  
  
<variable_length_column_options>  
具有字符分隔可变长度列的 *source_data_file_name* 的选项。 默认情况下， *source_data_file_name* 包含可变长度列中的 ASCII 字符。  
  
对于 ASCII 文件，通过将分隔符分隔来表示 Null。 例如，在带管道的文件中 ( "|") ，则 "| |" 指示 NULL 值。 在以逗号分隔的文件中，空值由 "，，" 指示。 此外，必须指定 **-E** (--emptyStringAsNull) 选项。 有关-E 的详细信息，请参阅下文。  
  
**-e** *character_encoding*  
指定要从数据文件中加载的数据的字符编码类型。 选项为 ASCII (默认值) 、UTF8、UTF16 或 UTF16BE，其中 UTF16 为 little endian，UTF16BE 为大字节序。 这些选项不区分大小写。  
  
**-t** *field_delimiter*  
) 行中每个字段 (列的分隔符。 字段分隔符是其中一个或多个 ASCII 转义符或 ASCII 十六进制值。  
  
|名称|转义字符|十六进制字符|  
|--------|--------------------|-----------------|  
|选项卡|\t|0x09|  
|回车符 (CR) |\r|0x0d|  
|换行符 (LF) |\n|0x0a|  
|CRLF|！|0x0d0x0a|  
|逗号|','|0x2c|  
|双引号|\\"|0x22|  
|单引号|\\'|0x27|  
  
若要在命令行中指定管道字符，请将其括在双引号 "|" 中。 这将避免命令行分析器解释。 其他字符用单引号括起来。  
  
示例:  
  
-t "|"  
  
-t ""  
  
-t 0x0a  
  
-t \t  
  
-t "~ | ~"  
  
**-r** *row_delimiter*  
源数据文件中每行的分隔符。 行分隔符是一个或多个 ASCII 值。  
  
若要指定回车符 (CR) 、换行符 (LF) 或制表符作为分隔符，可以使用转义字符 ( \r、\n、\t) 或其十六进制值 (0x、0d、09) 。 若要将任何其他特殊字符指定为分隔符，请使用其十六进制值。  
  
CR + LF 的示例：  
  
-r \r\n  
  
-r 0x0d0x0a  
  
CR 的示例：  
  
-r \r  
  
-r 0x0d  
  
LF 的示例：  
  
-r \n  
  
-r 0x0a  
  
需要使用 LF 作为 Unix。 Windows 需要 CR。  
  
**-s** *string_delimiter*  
用文本分隔的输入文件的 string 数据类型字段的分隔符。 字符串分隔符是一个或多个 ASCII 值。  它可以指定为字符 (例如，-s * ) 或十六进制值 (例如，-s 0x22 对于双引号) 。  
  
示例:  
  
些  
  
-s 0x22  
  
< fixed_width_column_options>  
具有固定长度列的源数据文件的选项。 默认情况下， *source_data_file_name* 包含可变长度列中的 ASCII 字符。  
  
如果-e 为 UTF8，则不支持固定宽度列。  
  
**-w** *fixed_width_config_file*  
配置文件的路径和名称，该配置文件指定每列中的字符数。 必须指定每个字段。  
  
此文件必须位于加载服务器上。 路径可以是 UNC、相对路径或绝对路径。 *Fixed_width_config_file* 中的每一行都包含一个列的名称和该列的字符数。 每个列都有一个行，如下所示，文件中的顺序必须与目标表中的顺序匹配：  
  
*column_name* =*num_chars*  
  
*column_name* =*num_chars*  
  
固定宽度配置文件示例：  
  
SalesCode = 3  
  
SalesID = 10  
  
*Source_data_file_name* 中的示例行：  
  
230Shirts0056  
  
320Towels1356  
  
在上面的示例中，第一个加载行的 SalesCode 为 "230"，SalesID = "Shirts0056"。 第二个加载的行将具有 SalesCode = ' 320 ' 和 SaleID = ' Towels1356 '。  
  
有关如何在固定宽度模式下处理前导空格和尾随空格或数据类型转换的信息，请参阅 [dwloader 的数据类型转换规则](dwloader-data-type-conversion-rules.md)。  
  
**-e** *character_encoding*  
指定要从数据文件中加载的数据的字符编码类型。 选项为 ASCII (默认值) 、UTF8、UTF16 或 UTF16BE，其中 UTF16 为 little endian，UTF16BE 为大字节序。 这些选项不区分大小写。  
  
如果-e 为 UTF8，则不支持固定宽度列。  
  
**-r** *row_delimiter*  
源数据文件中每行的分隔符。 行分隔符是一个或多个 ASCII 值。  
  
若要指定回车符 (CR) 、换行符 (LF) 或制表符作为分隔符，可以使用转义字符 ( \r、\n、\t) 或其十六进制值 (0x、0d、09) 。 若要将任何其他特殊字符指定为分隔符，请使用其十六进制值。  
  
CR + LF 的示例：  
  
-r \r\n  
  
-r 0x0d0x0a  
  
CR 的示例：  
  
-r \r  
  
-r 0x0d  
  
LF 的示例：  
  
-r \n  
  
-r 0x0a  
  
需要使用 LF 作为 Unix。 Windows 需要 CR。  
  
**-D** { **ymd** \| ydm \| mdy \| myd \| dmy \| dym \| *custom_date_format* }  
指定输入文件中所有日期时间字段的月 (m) ，day (d) ，年份 (y) 的顺序。 默认顺序为 ymd。 若要为同一源文件指定多个订单格式，请使用-dt 选项。  
  
ymd \| dmy  
ydm 和 dmy 允许相同的输入格式。 两者都允许在日期的开始或结束时间。 例如，对于 **ydm** 和 **dmy** 日期格式，输入文件中可能有2013-02-03 或02-03-2013。  
  
ydm  
只能将格式化为 ydm 的输入加载到数据类型为 datetime 和 smalldatetime 的列中。 不能将 ydm 值加载到 datetime2、date 或 datetimeoffset 数据类型的列中。  
  
mdy  
mdy 允许 \<month> \<space> \<day> \<comma> \<year> 。  
  
1975年1月1日 mdy 输入数据的示例：  
  
-   1975年1月1日  
  
-   Jan 01，75  
  
-   Jan/1/75  
  
-   01011975  
  
myd  
2010年3月4日的输入文件示例：03-2010-04、3/2010/4  
  
dym  
2010年3月的输入文件示例，：04-2010-03，4/2010/3  
  
*custom_date_format*  
*custom_date_format* 是一种自定义日期格式 (例如，MM/dd/yyyy) ，并且仅为向后兼容性提供。 dwloader 不强制执行自定义日期格式。 相反，当指定自定义日期格式时， **dwloader** 会将其转换为 ymd、ydm、mdy、myd、dym 或 dmy 的相应设置。  
  
例如，如果指定-D MM/dd/yyyy，则 dwloader 预计所有日期输入都首先按月进行排序，然后按年 (mdy) 。 它不会强制使用自定义日期格式指定的2个字符月、2位数的日期和4位数年份。 以下示例介绍了日期格式为-D MM/dd/yyyy：01/02/2013、02.2013、1/2/2013 时，如何在输入文件中设置日期格式：  
  
有关更全面的格式设置信息，请参阅 [dwloader 的数据类型转换规则](dwloader-data-type-conversion-rules.md)。  
  
**-dt** *datetime_format_file*  
在名为 *datetime_format_file* 的文件中指定每个日期时间格式。 与命令行参数不同，包含空格的文件参数不得用双引号引起来。 在加载数据时，不能更改日期时间格式。 源数据文件与其在目标表中的对应列必须具有相同的格式。  
  
每行都包含目标表中列的名称及其日期时间格式。  
  
示例:  
  
`LastReceiptDate=ymd`  
  
`ModifiedDate=dym`  
  
**-d** *staging_database_name*  
将包含临时表的数据库名称。 默认值为通过-T 选项指定的数据库，它是目标表的数据库。 有关使用临时数据库的详细信息，请参阅 [创建临时数据库](staging-database.md)。  
  
**-M** *load_mode_option*  
指定是否追加、upsert 或重新加载数据。 默认模式为 append。  
  
append  
加载程序将行插入到目标表中的现有行的末尾。  
  
fastappend  
加载程序直接将行插入到目标表中的现有行的末尾，而不是使用临时表。 fastappend 需要多事务 (-m) 选项。 使用 fastappend 时，不能指定临时数据库。 没有 fastappend 的回滚，这意味着从失败或中止的负载中进行的恢复必须由您自己的加载进程处理。  
  
upsert *merge_column* [,.。。*n* ]    
加载器使用 SQL Server Merge 语句来更新现有行和插入新行。  
  
-K 选项指定合并时所基于的一个或多个列。 这些列构成一个合并关键字，它应表示唯一行。 如果目标表中存在合并关键字，则更新该行。 如果目标表中不存在合并关键字，则追加行。  
  
对于哈希分布表，合并关键字必须是或包含分布列。  
  
对于复制的表，合并键是一列或多列的组合。 根据应用程序的需求指定这些列。  
  
多个列必须以逗号分隔，且不能包含空格，也不能以空格分隔，并用单引号括起来。  
  
如果源表中的两行具有匹配的合并键值，则它们各自的行必须相同。  
  
刷新  
加载程序在插入源数据前截断目标表。  
  
**-b** *batchsize*  
建议仅 Microsoft 支持部门使用， *batchsize* 是 Dm 在计算节点上的 SQL Server 实例中执行的大容量复制的 SQL Server 批大小。  当指定为 *batchsize* 时，SQL Server PDW 将覆盖为每个负载动态计算的批处理负载大小。  
  
从 SQL Server 2012 PDW 开始，默认情况下，控件节点会动态计算每个负载的批大小。 此自动计算基于几个参数，如内存大小、目标表类型、目标表架构、加载类型、文件大小以及用户的资源类。  
  
例如，如果加载模式为 FASTAPPEND，并且该表具有聚集列存储索引，则默认情况下，SQL Server PDW 将尝试使用批大小1048576，以便行组将变为关闭状态并直接加载到列存储中，而无需通过增量存储区。 如果内存不允许批大小1048576，dwloader 将选择较小的 batchsize。  
  
如果负载类型为 FASTAPPEND，则 *batchsize* 适用于将数据加载到表中，否则 *batchsize* 适用于将数据加载到临时表中。  
  
<reject_options>  
指定用于确定加载程序将允许的加载失败次数的选项。 如果加载失败超出阈值，加载程序将暂停，并且不提交任何行。  
  
**-rt** { **value** | 百分率}  
指定 **-rv** *reject_value* 选项中的-*reject_value* 是 (值的文本行数) 还是失败率 (百分比) 。 默认值为。  
  
百分比选项是按-rs 选项间隔发生的实时计算。  
  
例如，如果加载程序尝试加载100行，25个失败并且75成功，则失败率为25%。  
  
**-rv** *reject_value*  
指定在停止负载之前允许的行否决的数目或百分比。 **-Rt** 选项决定 *reject_value* 是指行数还是行数百分比。  
  
默认 *reject_value* 为0。  
  
当与 rt 值一起使用时，加载程序会在拒绝的行计数超出 reject_value 时停止负载。  
  
使用-rt 百分比时，加载程序将按间隔 (-rs 选项) 计算百分比。 因此，失败行的百分比可能会超出 *reject_value*。  
  
**-rs** *reject_sample_size*  
与用于 `-rt percentage` 指定增量百分比检查的选项一起使用。 例如，如果 reject_sample_size 为1000，加载程序将在尝试加载1000行后计算失败行的百分比。 它会在尝试加载每个附加的1000行后重新计算失败行的百分比。  
  
**-c**  
删除 char、nchar、varchar 和 nvarchar 字段的左侧和右侧的空白字符。 将仅包含空格字符的每个字段转换为空字符串。  
  
示例:  
  
"" 截断为 ""  
  
"abc" 截断为 "abc"  
  
当-c 与-E 一起使用时，-E 操作首先发生。 只包含空格字符的字段会转换为空字符串，而不会转换为 NULL。  
  
**-E**  
将空字符串转换为 NULL。 默认为不执行这些转换。  
  
**-m**  
在加载的第二个阶段使用多事务模式;将数据从临时表加载到分布式表时。  
  
对于 **-m**，SQL Server PDW 并行执行和提交负载。 这比默认加载模式执行得更快，但并不是事务安全的。  
  
不带 **-m** 的 SQL Server PDW 会跨每个计算节点内的分布，同时跨计算节点执行和提交负载。 此方法比多事务模式慢，但是事务安全的。  
  
**-m** 对于 *append*、 *reload.sql* 和 *upsert* 是可选的。  
  
**-m** 对于 fastappend 是必需的。  
  
**-m** 不能与复制的表一起使用。  
  
**-m** 仅适用于第二个加载阶段。 它不适用于第一个加载阶段;正在将数据加载到临时表中。  
  
不存在多事务模式的回滚，这意味着从失败或中止的负载中进行的恢复必须由您自己的加载进程处理。  
  
建议仅在装入空表时使用 **-m** ，以便在不丢失数据的情况下进行恢复。 从加载失败中恢复：删除目标表，解决加载问题，重新创建目标表，然后再次运行加载。  
  
**-N**  
验证目标设备是否具有来自受信任的颁发机构的有效 SQL Server PDW 证书。 使用此方法有助于确保你的数据不被攻击者截获并发送到未经授权的位置。 此证书必须已安装在设备上。 安装证书的唯一受支持的方法是让设备管理员使用 Configuration Manager 工具安装它。 如果你不确定设备是否安装了受信任的证书，请询问你的设备管理员。  
  
**-se**  
跳过加载空文件。 这也会跳过解压缩空的 gzip 文件。

**-l**  
可与 CU 7.4 更新一起使用，以字节为单位指定 () 可加载的最大行长度。 有效值为介于32768和33554432之间的整数。 仅在需要加载 (大于 32KB) 的大型行时使用，因为这会在客户端和服务器上分配更多内存。
  
## <a name="return-code-values"></a>返回代码值  
0 (成功) 或其他整数值 (失败)   
  
在命令窗口或批处理文件中，使用 `errorlevel` 显示返回代码。 例如：  
  
```  
dwloader  
echo ReturnCode=%errorlevel%  
if not %errorlevel%==0 echo Fail  
if %errorlevel%==0 echo Success  
```  
  
使用 PowerShell 时，请使用 `$LastExitCode` 。  
  
## <a name="permissions"></a>权限  
 (在目标表中插入、更新、删除) 时需要加载权限和适用的权限。 需要创建权限 (以便在临时数据库中创建临时表) 。 如果未使用临时数据库，则需要在目标数据库上创建权限。 

<!-- MISSING LINK
For more information, see [Grant permissions to load data](grant-permissions-to-load-data.md).  
-->
  
## <a name="general-remarks"></a>一般备注  
有关 dwloader 加载时的数据类型转换的信息，请参阅 [dwloader 的数据类型转换规则](dwloader-data-type-conversion-rules.md)。  
  
如果参数包含一个或多个空格，请将参数用双引号引起来。  
  
应该从安装的位置运行加载程序。 Dwloader 可执行文件已预安装到设备上，位于 C:\Program Files\Microsoft SQL Server Data Warehouse\DWLoader 目录中。  
  
可以通过将参数文件中指定的参数指定为命令行参数来覆盖参数文件中指定的参数 (-f 选项) 。  
  
可以同时运行加载程序的多个实例。 加载器实例的最大数目是预配置的，无法更改。 

<!-- MISSING LINK
For the maximum number of loads per appliance, see [Minimum and Maximum Values](minimum-maximum-values.md)  
-->
  
加载的数据在设备上可能需要比源位置更多或更少的空间。 可以执行包含数据子集的测试导入来估算磁盘使用情况。  
  
尽管 **dwloader** 是一个事务进程，并将在失败时正常回滚，但一旦成功完成大容量加载，就不能将其回滚。 若要取消活动的 **dwloader** 进程，请键入 CTRL + C。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
并发发生的所有加载的总大小必须小于数据库的 LOG_SIZE，建议所有并发加载的总大小小于 LOG_SIZE 的50%。 若要实现此大小限制，可将大负载拆分为多个批处理。 有关 LOG_SIZE 的详细信息，请参阅 [创建数据库](../t-sql/statements/create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)  
  
在加载包含一个 load 命令的多个文件时，所有拒绝的行将写入同一拒绝文件。 拒绝文件不显示哪个输入文件包含每个被拒绝行。  
  
空字符串不应用作分隔符。 当空字符串用作行分隔符时，加载将失败。 当用作列分隔符时，加载将忽略分隔符，并继续使用默认值 "|" 作为列分隔符。 当用作字符串分隔符时，将忽略空字符串并应用默认行为。  
  
## <a name="locking-behavior"></a>锁定行为  
**dwloader** 锁定行为根据 *load_mode_option* 的不同而异。  
  
-   建议使用 **追加** 追加，这是最常用的选项。 追加将数据加载到临时表中。 下面将详细介绍锁定。  
  
-   **快速追加** -以快速方式将加载追加到 ExclusiveUpdate 表锁的最终表中，这是不使用临时表的唯一模式。  
  
-   **重载** -重新加载将数据加载到临时表中，并在临时表和最终表上都需要排他锁。 不建议对并发操作执行重载。  
  
-   **upsert** -upsert 将数据加载到临时表中，然后执行从临时表到最终表的合并操作。 Upsert 无需对最终表进行排他锁。 使用 upsert 时，性能可能会有所不同。 测试环境中的行为。  
  
### <a name="locking-behavior"></a>锁定行为  
**追加模式锁定**  
  
追加可以在使用-m 参数)  (多事务性模式下运行，但它不是事务安全的。 因此追加应用作事务性操作，而不使用-m 参数)  (。 遗憾的是，在最后的 INSERT SELECT 操作过程中，事务模式当前大约比多事务性模式慢六倍。  
  
追加模式分两个阶段加载数据。 第一阶段会将源文件中的数据加载到临时表中， (碎片可能会) 。 阶段2：将数据从临时表加载到最终表。 第二个阶段执行 **INSERT INTO .。。选择 WITH (TABLOCK)** 操作。 下表显示了对最终表的锁定行为以及使用追加模式时的日志记录行为：  
  
|表类型|多事务<br />模式 (-m) |表为空|支持并发|日志记录|  
|--------------|-----------------------------------|------------------|-------------------------|-----------|  
|堆|是|是|是|最少|  
|堆|是|否|是|最少|  
|堆|否|是|否|最少|  
|堆|否|否|否|最少|  
|Cl|是|是|否|最少|  
|Cl|是|否|是|完整|  
|Cl|否|是|否|最少|  
|Cl|否|否|是|完整|  
  
上表显示了 **dwloader** ，使用追加模式加载到堆或聚集索引 (CI) 表，其中包含或不包含多事务性标志，并且加载到空表或非空表中。 表中显示了每个这种负载组合的锁定和日志记录行为。 例如，使用追加模式将 (第 2) 阶段加载到聚集索引中，而不是使用多事务性模式，而在空表中，将在表中创建排他锁，并且日志记录是最少的。 这意味着，客户将无法加载 (第二) 阶段并同时查询到空表中。 但是，在非空表中加载具有相同配置的时，PDW 不会对表发出排他锁，并且可能会发生并发。 遗憾的是，发生了完整的日志记录，从而降低了处理速度。  
  
## <a name="examples"></a>示例  
  
### <a name="a-simple-dwloader-example"></a>A. 简单的 dwloader 示例  
下面的示例显示 **加载程序** 的启动，仅选择所需的选项。 其他选项取自全局配置文件 *loadparamfile.txt*。  
  
使用 SQL Server 身份验证的示例。  
  
```  
--Load over Ethernet  
dwloader.exe -S 10.192.63.148 -U mylogin -P 123jkl -f /configfiles/loadparamfile.txt  
  
--Load over InfiniBand to appliance named MyPDW  
dwloader.exe -S MyPDW-SQLCTL01 -U mylogin -P 123jkl -f /configfiles/loadparamfile.txt  
```  
  
使用 Windows 身份验证的同一示例。  
  
```  
--Load over Ethernet  
dwloader.exe -S 10.192.63.148 -W -f /configfiles/loadparamfile.txt  
  
--Load over InfiniBand to appliance named MyPDW  
dwloader.exe -S MyPDW-SQLCTL01 -W -f /configfiles/loadparamfile.txt  
```  
  
使用源文件和错误文件的参数的示例。  
  
```  
--Load over Ethernet  
dwloader.exe -U mylogin -P 123jkl -S 10.192.63.148  -i C:\SQLData\AWDimEmployees.csv -T AdventureWorksPDW2012.dbo.DimEmployees -R C:\SQLData\LoadErrors  
```  
  
### <a name="b-load-data-into-an-adventureworks-table"></a>B. 将数据加载到 AdventureWorks 表中  
下面的示例是将数据加载到 **AdventureWorksPDW2012** 的批处理脚本的一部分。  若要查看完整脚本，请打开 **AdventureWorksPDW2012** 安装包附带的 aw_create.bat 文件。 

<!-- Missing link
For more information, see [Install AdventureWorksPDW2012](install-adventureworkspdw2012.md).  
-->

以下脚本代码片段使用 dwloader 将数据加载到 Dbo.dimaccount 表和 DimCurrency 表中。 此脚本使用的是以太网地址。 如果使用了 "无限"，则服务器将 *<appliance_name>* `-SQLCTL01` 。  
  
```  
set server=10.193.63.134  
set user=<MyUser>  
set password=<MyPassword>  
  
set schema=AdventureWorksPDW2012.dbo  
set load="C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\dwloader.exe"  
set mode=reload  
  
--Loads data into the AdventureWorksPDW2012.dbo.DimAccount table  
--Source data is stored in the file DimAccount.txt,   
--which is in the current directory.  
  
set t1=DimAccount  
%load% -S %server% -E -M %mode% -e Utf16 -i .\%t1%.txt -T %schema%.%t1% -R %t1%.bad -t "|" -r \r\n -U %user% -P %password%   
  
--Loads data from the DimCurrency.txt file into  
--AdventureWorksPDW2012.dbo.DimCurrency  
set t1=DimCurrency  
%load% -S %server% -E -M %mode% -e Utf16 -i .\%t1%.txt -T %schema%.%t1% -R %t1%.bad -t "|" -r \r\n -U %user% -P %password%  
```  
  
下面是 Dbo.dimaccount 表的 DDL。  
  
```  
CREATE TABLE DimAccount(  
AccountKey int NOT NULL,  
ParentAccountKey int,  
AccountCodeAlternateKey int,  
ParentAccountCodeAlternateKey int,  
AccountDescription nvarchar(50),  
AccountType nvarchar(50),  
Operator nvarchar(50),  
CustomMembers nvarchar(300),  
ValueType nvarchar(50),  
CustomMemberOptions nvarchar(200))  
with (CLUSTERED INDEX(AccountKey),  
DISTRIBUTION = REPLICATE);  
```  
  
下面是 DimAccount.txt 的数据文件示例，其中包含要加载到表 Dbo.dimaccount 中的数据。  
  
```  
--Sample of data in the DimAccount.txt load file.  
  
1||1||Balance Sheet||~||Currency|  
2|1|10|1|Assets|Assets|+||Currency|  
3|2|110|10|Current Assets|Assets|+||Currency|  
4|3|1110|110|Cash|Assets|+||Currency|  
5|3|1120|110|Receivables|Assets|+||Currency|  
6|5|1130|1120|Trade Receivables|Assets|+||Currency|  
7|5|1140|1120|Other Receivables|Assets|+||Currency|  
8|3|1150|110|Allowance for Bad Debt|Assets|+||Currency|  
9|3|1160|110|Inventory|Assets|+||Currency|  
10|9|1162|1160|Raw Materials|Assets|+||Currency|  
11|9|1164|1160|Work in Process|Assets|+||Currency|  
12|9|1166|1160|Finished Goods|Assets|+||Currency|  
13|3|1170|110|Deferred Taxes|Assets|+||Currency|  
```  
  
### <a name="c-load-data-from-the-command-line"></a>C. 从命令行加载数据  
可以通过在命令行上输入所有参数来替换示例 B 中的脚本，如以下示例中所示。  
  
```  
C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\dwloader.exe -S <Control node IP> -E -M reload -e UTF16 -i .\DimAccount.txt -T AdventureWorksPDW2012.dbo.DimAccount -R DimAccount.bad -t "|" -r \r\n -U <login> -P <password>  
```  
  
命令行参数说明：  
  
-   *C:\Program Files\Microsoft SQL Server 并行数据 Warehouse\100\dwloader.exe* 是 dwloader.exe 的安装位置。  
  
-   *-S* 后跟控制节点的 IP 地址。  
  
-   *-E* 指定将空字符串加载为 NULL。  
  
-   *-M reload.sql* 指定在插入源数据前截断目标表。  
  
-   *-e UTF16* 指示源文件使用 little endian 字符编码类型。  
  
-   *-i .\DimAccount.txt* 指定数据位于一个名为 DimAccount.txt 的文件中，该文件位于当前目录中。  
  
-   *-T AdventureWorksPDW2012* 指定要接收数据的表的由三部分组成的名称。  
  
-   *-R dbo.dimaccount* 指定无法加载的行将写入名为 dbo.dimaccount 的文件。  
  
-   *-t "|"* 指示输入文件中的字段（DimAccount.txt）是用竖线分隔的。  
  
-   *-r \r\n* 指定 DimAccount.txt 以回车符和换行符开头的每一行。  
  
-   *-U <login_name>-P <password>* 指定有权执行加载的登录名和密码。  
  

<!-- MISSING LINK

## See Also  
[Common Metadata Query Examples](metadata-query-examples.md)  

-->
