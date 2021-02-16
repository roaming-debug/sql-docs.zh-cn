---
description: 排序规则和 Unicode 支持
title: 排序规则和 Unicode 支持 | Microsoft Docs
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- binary collations [SQL Server]
- expression-level collations [SQL Server]
- Windows collations [SQL Server]
- globalization [SQL Server], about globalization
- supplementary character support
- code pages [SQL Server], about code pages
- collations [SQL Server], about collations
- Unicode [SQL Server], collations
- database-level collations [SQL Server]
- reading order
- column-level collations
- locales [SQL Server], about locales
- locales [SQL Server]
- code pages [SQL Server]
- SQL Server collations
- UTF-8
- UTF-16
- UTF8
- UTF16
- UCS2
- server-level collations [SQL Server]
ms.assetid: 92d34f48-fa2b-47c5-89d3-a4c39b0f39eb
author: pmasl
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 43b6fe61e9a7d1344f1f57ceb725a881d685daff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339575"
---
# <a name="collation-and-unicode-support"></a>排序规则和 Unicode 支持
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的排序规则可为您的数据提供排序规则、区分大小写属性和区分重音属性。 与诸如 char 和 varchar 等字符数据类型一起使用的排序规则规定可表示该数据类型的代码页和对应字符 。 

无论你是要安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的新实例、还原数据库备份，还是将服务器连接到客户端数据库，都必须了解正在处理的数据的区域设置要求、排序顺序以及是否区分大小写和重音。 若要列出在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例上可用的排序规则，请参阅 [sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)。    
    
为服务器、数据库、列或表达式选择排序规则时，同时也是在向数据分配某些特征。 这些特征会影响数据库中许多操作的结果。 例如，使用 `ORDER BY` 构造查询时，结果集的排序顺序可能取决于应用于该数据库的排序规则或 `COLLATE` 子句中在查询的表达式级别规定的排序规则。    
    
为了充分利用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的排序规则支持，应了解本主题中所定义的术语以及这些术语与数据的特征之间的关系。    
    
##  <a name="collation-terms"></a><a name="Terms"></a> 排序规则术语    
    
