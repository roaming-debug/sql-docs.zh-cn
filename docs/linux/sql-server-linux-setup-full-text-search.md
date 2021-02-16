---
title: 在 Linux 上安装 SQL Server 全文搜索
description: 了解如何在 Linux 上安装 SQL Server 全文搜索。 全文搜索提供了对 SQL Server 表中基于字符的数据运行全文查询的功能。
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: bb42076f-e823-4cee-9281-cd3f83ae42f5
ms.openlocfilehash: 406c1a5eb384db2d03514193e0e252ca89c94bd4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346375"
---
# <a name="install-sql-server-full-text-search-on-linux"></a>在 Linux 上安装 SQL Server 全文搜索

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

下列步骤介绍在 Linux 上安装[SQL Server 全文搜索](../relational-databases/search/full-text-search.md) (mssql-server-fts)。 全文搜索提供了对 SQL Server 表中基于字符的数据运行全文查询的功能。 对于此版本的已知问题，请参阅[发行说明](sql-server-linux-release-notes.md)。

> [!NOTE]
> 在安装 SQL Server 全文搜索前，首先请[安装 SQL Server](sql-server-linux-setup.md#platforms)。 这将配置安装 mssql-server-fts 包时要用到的密钥和存储库。

为以下平台安装 SQL Server 全文搜索：

- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)

## <a name=""></a><a name="RHEL">在 RHEL 上安装</a>

通过下列命令在 Red Hat Enterprise Linux 上安装 mssql-server-fts。 

```bash
sudo yum install -y mssql-server-fts
```

如果已安装 mssql-server-fts，可使用下列命令将其更新至最新版本：

```bash
sudo yum check-update
sudo yum update mssql-server-fts
```

