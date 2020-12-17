---
title: 以前版本的 SQL Server Data Tools (SSDT)
description: 了解 SSDT 和 SSDT-BI 的哪些版本适用于 Visual Studio 的哪些版本。 了解如何安装不同版本的 SSDT 和 SSDT-BI。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 5d32e301-0f44-4916-b0db-76e8322c0ab7
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 06/17/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 35c16a4184665052781ae88ad37635ede298d668
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440491"
---
# <a name="previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi"></a>以前版本的 SQL Server Data Tools（SSDT 和 SSDT-BI）

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server Data Tools (SSDT) 为生成 SQL Server 内容类型（关系数据库、Analysis Services 模型、Reporting Services 报表和 Integration Services 包）提供项目模板和设计图面。

SSDT 可向后兼容，因此用户始终都可以使用[最新的 SSDT](download-sql-server-data-tools-ssdt.md) 来设计和部署在较早版本 SQL Server 上运行的数据库、模型、报表和包。

以前，用于创建 SQL Server 内容类型的 Visual Studio shell 在各种不同的名称下发布，包括“SQL Server Data Tools”  、“SQL Server Data Tools-Business Intelligence”  和“Business Intelligence Development Studio”  。 以前的版本附带多组不同的项目模板。 若要在一个 SSDT 中同时获取全部项目模板，你需要 [最新版本](download-sql-server-data-tools-ssdt.md)。 否则，你可能需要安装多个以前的版本才可获取 SQL Server 中使用的全部模板。 每个 Visual Studio 版本只会安装一个 shell；安装另一个 SSDT 只会添加缺少的模板。

## <a name="previous-ssdt-releases"></a>以前的 SSDT 版本

通过选择相关部分中的下载链接下载以前的 SSDT 版本。