-   [排序规则](#Collation_Defn) 
    - [排序规则集](#Collation_sets)
    - [排序规则级别](#Collation_levels)
-   [区域设置](#Locale_Defn)    
-   [代码页](#Code_Page_Defn)    
-   [排序顺序](#Sort_Order_Defn)    
    
###  <a name="collation"></a><a name="Collation_Defn"></a> 排序规则    
排序规则指定表示数据集中每个字符的位模式。 排序规则还确定数据的排序和比较规则。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持在单个数据库中存储具有不同排序规则的对象。 对于非 Unicode 列，排序规则设置指定数据的代码页以及可以表示哪些字符。 必须将在非 Unicode 列间移动的数据从源代码页转换到目标代码页。    
    
当[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在具有不同排序规则设置的不同数据库上下文中运行时，其运行结果可能会不同。 如果可能，请为组织使用标准化排序规则。 这样就不必显式指定每个字符或 Unicode 表达式中的排序规则。 如果必须使用具有不同排序规则和代码页设置的对象，请对查询进行编码，以考虑排序规则的优先顺序规则。 有关详细信息，请参阅 [排序规则优先顺序 (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md)。    
    
与排序规则关联的选项区分大小写、区分重音、区分假名、区分全半角以及区分变体选择符。 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 为 [UTF-8](https://www.wikipedia.org/wiki/UTF-8) 编码引入了其他选项。 

可以通过将这些选项附加到排序规则名称中来指定这些选项。 例如，排序规则 Japanese_Bushu_Kakusu_100_CS_AS_KS_WS_UTF8 区分大小写、区分重音、区分假名、区分全半角以及使用 UTF-8 编码。 再举一例，此排序规则 Japanese_Bushu_Kakusu_140_CI_AI_KS_WS_VSS 就不区分大小写、不区分重音、区分假名、区分全半角、区分变体选择符，并且使用非 Unicode 编码。 

下表描述了与这些不同选项关联的行为：    
    
|选项|说明|    
|------------|-----------------|    
|区分大小写 (\_CS)|区分大写字母和小写字母。 如果选择此项，排序时小写字母将在其对应的大写字母之前。 如果未选择此选项，排序规则将不区分大小写。 即 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在排序时将大写字母和小写字母视为相同。 通过指定 \_CI，可以显式选择不区分大小写。|   
|区分重音 (\_AS)|区分重音字符和非重音字符。 例如，“a”和“ấ”视为不同字符。 如果未选择此选项，则排序规则将不区分重音。 即 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在排序时将字母的重音形式和非重音形式视为相同。 通过指定 \_AI，可以显式选择不区分重音。|    
|区分假名 (\_KS)|区分日语中的两种假名字符类型：平假名和片假名。 如果未选择此选项，则排序规则将不区分假名。 即 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在排序时将平假名字符和片假名字符视为相同。 省略此选项是指定不区分假名的唯一方法。|   
|区分全半角 (\_WS)|区分全角字符和半角字符。 如果未选择此选项，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会在排序时将同一字符的全角和半角形式视为相同。 省略此选项是指定不区分全半角的唯一方法。|  
|区分变体选择符 (\_VSS)|区分 [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 中首次引入的日语排序规则 Japanese_Bushu_Kakusu_140 和 Japanese_XJIS_140 中不同的象形变体选择符 。 变体序列包含基本字符加上其他变体选择符。 如果未选择 \_VSS 选项，排序规则不区分变体选择符，并且在比较中不考虑变体选择符。 也就是说，出于排序的考虑，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将具有不同变体选择符但基于相同基本字符的字符视为相同。 有关详细信息，请参阅 [Unicode Ideographic Variation Database](https://www.unicode.org/reports/tr37/)（Unicode 象形变体数据库）。<br/><br/> 全文搜索索引中不支持区分变体选择符 (\_VSS) 排序规则。 全文搜索索引仅支持区分重音 (\_AS)、区分假名 (\_KS) 和区分全半角 (\_WS) 选项。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] XML 和 CLR 引擎不支持 (\_VSS) 变体选择符。|      
|二进制 (\_BIN) <sup>1</sup>|根据为每个字符定义的位模式对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的数据进行排序和比较。 二进制排序顺序不仅区分大小写，而且也区分重音。 二进制排序顺序的速度也最快。 有关详细信息，请参阅本文中的[二进制排序规则](#Binary-collations)部分。|      
|二进制-码位 (\_BIN2) <sup>1</sup> | 根据 Unicode 数据的 Unicode 码位对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的数据进行排序和比较。 对于非 Unicode 数据，二进制码位将使用与二进制排序相同的比较方式。<br/><br/> 使用二进制-码位排序顺序的优点是：对已排序的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据进行比较的应用程序不必重新对数据进行排序。 因此，二进制-码位排序顺序使应用程序开发变得更加简单，从而可以提高性能。 有关详细信息，请参阅本文中的[二进制排序规则](#Binary-collations)部分。|
|UTF-8 (\_UTF8)|启用要在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中存储的 UTF-8 编码数据。 如果未选择此选项，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会对适用的数据类型使用默认的非 Unicode 编码格式。 有关详细信息，请参阅本文中的 [UTF-8 支持](#utf8)部分。| 

<sup>1</sup> 如果选择二进制或二进制-码位，则区分大小写 (\_CS)、区分重音 (\_AS)、区分假名 (\_KS) 和区分全半角 (\_WS) 选项均不可用。      

#### <a name="examples-of-collation-options"></a>排序规则选项示例
每个排序规则都由一系列定义大小写、重音、全半角或假名的后缀组合而成。 下列示例说明了各种后缀组合的排序顺序行为。

|Windows 排序规则后缀|排序顺序说明|
|------------|-----------------| 
|\_BIN<sup>1</sup>|二进制排序|
|\_BIN2<sup>1, 2</sup>|二进制码位排序顺序|
|\_CI_AI<sup>2</sup>|不区分大小写、不区分重音、不区分假名、不区分全半角|
|\_CI_AI_KS<sup>2</sup>|不区分大小写、不区分重音、区分假名、不区分全半角|
|\_CI_AI_KS_WS<sup>2</sup>|不区分大小写、不区分重音、区分假名、区分全半角|
|\_CI_AI_WS<sup>2</sup>|不区分大小写、不区分重音、不区分假名、区分全半角|
|\_CI_AS<sup>2</sup>|不区分大小写、区分重音、不区分假名、不区分全半角|
|\_CI_AS_KS<sup>2</sup>|不区分大小写、区分重音、区分假名、不区分全半角|
|\_CI_AS_KS_WS<sup>2</sup>|不区分大小写、区分重音、区分假名、区分全半角|
|\_CI_AS_WS<sup>2</sup>|不区分大小写、区分重音、不区分假名、区分全半角|
|\_CS_AI<sup>2</sup>|区分大小写、不区分重音、不区分假名、不区分全半角|
|\_CS_AI_KS<sup>2</sup>|区分大小写、不区分重音、区分假名、不区分全半角|
|\_CS_AI_KS_WS<sup>2</sup>|区分大小写、不区分重音、区分假名、区分全半角|
|\_CS_AI_WS<sup>2</sup>|区分大小写、不区分重音、不区分假名、区分全半角|
|\_CS_AS<sup>2</sup>|区分大小写、区分重音、不区分假名、不区分全半角|
|\_CS_AS_KS<sup>2</sup>|区分大小写、区分重音、区分假名、不区分全半角|
|\_CS_AS_KS_WS<sup>2</sup>|区分大小写、区分重音、区分假名、区分全半角|
|\_CS_AS_WS<sup>2</sup>|区分大小写、区分重音、不区分假名、区分全半角|

<sup>1</sup> 如果选择二进制或二进制-码位，则区分大小写 (\_CS)、区分重音 (\_AS)、区分假名 (\_KS) 和区分全半角 (\_WS) 选项均不可用。    

<sup>2</sup> 如果添加 UTF-8 选项 (\_UTF8)，可使用 UTF-8 对 Unicode 数据进行编码。 有关详细信息，请参阅本文中的 [UTF-8 支持](#utf8)部分。 

### <a name="collation-sets"></a><a name="Collation_sets"></a> 排序规则集

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持以下排序规则集：    

-  [Windows 排序规则](#Windows-collations)
-  [二进制排序规则](#Binary-collations)
-  [SQL Server 排序规则](#SQL-collations)
    
#### <a name="windows-collations"></a><a name="Windows-collations"></a> Windows 排序规则    
Windows 排序规则根据关联的 Windows 系统区域设置来定义字符数据的存储规则。 在 Windows 排序规则中，可以使用与 Unicode 数据相同的算法实现非 Unicode 数据的比较。 Windows 基本排序规则指定应用字典排序时所用的字母表或语言。 规则还指定用于存储非 Unicode 字符数据的代码页。 Unicode 排序和非 Unicode 排序都与特定 Windows 版本中的字符串比较相兼容。 这保证了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中所有数据类型的一致性，使开发人员能够使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所使用的相同规则对应用程序中的字符串排序。 有关详细信息，请参阅 [Windows 排序规则名称 (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md)。    
    
#### <a name="binary-collations"></a><a name="Binary-collations"></a> 二进制排序规则    
二进制排序规则基于区域设置和数据类型定义的编码值顺序来对数据进行排序。 它们区分大小写。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的二进制排序规则定义了所使用的区域设置和 ANSI 代码页。 这将强制使用二进制排序顺序。 由于它们相对简单，因此二进制排序规则有助于提高应用程序性能。 对于非 Unicode 数据类型，数据比较将基于 ANSI 代码页中定义的码位。 对于 Unicode 数据类型，数据比较将基于 Unicode 码位。 对于 Unicode 数据类型的二进制排序规则，数据排序将不考虑区域设置。 例如，对 Unicode 数据应用 Latin_1_General_BIN 和 Japanese_BIN，会得到完全相同的排序结果 。 有关详细信息，请参阅 [Windows 排序规则名称 (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md)。   
    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中有两种类型的二进制排序规则：

-  早期的 BIN 排序规则对 Unicode 数据执行的是不完整的逐码位比较。 这些早期的二进制排序规则将第一个字符作为 WCHAR 比较，接下来是逐字节比较。 在 BIN 排序规则中，仅首字符按照码位排序，其余字符根据其字节值排序。

-  更新的 BIN2 排序规则可实现纯码位比较。 在 BIN2 排序规则中，所有字符根据其码位排序。 由于 Intel 平台是一个 little endian 体系结构，因此 Unicode 码字符始终以字节对调的形式存储。     
    
#### <a name="sql-server-collations"></a><a name="SQL-collations"></a> SQL Server 排序规则    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则 (SQL_\*) 提供与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本兼容的排序顺序。 非 Unicode 数据的字典排序规则与 Windows 操作系统提供的任何排序例程都不兼容。 但是，Unicode 数据的排序与特定版本的 Windows 排序规则兼容。 由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则对非 Unicode 数据和 Unicode 数据使用不同的比较规则，因此对于相同数据的比较会看到不同的结果，具体取决于基本数据类型。 有关详细信息，请参阅 [SQL Server 排序规则名称 (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)。 

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中，默认安装排序规则设置由操作系统 (OS) 区域设置确定。 服务器级排序规则可以在安装期间进行更改，也可以在安装前通过更改 OS 区域设置进行更改。 出于后向兼容性原因，默认排序规则设置为与每个特定区域设置关联的最早可用版本。 因此，不推荐总是使用默认排序规则。 更改 Windows 排序规则的默认安装设置可充分利用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。 例如，对于 OS 区域设置“英语(美国)”（代码页 1252），安装过程中的默认排序规则是 SQL_Latin1_General_CP1_CI_AS，可将其更改为最接近的 Windows 对等排序规则 Latin1_General_100_CI_AS_SC 。
    
> [!NOTE]    
> 升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的英文实例时可以指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则 (SQL_\*)，以便与现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例兼容。 由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的默认排序规则是在安装过程中定义的，因此在以下情况下请确保慎重指定排序规则设置：    
>     
> -   应用程序代码依赖早期 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则的行为。    
> -   必须存储反映多种语言的字符数据。    
    
### <a name="collation-levels"></a><a name="Collation_levels"></a> 排序规则级别
支持在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的下列级别设置排序规则：    

-  [服务器级排序规则](#Server-level-collations)
-  [数据库级排序规则](#Database-level-collations)
-  [列级排序规则](#Column-level-collations)
-  [表达式级排序规则](#Expression-level-collations)

#### <a name="server-level-collations"></a><a name="Server-level-collations"></a> 服务器级排序规则   
默认服务器排序规则是在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中设置的，它将成为系统数据库和所有用户数据库的默认排序规则。 

下表显示由操作系统 (OS) 区域设置确定的默认排序规则标识，其中包括它们的 Windows 和 SQL 语言代码标识符 (LCID)：

|Windows 区域设置|Windows LCID|SQL LCID|默认排序规则|
|---------------|---------|---------|---------------|
|南非荷兰语（南非）|0x0436|0x0409|Latin1_General_CI_AS|
|阿尔巴尼亚语（阿尔巴尼亚）|0x041c|0x041c|Albanian_CI_AS|
|阿尔萨斯语（法国）|0x0484|0x0409|Latin1_General_CI_AS|
|阿姆哈拉语（埃塞俄比亚）|0x045e|0x0409|Latin1_General_CI_AS|
|阿拉伯语（阿尔及利亚）|0x1401|0x0401|Arabic_CI_AS|
|阿拉伯语（巴林）|0x3c01|0x0401|Arabic_CI_AS|
|阿拉伯语（埃及）|0x0c01|0x0401|Arabic_CI_AS|
|阿拉伯语（伊拉克）|0x0801|0x0401|Arabic_CI_AS|
|阿拉伯语（约旦）|0x2c01|0x0401|Arabic_CI_AS|
|阿拉伯语（科威特）|0x3401|0x0401|Arabic_CI_AS|
|阿拉伯语（黎巴嫩）|0x3001|0x0401|Arabic_CI_AS|
|阿拉伯语（利比亚）|0x1001|0x0401|Arabic_CI_AS|
|阿拉伯语（摩洛哥）|0x1801|0x0401|Arabic_CI_AS|
|阿拉伯语（阿曼）|0x2001|0x0401|Arabic_CI_AS|
|阿拉伯语（卡塔尔）|0x4001|0x0401|Arabic_CI_AS|
|阿拉伯语（沙特阿拉伯）|0x0401|0x0401|Arabic_CI_AS|
|阿拉伯语（叙利亚）|0x2801|0x0401|Arabic_CI_AS|
|阿拉伯语（突尼斯）|0x1c01|0x0401|Arabic_CI_AS|
|阿拉伯语（阿拉伯联合酋长国）|0x3801|0x0401|Arabic_CI_AS|
|阿拉伯语（也门）|0x2401|0x0401|Arabic_CI_AS|
|亚美尼亚语（亚美尼亚）|0x042b|0x0419|Latin1_General_CI_AS|
|阿萨姆语（印度）|0x044d|0x044d|在服务器级别不可用|
|阿塞拜疆语（阿塞拜疆，西里尔文）|0x082c|0x082c|不推荐使用，在服务器级别不可用|
|阿塞拜疆语（阿塞拜疆，拉丁语）|0x042c|0x042c|不推荐使用，在服务器级别不可用|
|巴什基尔语（俄罗斯）|0x046d|0x046d|Latin1_General_CI_AI|
|巴斯克语（巴斯克）|0x042d|0x0409|Latin1_General_CI_AS|
|白俄罗斯语（白俄罗斯）|0x0423|0x0419|Cyrillic_General_CI_AS|
|孟加拉语（孟加拉国）|0x0845|0x0445|在服务器级别不可用|
|孟加拉语（印度）|0x0445|0x0439|在服务器级别不可用|
|波斯尼亚语（波斯尼亚和黑塞哥维那，西里尔文）|0x201a|0x201a|Latin1_General_CI_AI|
|波斯尼亚语（波斯尼亚和黑塞哥维那，拉丁语）|0x141a|0x141a|Latin1_General_CI_AI|
|布列塔尼语（法国）|0x047e|0x047e|Latin1_General_CI_AI|
|保加利亚语（保加利亚）|0x0402|0x0419|Cyrillic_General_CI_AS|
|加泰罗尼亚语（加泰罗尼亚）|0x0403|0x0409|Latin1_General_CI_AS|
|中文（香港特别行政区）|0x0c04|0x0404|Chinese_Taiwan_Stroke_CI_AS|
|中文（澳门特别行政区）|0x1404|0x1404|Latin1_General_CI_AI|
|中文（澳门特别行政区）|0x21404|0x21404|Latin1_General_CI_AI|
|中文（中华人民共和国）|0x0804|0x0804|Chinese_PRC_CI_AS|
|中文（中华人民共和国）|0x20804|0x20804|Chinese_PRC_Stroke_CI_AS|
|中文（新加坡）|0x1004|0x0804|Chinese_PRC_CI_AS|
|中文（新加坡）|0x21004|0x20804|Chinese_PRC_Stroke_CI_AS|
|中文（台湾）|0x30404|0x30404|Chinese_Taiwan_Bopomofo_CI_AS|
|中文（台湾）|0x0404|0x0404|Chinese_Taiwan_Stroke_CI_AS|
|科西嘉语（法国）|0x0483|0x0483|Latin1_General_CI_AI|
|克罗地亚语（波斯尼亚和黑塞哥维那，拉丁语）|0x101a|0x041a|Croatian_CI_AS|
|克罗地亚语（克罗地亚）|0x041a|0x041a|Croatian_CI_AS|
|捷克语（捷克共和国）|0x0405|0x0405|Czech_CI_AS|
|丹麦语（丹麦）|0x0406|0x0406|Danish_Norwegian_CI_AS|
|达里语（阿富汗）|0x048c|0x048c|Latin1_General_CI_AI|
|迪维希语（马尔代夫）|0x0465|0x0465|在服务器级别不可用|
|荷兰语（比利时）|0x0813|0x0409|Latin1_General_CI_AS|
|荷兰语（荷兰）|0x0413|0x0409|Latin1_General_CI_AS|
|英语（澳大利亚）|0x0c09|0x0409|Latin1_General_CI_AS|
|英语（伯利兹）|0x2809|0x0409|Latin1_General_CI_AS|
|英语（加拿大）|0x1009|0x0409|Latin1_General_CI_AS|
|英语（加勒比海）|0x2409|0x0409|Latin1_General_CI_AS|
|英语（印度）|0x4009|0x0409|Latin1_General_CI_AS|
|英语（爱尔兰）|0x1809|0x0409|Latin1_General_CI_AS|
|英语（牙买加）|0x2009|0x0409|Latin1_General_CI_AS|
|英语（马来西亚）|0x4409|0x0409|Latin1_General_CI_AS|
|英语（新西兰）|0x1409|0x0409|Latin1_General_CI_AS|
|英语（菲律宾）|0x3409|0x0409|Latin1_General_CI_AS|
|英语（新加坡）|0x4809|0x0409|Latin1_General_CI_AS|
|英语（南非）|0x1c09|0x0409|Latin1_General_CI_AS|
|英语（特立尼达和多巴哥）|0x2c09|0x0409|Latin1_General_CI_AS|
|英语（英国）|0x0809|0x0409|Latin1_General_CI_AS|
|英语（美国）|0x0409|0x0409|SQL_Latin1_General_CP1_CI_AS|
|英语（津巴布韦）|0x3009|0x0409|Latin1_General_CI_AS|
|爱沙尼亚语（爱沙尼亚）|0x0425|0x0425|Estonian_CI_AS|
|法罗语（法罗群岛）|0x0438|0x0409|Latin1_General_CI_AS|
|菲律宾语（菲律宾）|0x0464|0x0409|Latin1_General_CI_AS|
|芬兰语（芬兰）|0x040b|0x040b|Finnish_Swedish_CI_AS|
|法语（比利时）|0x080c|0x040c|French_CI_AS|
|法语（加拿大）|0x0c0c|0x040c|French_CI_AS|
|法语（法国）|0x040c|0x040c|French_CI_AS|
|法语（卢森堡）|0x140c|0x040c|French_CI_AS|
|法语（摩纳哥）|0x180c|0x040c|French_CI_AS|
|法语（瑞士）|0x100c|0x040c|French_CI_AS|
|弗里西亚语（荷兰）|0x0462|0x0462|Latin1_General_CI_AI|
|加利西亚语|0x0456|0x0409|Latin1_General_CI_AS|
|格鲁吉亚语（格鲁吉亚）|0x10437|0x10437|Georgian_Modern_Sort_CI_AS|
|格鲁吉亚语（格鲁吉亚）|0x0437|0x0419|Latin1_General_CI_AS|
|德语 - 电话簿排序 (DIN)|0x10407|0x10407|German_PhoneBook_CI_AS|
|德语（奥地利）|0x0c07|0x0409|Latin1_General_CI_AS|
|德语（德国）|0x0407|0x0409|Latin1_General_CI_AS|
|德语（列支敦士登）|0x1407|0x0409|Latin1_General_CI_AS|
|德语（卢森堡）|0x1007|0x0409|Latin1_General_CI_AS|
|德语（瑞士）|0x0807|0x0409|Latin1_General_CI_AS|
|希腊语（希腊）|0x0408|0x0408|Greek_CI_AS|
|格陵兰语（格陵兰）|0x046f|0x0406|Danish_Norwegian_CI_AS|
|古吉拉特语（印度）|0x0447|0x0439|在服务器级别不可用|
|豪撒语（尼日利亚，拉丁语）|0x0468|0x0409|Latin1_General_CI_AS|
|希伯来语（以色列）|0x040d|0x040d|Hebrew_CI_AS|
|印地语（印度）|0x0439|0x0439|在服务器级别不可用|
|匈牙利语（匈牙利）|0x040e|0x040e|Hungarian_CI_AS|
|匈牙利语技术排序|0x1040e|0x1040e|Hungarian_Technical_CI_AS|
|冰岛语（冰岛）|0x040f|0x040f|Icelandic_CI_AS|
|伊博语（尼日利亚）|0x0470|0x0409|Latin1_General_CI_AS|
|印度尼西亚语（印度尼西亚）|0x0421|0x0409|Latin1_General_CI_AS|
|因纽特语（加拿大，拉丁语）|0x085d|0x0409|Latin1_General_CI_AS|
|因纽特语（语音节）加拿大|0x045d|0x045d|Latin1_General_CI_AI|
|爱尔兰语（爱尔兰）|0x083c|0x0409|Latin1_General_CI_AS|
|意大利语（意大利）|0x0410|0x0409|Latin1_General_CI_AS|
|意大利语（瑞士）|0x0810|0x0409|Latin1_General_CI_AS|
|日语（日本 XJIS）|0x0411|0x0411|Japanese_CI_AS|
|日语（日本）|0x040411|0x40411|Latin1_General_CI_AI|
|卡纳达语（印度）|0x044b|0x0439|在服务器级别不可用|
|哈萨克语（哈萨克斯坦）|0x043f|0x043f|Kazakh_90_CI_AS|
|高棉语（柬埔寨）|0x0453|0x0453|在服务器级别不可用|
|基切语（危地马拉）|0x0486|0x0c0a|Modern_Spanish_CI_AS|
|卢旺达语（卢旺达）|0x0487|0x0409|Latin1_General_CI_AS|
|孔卡尼语（印度）|0x0457|0x0439|在服务器级别不可用|
|韩语（韩语字典排序）|0x0412|0x0412|Korean_Wansung_CI_AS|
|柯尔克孜语（吉尔吉斯斯坦）|0x0440|0x0419|Cyrillic_General_CI_AS|
|老挝语（老挝人民民主共和国）|0x0454|0x0454|在服务器级别不可用|
|拉脱维亚语（拉脱维亚）|0x0426|0x0426|Latvian_CI_AS|
|立陶宛语（立陶宛）|0x0427|0x0427|Lithuanian_CI_AS|
|下索布语（德国）|0x082e|0x0409|Latin1_General_CI_AS|
|卢森堡语（卢森堡）|0x046e|0x0409|Latin1_General_CI_AS|
|马其顿语（北马其顿，FYROM）|0x042f|0x042f|Macedonian_FYROM_90_CI_AS|
|马来语（文莱达鲁萨兰国）|0x083e|0x0409|Latin1_General_CI_AS|
|马来语（马来西亚）|0x043e|0x0409|Latin1_General_CI_AS|
|马拉雅拉姆语（印度）|0x044c|0x0439|在服务器级别不可用|
|马耳他语（马耳他）|0x043a|0x043a|Latin1_General_CI_AI|
|毛利语（新西兰）|0x0481|0x0481|Latin1_General_CI_AI|
|马普丹冈语（智利）|0x047a|0x047a|Latin1_General_CI_AI|
|马拉地语（印度）|0x044e|0x0439|在服务器级别不可用|
|莫霍克语（加拿大）|0x047c|0x047c|Latin1_General_CI_AI|
|蒙古语（蒙古）|0x0450|0x0419|Cyrillic_General_CI_AS|
|蒙古语（中国）|0x0850|0x0419|Cyrillic_General_CI_AS|
|尼泊尔语（尼泊尔）|0x0461|0x0461|在服务器级别不可用|
|挪威语（博克马尔语，挪威）|0x0414|0x0414|Latin1_General_CI_AI|
|挪威语（尼诺斯克语，挪威）|0x0814|0x0414|Latin1_General_CI_AI|
|奥克西唐语（法国）|0x0482|0x040c|French_CI_AS|
|奥里亚语（印度）|0x0448|0x0439|在服务器级别不可用|
|普什图语（阿富汗）|0x0463|0x0463|在服务器级别不可用|
|波斯语（伊朗）|0x0429|0x0429|Latin1_General_CI_AI|
|波兰语（波兰）|0x0415|0x0415|Polish_CI_AS|
|葡萄牙语（巴西）|0x0416|0x0409|Latin1_General_CI_AS|
|葡萄牙语(葡萄牙)|0x0816|0x0409|Latin1_General_CI_AS|
|旁遮普语（印度）|0x0446|0x0439|在服务器级别不可用|
|克丘亚语（玻利维亚）|0x046b|0x0409|Latin1_General_CI_AS|
|克丘亚语（厄瓜多尔）|0x086b|0x0409|Latin1_General_CI_AS|
|克丘亚语（秘鲁）|0x0c6b|0x0409|Latin1_General_CI_AS|
|罗马尼亚语（罗马尼亚）|0x0418|0x0418|Romanian_CI_AS|
|罗曼什语（瑞士）|0x0417|0x0417|Latin1_General_CI_AI|
|俄语（俄罗斯）|0x0419|0x0419|Cyrillic_General_CI_AS|
|萨哈语（俄罗斯）|0x0485|0x0485|Latin1_General_CI_AI|
|萨米语（伊纳里，芬兰）|0x243b|0x083b|Latin1_General_CI_AI|
|萨米语(律勒欧，挪威)|0x103b|0x043b|Latin1_General_CI_AI|
|萨米语（律勒欧，瑞典）|0x143b|0x083b|Latin1_General_CI_AI|
|萨米语（北方，芬兰）|0x0c3b|0x083b|Latin1_General_CI_AI|
|萨米语(北方，挪威)|0x043b|0x043b|Latin1_General_CI_AI|
|萨米语（北方，瑞典）|0x083b|0x083b|Latin1_General_CI_AI|
|萨米语（斯科特，芬兰）|0x203b|0x083b|Latin1_General_CI_AI|
|萨米语（南方，挪威）|0x183b|0x043b|Latin1_General_CI_AI|
|萨米语（南方，瑞典）|0x1c3b|0x083b|Latin1_General_CI_AI|
|梵语（印度）|0x044f|0x0439|在服务器级别不可用|
|塞尔维亚语（波斯尼亚和黑塞哥维那，西里尔文）|0x1c1a|0x0c1a|Latin1_General_CI_AI|
|塞尔维亚语（波斯尼亚和黑塞哥维那，拉丁语）|0x181a|0x081a|Latin1_General_CI_AI|
|塞尔维亚语（塞尔维亚，西里尔语）|0x0c1a|0x0c1a|Latin1_General_CI_AI|
|塞尔维亚语（塞尔维亚，拉丁语）|0x081a|0x081a|Latin1_General_CI_AI|
|巴索托语/北索托语（南非）|0x046c|0x0409|Latin1_General_CI_AS|
|茨瓦纳语/博茨瓦纳（南非）|0x0432|0x0409|Latin1_General_CI_AS|
|僧伽罗语（斯里兰卡）|0x045b|0x0439|在服务器级别不可用|
|斯洛伐克语（斯洛伐克）|0x041b|0x041b|Slovak_CI_AS|
|斯洛文尼亚语（斯洛文尼亚）|0x0424|0x0424|Slovenian_CI_AS|
|西班牙语（阿根廷）|0x2c0a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（玻利维亚）|0x400a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（智利）|0x340a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（哥伦比亚）|0x240a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（哥斯达黎加）|0x140a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（多米尼加共和国）|0x1c0a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（厄瓜多尔）|0x300a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（萨尔瓦多）|0x440a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（危地马拉）|0x100a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（洪都拉斯）|0x480a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（墨西哥）|0x080a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙（尼加拉瓜）|0x4c0a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（巴拿马）|0x180a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（巴拉圭）|0x3c0a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（秘鲁）|0x280a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（波多黎各）|0x500a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语(西班牙)|0x0c0a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（西班牙，传统风格）|0x040a|0x040a|Traditional_Spanish_CI_AS|
|西班牙语（美国）|0x540a|0x0409|Latin1_General_CI_AS|
|西班牙语（乌拉圭）|0x380a|0x0c0a|Modern_Spanish_CI_AS|
|西班牙语（委内瑞拉）|0x200a|0x0c0a|Modern_Spanish_CI_AS|
|斯瓦希里语（肯尼亚）|0x0441|0x0409|Latin1_General_CI_AS|
|瑞典语（芬兰）|0x081d|0x040b|Finnish_Swedish_CI_AS|
|瑞典语（瑞典）|0x041d|0x040b|Finnish_Swedish_CI_AS|
|叙利亚语（叙利亚）|0x045a|0x045a|在服务器级别不可用|
|塔吉克语（塔吉克斯坦）|0x0428|0x0419|Cyrillic_General_CI_AS|
|塔马塞特文（阿尔及利亚，拉丁语）|0x085f|0x085f|Latin1_General_CI_AI|
|泰米尔语（印度）|0x0449|0x0439|在服务器级别不可用|
|鞑靼语（俄罗斯）|0x0444|0x0444|Cyrillic_General_CI_AS|
|泰卢固语（印度）|0x044a|0x0439|在服务器级别不可用|
|泰语（泰国）|0x041e|0x041e|Thai_CI_AS|
|藏语（中国）|0x0451|0x0451|在服务器级别不可用|
|土耳其语（土耳其）|0x041f|0x041f|Turkish_CI_AS|
|土库曼语（土库曼斯坦）|0x0442|0x0442|Latin1_General_CI_AI|
|维吾尔语（中国）|0x0480|0x0480|Latin1_General_CI_AI|
|乌克兰语（乌克兰）|0x0422|0x0422|Ukrainian_CI_AS|
|上索布语（德国）|0x042e|0x042e|Latin1_General_CI_AI|
|乌尔都语（巴基斯坦）|0x0420|0x0420|Latin1_General_CI_AI|
|乌兹别克语（乌兹别克斯坦，西里尔文）|0x0843|0x0419|Cyrillic_General_CI_AS|
|乌兹别克语（乌兹别克斯坦，拉丁语）|0x0443|0x0443|Uzbek_Latin_90_CI_AS|
|越南语（越南）|0x042a|0x042a|Vietnamese_CI_AS|
|威尔士语（英国）|0x0452|0x0452|Latin1_General_CI_AI|
|沃洛夫语（塞内加尔）|0x0488|0x040c|French_CI_AS|
|班图语/索萨语（南非）|0x0434|0x0409|Latin1_General_CI_AS|
|彝语（中国）|0x0478|0x0409|Latin1_General_CI_AS|
|约鲁巴语（尼日利亚）|0x046a|0x0409|Latin1_General_CI_AS|
|祖鲁语（南非）|0x0435|0x0409|Latin1_General_CI_AS|

为服务器分配排序规则后，只能通过导出所有数据库对象和数据来更改它，重新生成 master 数据库，并导入所有数据库对象和数据。 与更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的默认排序规则相反，可在创建新数据库或数据库列时指定所需的排序规则。    

若要查询 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的服务器排序规则，请使用下列 `SERVERPROPERTY` 函数：

```sql
SELECT CONVERT(varchar, SERVERPROPERTY('collation'));
```

若要查询服务器以找到所有可用的排序规则，请使用以下 `fn_helpcollations()` 内置函数：

```sql
SELECT * FROM sys.fn_helpcollations();
```
    
#### <a name="database-level-collations"></a><a name="Database-level-collations"></a> 数据库级排序规则    
创建或修改数据库时，可使用 `CREATE DATABASE` 或 `ALTER DATABASE` 语句的 `COLLATE` 子句指定默认数据库排序规则。 如果未指定排序规则，将为该数据库分配服务器排序规则。    
    
除非更改服务器的排序规则，否则无法更改系统数据库的排序规则。
    
数据库排序规则将应用于数据库中的所有元数据，并且是所有字符串列、临时对象、变量名称和数据库中使用的任何其他字符串的默认排序规则。 当更改用户数据库的排序规则时，如果在数据库访问临时表中进行查询，则可能出现排序规则冲突。 临时表始终存储在 tempdb 系统数据库中，该数据库使用实例的排序规则。 如果排序规则导致计算字符数据时出现冲突，则比较用户数据库和 tempdb 之间的字符数据的查询可能会失败。 可以通过在查询中指定 `COLLATE` 子句来解决此问题。 有关详细信息，请参阅[排序规则 (Transact-SQL)](~/t-sql/statements/collations.md)。    

> [!NOTE]
> 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 上创建数据库后，将无法更改排序规则。

可使用如下的 `ALTER DATABASE` 语句更改用户数据库的排序规则：

```sql
ALTER DATABASE myDB COLLATE Greek_CS_AI;
```

> [!IMPORTANT]
> 更改数据库级排序规则不会影响列级排序规则或表达式级排序规则。

可使用如下的语句来检索数据库的当前排序规则：

```sql
SELECT CONVERT (VARCHAR(50), DATABASEPROPERTYEX('database_name','collation'));
```

#### <a name="column-level-collations"></a><a name="Column-level-collations"></a> 列级排序规则    
当创建或更改表时，可使用 `COLLATE` 子句指定每个字符串列的排序规则。 如果不指定排序规则，将为列分配数据库的默认排序规则。    

可使用如下的 `ALTER TABLE` 语句更改列的排序规则：

```sql
ALTER TABLE myTable ALTER COLUMN mycol NVARCHAR(10) COLLATE Greek_CS_AI;
```
    
#### <a name="expression-level-collations"></a><a name="Expression-level-collations"></a> 表达式级排序规则    
表达式级排序规则在语句运行时设置，并且影响结果集的返回方式。 这可以使 `ORDER BY` 排序结果特定于区域设置。 要实现表达式级别的排序规则，请使用如下的 `COLLATE` 子句：    
    
```sql    
SELECT name FROM customer ORDER BY name COLLATE Latin1_General_CS_AI;    
```    
    
###  <a name="locale"></a><a name="Locale_Defn"></a> 区域设置    
区域设置是与位置或区域性相关联的一组信息。 此信息可以包括所用语言的名称和标识符、用于书写该语言的文字以及文化习俗。 排序规则可以与一个或多个区域设置相关联。 有关详细信息，请参阅 [Microsoft 分配的区域设置 ID](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c)。    
    
###  <a name="code-page"></a><a name="Code_Page_Defn"></a> 代码页    
代码页是给定脚本的有序字符集，其中数值索引（即码位值）与每个字符相关联。 Windows 代码页通常被称为“字符集” 。 代码页用于支持不同的 Windows 系统区域设置所使用的字符集和键盘布局。     
 
###  <a name="sort-order"></a><a name="Sort_Order_Defn"></a> 排序顺序    
排序顺序指定数据值的排序方式。 该顺序影响数据比较的结果。 数据的排序通过使用排序规则而实现，且可使用索引对排序进行优化。    
    
##  <a name="unicode-support"></a><a name="Unicode_Defn"></a> Unicode 支持    
Unicode 是一种将码位映射到字符的标准。 由于它旨在涵盖全球所有语言的所有字符，因此，无需使用不同代码页来处理不同字符集。

### <a name="unicode-basics"></a>Unicode 基础知识
当只使用字符数据和代码页时，在一个数据库内很难以多种语言存储数据。 也很难为数据库找到一种能存储所有需要的特定语言字符的代码页。 此外，当运行不同代码页的不同客户端读取和更新特殊字符时，很难保证正确转换这些字符。 支持国际化客户端的数据库应始终使用 Unicode 数据类型，而不应使用非 Unicode 数据类型。

例如，有一个必须处理三种主要语言的北美洲客户的数据库：

-  墨西哥使用的西班牙语名称和地址
-  魁北克使用的法语名称和地址
-  加拿大其余地区和美国使用的英语名称和地址

当只使用字符列和代码页时，必须小心处理，确保与数据库一起安装的代码页能处理所有这三种语言的字符。 当正在运行的代码页的客户端读取另一种语言的字符时，还必须注意确保从任何一种语言正确转换字符。
   
> [!NOTE]
> 客户端使用的代码页由操作系统 (OS) 设置确定。 若要在 Windows 操作系统上设置客户端代码页，请使用“控制面板”中的 **“区域设置”** 。    

对于支持世界范围的读者所需的所有字符的字符数据类型，很难为其选择代码页。 在国际化数据库中，最简单的字符数据管理方法是始终使用支持 Unicode 的数据类型。 

### <a name="unicode-data-types"></a>Unicode 数据类型
如果在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本）中存储反映多种语言的字符数据，请使用 Unicode 数据类型（nchar、nvarchar 和 ntext），而不是非 Unicode 数据类型（char、varchar 和 text     ）。 

> [!NOTE]
> 对于 Unicode 数据类型，[!INCLUDE[ssde_md](../../includes/ssde_md.md)]最多可以使用 UCS-2 表示 65,535 个字符；或者，如果使用了附属字符，可表示整个 Unicode 范围（‭1,114,111 个字符）。 如需详细了解如何启用增补字符，请参阅[字符](#Supplementary_Characters)。

或者，从 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 开始，如果使用支持 UTF-8 的排序规则 (\_UTF8)，则以前的非 Unicode 数据类型（char 和 varchar）将变为使用 UTF-8 编码的 Unicode 数据类型 。 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 不会更改以前存在的 Unicode 数据类型（nchar、nvarchar 和 ntext）的行为，且继续使用 UCS-2 或 UTF-16 编码  。 有关详细信息，请参阅 [UTF-8 与 UTF-16 的存储差异](#storage_differences)。

### <a name="unicode-considerations"></a>Unicode 注意事项
非 Unicode 数据类型有明显的局限性， 这是因为非 Unicode 计算机只能使用单个代码页。 使用 Unicode，你可能会体验到性能提升，因为这只需要较少的代码页转换。 必须在数据库级、列级或表达式级单独选择 Unicode 排序规则，因为在服务器级不支持 Unicode 排序规则。    

在将数据从服务器移到客户端时，服务器排序规则可能无法由旧版本的客户端驱动程序识别。 将数据从 Unicode 服务器移到非 Unicode 客户端时，可能会出现此情况。 最好升级客户端操作系统，以使基础系统排序规则得以升级。 如果客户端上安装了数据库客户端软件，则可以考虑对数据库客户端软件应用服务更新。    
    
> [!TIP]
> 还可以尝试针对服务器上的数据使用另一个排序规则。 选择一个映射到客户端上的代码页的排序规则。    
>
> 若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本）中提供的 UTF-16 排序规则来改进对一些 Unicode 字符的搜索和排序（仅 Windows 排序规则），可以选择增补字符 (\_SC) 排序规则之一，或版本 140 排序规则之一。    
 
若要使用 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 中提供的 UTF-8 排序规则来改进对某些 Unicode 字符的搜索和排序（仅 Windows 排序规则），必须选择已启用 UTF-8 编码的排序规则 (\_UTF8)。
 
-   UTF8 标志可应用于：    
    -   已支持补充字符 (\_SC) 或区分变体选择符 (\_VSS) 感知的语言排序规则
    -   BIN2<sup>1</sup> 二进制排序规则
    
-   UTF8 标志可应用于：    
    -   不支持补充字符 (\_SC) 或区分变体选择符 (\_VSS) 感知的语言排序规则
    -   BIN 或 BIN2<sup>2</sup> 二进制排序规则
    -   SQL\_* 排序规则  
    
<sup>1</sup>自 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 2.3 起。 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 3.0 已将排序规则 UTF8_BIN2 替换为 Latin1_General_100_BIN2_UTF8 。        
<sup>2</sup>截至 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 2.3。    
    
若要评估与使用 Unicode 或非 Unicode 数据类型相关的问题，请测试您的具体方案以确定您所在环境下的性能差异大小。 最好对整个组织中的系统所使用的排序规则进行标准化，并尽可能部署 Unicode 服务器和客户端。    
    
在许多情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 与其他服务器或客户端交互，你的组织可能会使用应用程序和服务器实例之间的多种数据访问标准。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客户端是两种主要类型之一：    
    
-   使用 OLE DB 和开放式数据库连接 (ODBC) 3.7 版或更高版本的 Unicode 客户端。    
-   使用 DB-Library 和 ODBC 3.6 版或更低版本的非 Unicode 客户端。    
    
下表提供有关以 Unicode 和非 Unicode 服务器的各种组合使用多语言数据的信息：    
    
|服务器|Client|优点或局限性|    
|------------|------------|-----------------------------|    
|Unicode|Unicode|因为 Unicode 数据在整个系统中使用，所以此方案可提供最佳的性能并可保护检索到的数据免受破坏。 ActiveX 数据对象 (ADO)、OLE DB 和 ODBC 3.7 版或更高版本都采用这样的配置。|    
|Unicode|非 Unicode|在这种情况下，尤其对于正在运行新操作系统的服务器与正在运行旧版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]或基于旧操作系统的客户端之间的连接，当向客户端计算机移动数据时，会受到限制或出现错误。 服务器上的 Unicode 数据尝试映射到非 Unicode 客户端的对应代码页，以转换数据。|    
|非 Unicode|Unicode|该方案对使用多语言数据不是理想配置。 在此方案下不能向非 Unicode 服务器写入 Unicode 数据。 在向服务器的代码页之外的服务器发送数据时，很可能会发生问题。|    
|非 Unicode|非 Unicode|这是对多语言数据有极大局限性的方案。 您只可使用一个代码页。|    
    
##  <a name="supplementary-characters"></a><a name="Supplementary_Characters"></a> 增补字符    
Unicode 联盟为每个字符都分配一个唯一码位（介于 000000-10FFFF 之间的值）。 最常用字符的码位值介于范围 000000-00FFFF（65,535 个字符）之间，可以装入内存中和磁盘上的 8 位字或 16 位字中。 通常将此范围指定为基本多文种平面 (BMP)。 

但 Unicode 联盟额外建立了 16 个字符“平面”，每个平面的大小都与 BMP 相同。 此定义允许 Unicode 表示介于码位范围 000000-10FFFF 之间的 1,114,112 个字符（即 2<sup>16</sup>* 17 个字符）。 码位值大于 00FFFF 的字符需要 2 到 4 个连续 8 位字 (UTF-8)，或 2 个连续 16 位字 (UTF-16)。 超出 BMP 的字符称为“附属字符”，其他连续 8 位字或 16 位字称为“代理项对”。 如需了解增补字符、代理项、代理项对的更多详细信息，请参阅 [Unicode 标准](http://www.unicode.org/standard/standard.html)。    

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供用于存储介于 BMP 范围（000000-00FFFF）内的 Unicode 数据的数据类型（如 nchar 和 nvarchar），而[!INCLUDE[ssde_md](../../includes/ssde_md.md)]使用 UCS-2 编码它们。 

[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 引入了新增补字符 (\_SC) 排序规则系列，可以与下面的数据类型结合使用来表示整个 Unicode 字符范围（000000-10FFFF）：nchar、nvarchar 和 sql_variant。 例如：Latin1_General_100_CI_AS_SC 或 Japanese_Bushu_Kakusu_100_CI_AS_SC（如果使用日语排序规则） 。 
 
[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 将增补字符支持扩展到，与已启用 UTF-8 的新排序规则 ([\_UTF8](#utf8)) 结合使用的数据类型 char 和 varchar。 这些数据类型也能表示整个 Unicode 字符范围。   

> [!NOTE]
> 自 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 起，所有新 \_140 排序规则都自动支持增补字符。

如果使用增补字符：    
    
-   只有在 90 版本或更高版本的排序规则中才可以将增补字符用于排序和比较操作。    
-   所有 100 版本的排序规则均支持使用补充字符进行语言排序。    
-   不支持在元数据（如数据库对象的名称）中使用增补字符。    
-   SC 标志可应用于：    
    -   90 版本的排序规则    
    -   100 版本的排序规则    
    
-   SC 标志不能应用于：    
    -   80 版本非版本化 Windows 排序规则    
    -   BIN 或 BIN2 二进制排序规则    
    -   SQL\* 排序规则    
    -   140 版本的排序规则（这些排序规则无需 SC 标志，因为它们已支持增补字符）    
    
下表比较了某些字符串函数和字符串运算符在使用具有和没有补充字符识别 (SCA) 排序规则的补充字符时的行为：    
    
|字符串函数或运算符|具有 SCA 排序规则|没有 SCA 排序规则|    
|---------------------------------|--------------------------|-----------------------------|    
|[CHARINDEX](../../t-sql/functions/charindex-transact-sql.md)<br /><br /> [LEN](../../t-sql/functions/len-transact-sql.md)<br /><br /> [PATINDEX](../../t-sql/functions/patindex-transact-sql.md)|UTF-16 代理项对作为单个码位计数。|UTF-16 代理项对作为两个码位计数。|    
|[LEFT](../../t-sql/functions/left-transact-sql.md)<br /><br /> [REPLACE](../../t-sql/functions/replace-transact-sql.md)<br /><br /> [REVERSE](../../t-sql/functions/reverse-transact-sql.md)<br /><br /> [RIGHT](../../t-sql/functions/right-transact-sql.md)<br /><br /> [SUBSTRING](../../t-sql/functions/substring-transact-sql.md)<br /><br /> [STUFF](../../t-sql/functions/stuff-transact-sql.md)|这些函数会将每个代理项对作为单个码位处理并按预期方式工作。|这些函数可能拆分任意代理项对并导致意外的结果。|    
|[NCHAR](../../t-sql/functions/nchar-transact-sql.md)|返回对应于 0-0x10FFFF 范围内指定的 Unicode 码位值的字符。 如果指定的值位于 0-0xFFFF 范围内，则返回一个字符。 对于较高的值，则返回相应的代理项。|对于高于 0xFFFF 的值，将返回 NULL 而非相应的代理项。|    
|[UNICODE](../../t-sql/functions/unicode-transact-sql.md)|返回 0-0x10FFFF 范围内的一个 UTF-16 码位。|返回 0-0xFFFF 范围内的一个 UCS-2 码位。|    
|[匹配一个通配符](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)<br /><br /> [通配符 - 无需匹配的字符](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)|增补字符支持所有通配符操作。|增补字符不支持这些通配符操作。 支持其他通配符运算符。|    
    
## <a name="gb18030-support"></a><a name="GB18030"></a> GB18030 支持    
GB18030 是中国对中文字符进行编码的一个单独标准。 在 GB18030 中，字符长度可以是 1 个字节、2 个字节或 4 个字节。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过对从客户端应用程序进入服务器的 GB18030 编码字符进行确认，然后在本机将其转换并存储为 Unicode 字符，来对这些字符提供支持。 这些字符存储在服务器中后，在所有后续操作中均视为 Unicode 字符。 

可以使用任何中文排序规则，最好使用最新的 100 版本。 所有 \_100 级排序规则均支持使用 GB18030 字符进行语言排序。 如果数据中包含增补字符（代理项对），则可以使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中提供的 SC 排序规则来改进搜索和排序操作。    

> [!NOTE]
> 确保 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 等客户端工具使用 Dengxian 字体来正确地显示包含 GB18030 编码字符的字符串。
    
## <a name="complex-script-support"></a><a name="Complex_script"></a> 复杂文种支持    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可支持输入、存储、更改和显示复杂文种。 复杂文种包括下列几种类型：    
    
-   脚本包括从右到左和从左到右两种文字组合，如阿拉伯语和英语文字的组合。    
-   脚本的字符根据其位置或在与其他字符组合时改变形状，如阿拉伯语、印度语和泰语字符。    
-   像泰语这样的语言需要内部词典识别单词，因为这些单词之间不断字。    
    
与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 交互的数据库应用程序必须使用支持复杂文种的控件。 在托管代码中创建的标准 Windows 窗体控件支持复杂文种。    

## <a name="japanese-collations-added-in--sssql17-md"></a><a name="Japanese_Collations"></a> 在 [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 中添加的日语排序规则
 
从 [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 开始，支持新的日语排序规则系列，并具有各种排列选项（\_CS、\_AS、\_KS、\_WS 和 \_VSS）。 

若要列出这些排序规则，可以查询 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]：      

```sql 
SELECT Name, Description FROM fn_helpcollations()  
WHERE Name LIKE 'Japanese_Bushu_Kakusu_140%' OR Name LIKE 'Japanese_XJIS_140%'
``` 

由于所有新排序规则都内置有对增补字符的支持，因此新 \_140 排序规则都没有（或不需要）SC 标志。

[!INCLUDE[ssde_md](../../includes/ssde_md.md)]索引、内存优化表、列存储索引和本机编译模块支持这些排序规则。

<a name="ctp23"></a>

## <a name="utf-8-support"></a><a name="utf8"></a> UTF-8 支持
[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 完全支持广泛使用的 UTF-8 字符编码作为导入或导出编码，以及作为字符串数据的数据库级别或列级别排序规则。 UTF-8 受 char 和 varchar 数据类型支持，并在创建对象的排序规则或将其更改为带有 UTF8 后缀的排序规则时启用。 例如，将 LATIN1_GENERAL_100_CI_AS_SC 更改为 LATIN1_GENERAL_100_CI_AS_SC_UTF8 。 

UTF-8 仅适用于支持增补字符的 Windows 排序规则，如 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中所述。 nchar 和 nvarchar 数据类型仅支持 UCS-2 或 UTF-16 编码，并保持不变。

### <a name="storage-differences-between-utf-8-and-utf-16"></a><a name="storage_differences"></a> UTF-8 与 UTF-16 的存储差异
Unicode 联盟为每个字符都分配一个唯一码位（介于 000000-10FFFF 之间的值）。 使用 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 时，UTF-8 和 UTF-16 编码都可用来表示整个范围：    
-  如果使用 UTF-8 编码，ASCII 范围（000000-00007F）内的字符需要 1 个字节，介于 000080 和 0007FF、000800 和 00FFFF 以及 0010000 和 0010FFFF 之间的码位分别需要 2、3 和 4 个字节。 
-  如果使用 UTF-16 编码，介于 000000 和 00FFFF 以及 0010000 和 0010FFFF 之间的码位分别需要 2 和 4 个字节。 

下表列出了各个字符范围和编码类型的编码存储字节：

|代码范围（十六进制）|代码范围（十进制）|使用 UTF-8 时的存储字节<sup>1</sup>|使用 UTF-16 时的存储字节<sup>1</sup>|    
|---------------------------------|---------------------------------|--------------------------|-----------------------------|   
|000000–00007F|0–127|1|2|
|000080–00009F<br />0000A0–0003FF<br />000400–0007FF|128–159<br />160–1,023<br />1,024–2,047|2|2|
|000800–003FFF<br />004000–00FFFF|2,048–16,383<br />16,384–65,535|3|2|
|010000–03FFFF<sup>2</sup><br /><br />040000–10FFFF<sup>2</sup>|65,536–262,143<sup>2</sup><br /><br />262,144–1,114,111<sup>2</sup>|4|4|

<sup>1</sup> 存储字节是指编码字节长度，而不是数据类型在磁盘上的存储大小。 若要详细了解磁盘上的存储大小，请参阅 [nchar 和 nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)，以及 [char 和 varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md)。

<sup>2</sup>[增补字符](#Supplementary_Characters)的码位范围。

> [!TIP]   
> 通常认为，在 [CHAR(n) 和 VARCHAR(n)](../../t-sql/data-types/char-and-varchar-transact-sql.md) 或在 [NCHAR(n) 和 NVARCHAR(n)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) 中，n 定义字符数    。 这是因为在示例 CHAR(10) 列中，可以使用排序规则（如 Latin1_General_100_CI_AI）存储在 0-127 范围内的 10 ASCII 字符，因为此范围内的每个字符仅使用 1 个字节。
>    
> 但是，在 [CHAR(n) 和 VARCHAR(n)](../../t-sql/data-types/char-and-varchar-transact-sql.md) 中，n 以字节数 (0-8,000) 定义字符串大小，而在 [NCHAR(n) 和 NVARCHAR(n)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) 中，n 以字节对 (0-4,000) 定义字符串大小        。 n 不会定义可存储的字符数。

如你所见，选择适当的 Unicode 编码和数据类型可以节省大量存储或增加当前存储占用，具体视使用的字符集而定。 例如，如果使用启用了 UTF-8 的拉丁语排序规则（如 Latin1_General_100_CI_AI_SC_UTF8），则 `CHAR(10)` 列可存储 10 个字节，并且可保留 0-127 范围内的 10 ASCII 字符。 但只可保留 5 个 128-2047 范围内的字符和 3 个 2048-65535 范围内的字符。 相比之下，由于 `NCHAR(10)` 列存储 10 个字节对（20 个字节），因此该列可保留 10 个 0-65535 范围内的字符。  

在选择是要将 UTF-8 编码还是 UTF-16 编码用于数据库或列前，请先考虑要存储的字符串数据的分布情况：
-  如果它主要在 ASCII 范围 0-127 内（如英语），使用 UTF-8 和 UTF-16 时每个字符分别需要 1 个和 2 个字节。 UTF-8 具有存储优势。 如果使用已启用 UTF-8 的排序规则将包含在 0-127 范围内的 ASCII 字符的现有列数据类型从 `NCHAR(10)` 更改为 `CHAR(10)`，则会减少 50% 的存储需求。 之所以会有这种减少是因为，`NCHAR(10)` 需要 20 个字节进行存储，而 `CHAR(10)` 相比则需要 10 个字节用于相同的 Unicode 字符串表示形式。    
-  如果超出 ASCII 范围（几乎所有拉丁字母语言以及希腊语、西里尔文、科普特语、亚美尼亚语、希伯来语、阿拉伯语、叙利亚语、它拿语和西非书面文），使用 UTF-8 和 UTF-16 时每个字符都需要 2 个字节。 在这种情况下，可比较的数据类型（例如，char 与 nchar 之间）没有显著的存储差异。
-  如果它主要是东亚语言（如韩语、中文和日语），使用 UTF-8 和 UTF-16 时每个字符分别需要 3 个和 2 个字节。 UTF-16 具有存储优势。 
-  使用 UTF-8 和 UTF-16 时，介于 010000 和 10FFFF 范围内的字符都需要 4 个字节。 在这种情况下，可比较的数据类型（例如，char 与 nchar 之间）没有存储差异。

有关其他注意事项，请参阅[编写国际化 Transact-SQL 语句](../../relational-databases/collations/write-international-transact-sql-statements.md)。

### <a name="converting-to-utf-8"></a><a name="converting"></a> 转换为 UTF-8
因为在 [CHAR(n) 和 VARCHAR(n)](../../t-sql/data-types/char-and-varchar-transact-sql.md) 或在 [NCHAR(n) 和 NVARCHAR(n)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) 中，n 定义字节存储大小，而不定义可以存储的字符数，所以确定必须转换的数据类型大小很重要，这可以避免数据被截断    。 

例如，考虑定义为 NVARCHAR(100) 的列，该列存储了 180 个字节的日语字符。 在本示例中，当前使用 UCS-2 或 UTF-16 对列数据进行编码，每个字符使用 2 个字节。 将列类型转换为 VARCHAR(200) 不足以防止数据被截断，因为新的数据类型只能存储 200 个字节，而使用 UTF-8 编码时，日语字符需要 3 个字节。 因此，必须将列定义为 VARCHAR(270)，以避免由于数据截断而丢失数据。

因此，在将现有数据转换为 UTF-8 之前，需要事先知道列定义的预计字节大小，并相应地调整新数据类型的大小。 请参阅[数据示例 GitHub](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/unicode) 中的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本或 SQL 笔记本，其中使用 [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) 函数和 [COLLATE](../../t-sql/statements/collations.md) 语句来确定现有数据库中 UTF-8 转换操作的正确数据长度要求。

要更改现有表中的列排序规则和数据类型，请使用[设置或更改列排序规则](../../relational-databases/collations/set-or-change-the-column-collation.md)中所述的一种方法。

要更改数据库排序规则（默认允许新对象继承数据库排序规则）或更改服务器排序规则（默认允许新数据库继承系统排序规则），请参阅本文的[相关任务](#Related_Tasks)部分。 

##  <a name="related-tasks"></a><a name="Related_Tasks"></a> Related tasks    
    
|任务|主题|    
|----------|-----------|    
|介绍如何设置或更改 SQL Server 实例的排序规则。 请注意，更改服务器排序规则不会更改现有数据库的排序规则。|[设置或更改服务器排序规则](../../relational-databases/collations/set-or-change-the-server-collation.md)|    
|介绍如何设置或更改用户数据库的排序规则。 请注意，更改数据库排序规则不会更改现有表列的排序规则。|[设置或更改数据库排序规则](../../relational-databases/collations/set-or-change-the-database-collation.md)|    
|介绍如何设置或更改数据库中的列的排序规则|[设置或更改列排序规则](../../relational-databases/collations/set-or-change-the-column-collation.md)|    
|介绍如何返回服务器级、数据库级或列级的排序规则信息|[查看排序规则信息](../../relational-databases/collations/view-collation-information.md)|    
|介绍如何编写更易于在不同语言之间移植或更轻松地支持多种语言的 Transact-SQL 语句|[编写国际化 Transact-SQL 语句](../../relational-databases/collations/write-international-transact-sql-statements.md)|    
|介绍如何更改有关日期、时间和货币数据的使用和显示方式的错误消息和首选项的语言|[设置会话语言](../../relational-databases/collations/set-a-session-language.md)|    
    
##  <a name="related-content"></a><a name="Related_Content"></a> Related content    
有关详细信息，请参阅下列相关内容：
* [SQL Server Best Practices Collation Change](https://go.microsoft.com/fwlink/?LinkId=113891)  
* [使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)
* [编写国际化 Transact-SQL 语句](../../relational-databases/collations/write-international-transact-sql-statements.md)  
* [SQL Server 最佳做法 - 迁移到 Unicode](https://go.microsoft.com/fwlink/?LinkId=113890) - 不再进行维护   
* [Unicode Consortium](https://go.microsoft.com/fwlink/?LinkId=48619)  
* [Unicode 标准](http://www.unicode.org/standard/standard.html)  
* [适用于 SQL Server 的 OLE DB 驱动程序中的 UTF-8 支持](../../connect/oledb/features/utf-8-support-in-oledb-driver-for-sql-server.md)  
* [SQL Server 排序规则名称 (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)  
* [Windows 排序规则名称 (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md)  
* [SQL Server 的 UTF-8 支持简介](https://techcommunity.microsoft.com/t5/SQL-Server/Introducing-UTF-8-support-for-SQL-Server/ba-p/734928)（SQL Server 的 UTF-8 支持简介）      
* [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md)      
* [排序规则优先级](../../t-sql/statements/collation-precedence-transact-sql.md)    
    
## <a name="see-also"></a>另请参阅    
[包含数据库的排序规则](../../relational-databases/databases/contained-database-collations.md)     
[创建全文索引时选择语言](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md)     
[sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)       
[单字节和多字节字符集](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)