如果需要脱机安装，请在[发行说明](sql-server-linux-release-notes.md)中找到全文搜索包下载。 然后执行与文章[安装 SQL Server](sql-server-linux-setup.md#offline) 所述相同的脱机安装步骤。

## <a name=""></a><a name="ubuntu">在 Ubuntu 上安装</a>

通过下列命令在 Ubuntu 上安装 mssql-server-fts。 

```bash
sudo apt-get update 
sudo apt-get install -y mssql-server-fts
```

如果已安装 mssql-server-fts，可使用下列命令将其更新至最新版本：

```bash
sudo apt-get update 
sudo apt-get install -y mssql-server-fts 
```

如果需要脱机安装，请在[发行说明](sql-server-linux-release-notes.md)中找到全文搜索包下载。 然后执行与文章[安装 SQL Server](sql-server-linux-setup.md#offline) 所述相同的脱机安装步骤。

## <a name=""></a><a name="SLES">在 SLES 上安装</a>

通过下列命令在 SUSE Linux Enterprise Server 上安装 mssql-server-fts。 

```bash
sudo zypper install mssql-server-fts
```

如果已安装 mssql-server-fts，可使用下列命令将其更新至最新版本：

```bash
sudo zypper refresh
sudo zypper update mssql-server-fts
```

如果需要脱机安装，请在[发行说明](sql-server-linux-release-notes.md)中找到全文搜索包下载。 然后执行与文章[安装 SQL Server](sql-server-linux-setup.md#offline) 所述相同的脱机安装步骤。

## <a name="supported-languages"></a>支持的语言

全文搜索使用[分词系统](../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)来确定如何标识基于语言的单个单词。 可通过查询 sys.fulltext_languages 目录视图获取注册的分词系统列表。 SQL Server 已安装以下语言的分词系统：

| Language | 语言 ID |
|---|---|
| 中立 | 0 |
| 阿拉伯语 | 1025 |
| 孟加拉语（印度） | 1093 |
| 书面挪威语 | 1044 |
| 葡萄牙语(巴西) | 1046 |
| 英语（英国） | 2057 |
| 保加利亚语 | 1026 |
| 加泰罗尼亚语 | 1027 |
| 中文（香港特别行政区） | 3076 |
| 中文（澳门特别行政区） | 5124 |
| 中文（新加坡） | 4100 |
| 克罗地亚语 | 1050 |
| 捷克语 | 1029 |
| 丹麦语 | 1030 |
| 荷兰语 | 1043 |
| 英语 | 2052 |
| 法语 | 1036 |
| 德语 | 1031 |
| 希腊语 | 1032 |
| 古吉拉特语 | 1095 |
| 希伯来语 | 1037 |
| Hindi | 1081 |
| 冰岛语 | 1039 |
| 印度尼西亚语 | 1057 |
| 意大利语 | 1040 |
| 日语 | 1041 |
| 卡纳达语 | 1099 |
| 韩语 | 1042 |
| 拉脱维亚语 | 1062 |
| 立陶宛语 | 1063 |
| 马来语（马来西亚） | 1086 |
| 马拉雅拉姆语 | 1100 |
| 马拉地语 | 1102 |
| 波兰语 | 1045 |
| 葡萄牙语 | 2070 |
| 旁遮普语 | 1094 |
| 罗马尼亚语 | 1048 |
| 俄语 | 1049 |
| 塞尔维亚语（西里尔） | 3098 |
| 塞尔维亚语（拉丁） | 2074 |
| 简体中文 | 2052 |
| 斯洛伐克语 | 1051 |
| 斯洛文尼亚语 | 1060 |
| 西班牙语 | 3082 |
| 瑞典语 | 1053 |
| 泰米尔语 | 1097 |
| 泰卢固语 | 1098 |
| 泰语 | 1054 |
| 繁体中文 | 1028 |
| 土耳其语 | 1055 |
| 乌克兰语 | 1058 |
| 乌尔都语 | 1056 |
| 越南语 | 1066 |

## <a name="filters"></a><a id="filters"></a> 筛选器

全文搜索还适用于二进制文件中存储的文本。 但在这种情况下，需要安装一个筛选器来处理文件。 有关筛选器的详细信息，请参阅[配置和管理搜索筛选器](../relational-databases/search/configure-and-manage-filters-for-search.md)

可调用 sp_help_fulltext_system_components 'filter' 来查看已安装的筛选器列表。 对于 SQL Server，已安装的筛选器包括：

| 组件名称 | 类 ID | 版本 |
|---|---|---|
|.a | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.ans | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.asc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.ascx | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.asm | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.asp | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.aspx | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.asx | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.bas | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.bat | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.bcp | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.c | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.cc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.cls | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.cmd | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.cpp | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.cs | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.csa | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.css | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.csv | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.cxx | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.dbs | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.def | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.dic | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.dos | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.dsp | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.dsw | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.ext | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.faq | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.fky | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.h | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.hhc | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.hpp | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.hta | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.htm | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.html | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.htt | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.htw | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.htx | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.hxx | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.i | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.ibq | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.ics | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.idl | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.idq | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.inc | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.inf | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.ini | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.inl | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.inx | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.jav | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.java | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.js | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.kci | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.lgn | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.log | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.lst | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.m3u | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.mak | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.mk | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.odc | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.odh | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.odl | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.pkgdef | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.pkgundef | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.pl | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.prc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.rc | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.rc2 | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.rct | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.reg | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.rgs | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.rtf | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.rul | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.s | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.scc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.shtm | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.shtml | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.snippet | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.sol | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.sor | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.srf | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.stm | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.tab | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.tdl | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.tlh | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.tli | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.trg | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.txt | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.udf | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.udt | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.url | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.usr | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vbs | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.viw | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vsct | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vsixlangpack | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vsixmanifest | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vspscc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vsscc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vssscc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.wri | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.wtx | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.xml | 41B9BE05-B3AF-460C-BF0B-2CDD44A093B1 | 12.0.9735.0 |

## <a name="semantic-search"></a>语义搜索
[语义搜索](../relational-databases/search/semantic-search-sql-server.md)建立在全文搜索功能的基础上，用于提取和索引与统计相关的关键短语。 这使你能查询数据库中文档内的含义。 它还有助于标识其他类似的文档。

要使用语义搜索，必须先将语义语言统计信息数据库还原到你的计算机。

1. 使用工具，如 [sqlcmd](sql-server-linux-setup-tools.md) 在 Linux SQL Server 实例上运行以下 Transact-SQL 命令。 此命令还原语言统计信息数据库。

   ```sql
   RESTORE DATABASE [semanticsdb] FROM
   DISK = N'/opt/mssql/misc/semanticsdb.bak' WITH FILE = 1,
   MOVE N'semanticsdb' TO N'/var/opt/mssql/data/semanticsDB.mdf',
   MOVE N'semanticsdb_log' TO N'/var/opt/mssql/data/semanticsdb_log.ldf', NOUNLOAD, STATS = 5
   GO
   ```

   > [!NOTE]
   > 如有必要，请更新上一个“还原”命令中的路径以调整你的配置。

1. 运行以下 Transact-SQL 命令，注册语义语言统计信息数据库。

    ```sql
    EXEC sp_fulltext_semantic_register_language_statistics_db @dbname = N'semanticsdb';  
    GO
    ```

## <a name="next-steps"></a>后续步骤

有关全文搜索的相关信息，请参阅 [SQL Server 全文搜索](../relational-databases/search/full-text-search.md)。 