| SSDT 版本 | Visual Studio 版本 |
|--------------|-----------------------|
| [15.8](#ssdt-for-visual-studio-vs-2017) | 2017 |
| [17.4](#ssdt-for-visual-studio-vs-2015) | 2015 |
| [16.5](#ssdt-for-visual-studio-vs-2013) | 2013 |
| [11.1.50727.1](#ssdt-for-visual-studio-vs-2012) | 2012 |

### <a name="ssdt-for-visual-studio-vs-2017"></a>SSDT for Visual Studio (VS) 2017

**[下载 SSDT for Visual Studio 2017 (15.8)](https://go.microsoft.com/fwlink/?linkid=2124319)**

可以使用以下语言安装此 SSDT for Visual Studio 2017 版本：

[中文（简体）](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x804) | [中文（繁体）](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x404) | [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x409) | [法语](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x40c) | [德语](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x407) | [意大利语](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x410) | [日语](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x411) | [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x412) | [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x416) | [俄语](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x419) | [西班牙语](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x40a)

### <a name="ssdt-for-visual-studio-vs-2015"></a>SSDT for Visual Studio (VS) 2015

若要安装此版本的 SSDT，必须下载 ISO 映像。 该 ISO 文件是一个自包含文件，包含 SSDT 所需的所有组件并且可使用可重启的下载管理器进行下载，适用于网络带宽有限或网络不稳定的情形。 下载之后，该 ISO 即可作为驱动器安装。

安装步骤：

1. **[下载 SSDT for Visual Studio 2015 (17.4)](https://go.microsoft.com/fwlink/?linkid=2132817)** 。

2. 打开 ISO 映像。

3. 运行 SSDTSetup.exe 文件。

    ![ISO 映像](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

可以使用以下语言安装此 SSDT for Visual Studio 2015 版本：

| 语言 | SHA256 哈希 |
|----------|-------------|
| [简体中文](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x804) | 79A958B122DDEC2857F1F4B9F0272A6BD2106BB17B4DA94CC68CACFCDDC16EAE |
| [繁体中文](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x404) | 8F9661883F2D2D28D6928AE66242DB465DBB25696EDFE0348D222421CEAB000A |
| [英语（美国）](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x409) | 7727BA227A9E49C2DFCC1E9F2A10924CC472E3425653DC7DE8E4B830712B302E |
| [法语](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x40c) | 2DF6655819F604E8D20A396CA9FDFEE279C5A9E50776FFC143A5BA4C3E2F1849 |
| [德语](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x407) | 5C44502DEE8B31675D0B10B4CE8CA6F5D96A2692CC498B9859A77C24F30EF870 |
| [意大利语](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x410) | 6A616F6E3A1C7DD52457FB8C8692E5C1C551032FF46AD8C5112CF9364F17C631 |
| [日语](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x411) | 1244A5EADF673E4BD36E9967A2008F65CA2A1D40E8E7DD4D17640A6FC1EACA9A |
| [朝鲜语](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x412) | 6E0E057A853F54AB87F3F8984954F590FCCD3BE2EC2139F02EFA085FEA6D3E61 |
| [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x416) | 24122C092464B299F41A7644814F375F0CC2786877BCAE0282221FF8D73BD100 |
| [俄语](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x419) | 2CDE208C241C3F13D2EC37294C10503C7A9D1222ED33F6FE54849169F30BE697 |
| [西班牙语](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x40a) | BD12844E6F0A0ECFD5815D01EDFB8018CE9B4A1044DE8DF96AC740D85E800FD6 |

### <a name="ssdt-for-visual-studio-vs-2013"></a>SSDT for Visual Studio (VS) 2013

若要安装此版本的 SSDT，必须下载 ISO 映像。 该 ISO 文件是一个自包含文件，包含 SSDT 所需的所有组件并且可使用可重启的下载管理器进行下载，适用于网络带宽有限或网络不稳定的情形。 下载之后，该 ISO 即可作为驱动器安装。

安装步骤：

1. **[下载 SSDT for Visual Studio 2013 (16.5)](https://go.microsoft.com/fwlink/?linkid=832312)**

2. 打开 ISO 映像。

3. 运行 SSDTSetup.exe 文件。

    ![ISO 映像](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

可以使用以下语言安装此 SSDT for Visual Studio 2013 版本：

| 语言 | SHA256 哈希 |
|----------|-------------|
| [简体中文](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x804) | 79A958B122DDEC2857F1F4B9F0272A6BD2106BB17B4DA94CC68CACFCDDC16EAE |
| [繁体中文](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x404) | 8F9661883F2D2D28D6928AE66242DB465DBB25696EDFE0348D222421CEAB000A |
| [英语（美国）](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x409) | 7727BA227A9E49C2DFCC1E9F2A10924CC472E3425653DC7DE8E4B830712B302E |
| [法语](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x40c) | 2DF6655819F604E8D20A396CA9FDFEE279C5A9E50776FFC143A5BA4C3E2F1849 |
| [德语](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x407) | 5C44502DEE8B31675D0B10B4CE8CA6F5D96A2692CC498B9859A77C24F30EF870 |
| [意大利语](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x410) | 6A616F6E3A1C7DD52457FB8C8692E5C1C551032FF46AD8C5112CF9364F17C631 |
| [日语](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x411) | 1244A5EADF673E4BD36E9967A2008F65CA2A1D40E8E7DD4D17640A6FC1EACA9A |
| [朝鲜语](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x412) | 6E0E057A853F54AB87F3F8984954F590FCCD3BE2EC2139F02EFA085FEA6D3E61 |
| [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x416) | 24122C092464B299F41A7644814F375F0CC2786877BCAE0282221FF8D73BD100 |
| [俄语](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x419) | 2CDE208C241C3F13D2EC37294C10503C7A9D1222ED33F6FE54849169F30BE697 |
| [西班牙语](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x40a) | BD12844E6F0A0ECFD5815D01EDFB8018CE9B4A1044DE8DF96AC740D85E800FD6 |

### <a name="ssdt-for-visual-studio-vs-2012"></a>SSDT for Visual Studio (VS) 2012

若要安装此版本的 SSDT，必须下载 ISO 映像。 该 ISO 文件是一个自包含文件，包含 SSDT 所需的所有组件并且可使用可重启的下载管理器进行下载，适用于网络带宽有限或网络不稳定的情形。 下载之后，该 ISO 即可作为驱动器安装。

安装步骤：

1. **[下载 SSDT for Visual Studio 2012 (11.1.50727.1)](https://go.microsoft.com/fwlink/?linkid=518814)**

2. 打开 ISO 映像。

3. 运行 SSDTSetup.exe 文件。

    ![ISO 映像](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

可以使用以下语言安装此 SSDT for Visual Studio 2012 版本：

| 语言 | SHA256 哈希 |
|----------|-------------|
| [简体中文](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x804) | 78F20325648CFF49D9C58A26186E0AC199D104B3CF634E5663B4B262BEC69C07 |
| [繁体中文](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x404) | A2722380AF2EE1E8BB52B4FA54A61BAB411E5E5FD5B050108F81ED23DC87366D |
| [英语（美国）](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x409) | 748EF78D3F9CC6FE360432C378EA690DE51AEB2C747E87D43E08448D26F93A91 |
| [法语](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x40c) | 40E6504169BF618EDC7EB5B62D1215DC96726D6EEC3CA8EC3EEB49044E4B6FB7 |
| [德语](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x407) | C45C974E6B8F9611BA2AC1EE90C5C507992BCE5693BF46F6C7C138591ED6A123 |
| [意大利语](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x410) | C1B20CDB41C7B1B5DE76A71F9A091A6FDF5186B12F6AA269DA6338B3CB2D91A6 |
| [日语](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x411) | BF56958B904C1C5F28084BD0B16928B6CBFEC83EB3F0C913EC364FA335241943 |
| [朝鲜语](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x412) | 9023B0656785C357A67E39EB76A2806B923C2BD17342D7226A8ADA384A9F4E28 |
| [葡萄牙语（巴西）](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x416) | ACAD9FE03729E289ECE821D92C56CFB1D7FCCEA8423ABF1E164BF3C67ABEFEFB |
| [俄语](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x419) | E2191D787BA833DF4A85B064C5373DC44099E76214FBF9505728702D4D6B83F0 |
| [西班牙语](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x40a) | 6D81FB572A7003C54C29D2ACF076D2CED4A1CA80F329BFF9D41A806920D64EEE |

> [!Note]
> SSDT 支持两个最新版本的 Visual Studio。 Visual Studio 2019 发布后，Visual Studio 2015 及更早版本的 SSDT 版本将不再更新。 SSDT for Visual Studio 2010 不再可用。 有关详细信息，请参阅[此 SSDT 团队博客文章](/archive/blogs/ssdt/sql-server-data-tools-17-0-rc-and-ssdt-in-vs2017)的“常见问题解答”部分。

## <a name="sql-bi-analysis-services-reporting-services-integration-services"></a>SQL BI：Analysis Services、Reporting Services、Integration Services

BI 模板用于创建 SSAS 模型、SSRS 报表和 SSIS 包。 BI Designer 与特定版本的 SQL Server 关联。 若要使用较新的 BI 功能，请安装较新版本的 BI Designer。

## <a name="bi-designers"></a>BI Designer

[下载 SSDT-BI for Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)（SQL Server 2014、SQL Server 2012、SQL Server 2008 和 2008 R2）

[下载 SSDT-BI for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)（SQL Server 2014、SQL Server 2012、SQL Server 2008 和 2008 R2）

Business Intelligence Development Studio (BIDS) 通过 SQL Server 安装程序进行安装。 无网页版下载（SQL Server 2008 和 2008 R2）。

对于 SQL Server 2012 或 2014，你可以使用“” **SSDT-BI f或“” Visual Studio 2012** 或“” **SSDT-BI f或“” Visual Studio 2013**。 两者之间唯一的区别在于 Visual Studio 的版本。

## <a name="next-steps"></a>后续步骤

- [下载 SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [SQL Server Data Tools (SSDT) 发行说明](release-notes-ssdt.md)
- [下载 SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [下载 Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
- [SQL 工具和实用工具](../tools/overview-sql-tools.md)