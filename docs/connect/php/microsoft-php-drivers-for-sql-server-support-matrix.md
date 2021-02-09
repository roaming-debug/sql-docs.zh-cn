---
title: Microsoft Drivers for PHP 支持矩阵
description: 本页包含 SQL Server 的 Microsoft PHP 驱动程序的支持矩阵和支持生命周期策略。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ca05a57c88f93a890cf546b3b1c60382f672d3be
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076429"
---
# <a name="microsoft-php-drivers-for-sql-server-support-matrix"></a>SQL Server 的 Microsoft PHP 驱动程序支持矩阵

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

本页包含 SQL Server 的 Microsoft PHP 驱动程序的支持矩阵和支持生命周期策略。

## <a name="microsoft-php-drivers-support-lifecycle-matrix-and-policy"></a>Microsoft PHP 驱动程序支持生命周期矩阵和策略

Microsoft 支持生命周期 (MSL) 策略提供了与 Microsoft 产品的支持生命周期有关的可预测透明信息。 PHP Driver 3.x 版、4.x 版和 5.x 版提供五年主要支持（自驱动程序发布之日起开始计算）。 主流支持在 [Microsoft 支持生命周期网站](https://support.microsoft.com/lifecycle)上定义。

Microsoft PHP 驱动程序不提供扩展和自定义支持选项。

支持以下 Microsoft PHP 驱动程序，直到指定的支持结束日期。

|驱动程序名称|驱动程序包版本|主要支持结束日期|
|-|:-:|-|
|Microsoft PHP Drivers 5.9 for SQL Server|5.9|2026 年 1 月 29 日|
|Microsoft PHP Drivers 5.8 for SQL Server|5.8|2025 年 1 月 31 日|
|Microsoft PHP Driver 5.6 for SQL Server|5.6|2024 年 2 月 21 日|
|Microsoft PHP Driver 5.3 for SQL Server|5.3|2023 年 7 月 20 日|
|Microsoft PHP Driver 5.2 for SQL Server|5.2|2023 年 2 月 9 日|
|Microsoft PHP Driver 4.3 for SQL Server|4.3|2022 年 7 月 6 日|
|Microsoft PHP Driver 4.0 for SQL Server|4.0|2021 年 7 月 11 日|
| &nbsp; | &nbsp; | &nbsp; |

不再支持以下 Microsoft PHP 驱动程序。

|驱动程序名称|驱动程序包版本|主要支持结束日期|
|-|:-:|-|
|Microsoft PHP Driver 3.2 for SQL Server|3.2|2020 年 3 月 9 日|
|Microsoft PHP Driver 3.1 for SQL Server|3.1|2019 年 12 月 12 日|
|Microsoft PHP Driver 3.0 for SQL Server|3.0|2017 年 3 月 6 日|
|Microsoft PHP Driver 2.0 for SQL Server|2.0|2015 年 8 月 10 日|
|Microsoft PHP Driver 1.0 for SQL Server|1.0|2014 年 4 月 28 日|
| &nbsp; | &nbsp; | &nbsp; |

## <a name="sql-server-version-certified-compatibility"></a>SQL Server 版本认证的兼容性
 下面的矩阵列出了经过测试并已验证与相应驱动程序版本兼容的数据库版本。 我们努力保持与以前驱动程序版本的向后兼容性，但是随着 SQL Server 的发布，只有最新受支持的驱动程序才会通过新的 SQL Server 版本进行测试和认证。

|驱动程序版本&nbsp;&#8594;<br />&#8595; 数据库版本|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Azure SQL Database        |是|是|“是”|是|是|是|   |   |
|Azure SQL 托管实例|“是”|是|是|是|是|是|   |   |
|Azure Synapse Analytics   |是|是|是|是|是|是|   |   |
|SQL Server 2019           |是|是|   |   |   |   |   |   |
|SQL Server 2017           |是|是|是|是|是|是|   |   |
|SQL Server 2016           |是|是|是|是|是|是|是|   |
|SQL Server 2014           |是|是|是|是|是|是|是|是|
|SQL Server 2012           |是|是|是|是|是|是|是|是|
|SQL Server 2008 R2        |   |   |是|是|是|是|是|是|
|SQL Server 2008           |   |   |   |   |   |   |是|是|
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

有关对 Azure SQL 数据库使用 PHP 的信息，请参阅[连接到 Microsoft Azure SQL 数据库](connecting-to-microsoft-azure-sql-database.md)。

## <a name="php-version-support"></a>PHP 版本支持

列出的 Microsoft PHP 驱动程序版本支持以下版本的 PHP：

|驱动程序版本&nbsp;&#8594;<br />&#8595; PHP 版本|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|:---:|---|---|---|---|---|---|---|---|
|8.0|8.0.0+ |                |                |                |                |       |        |        |
|7.4|7.4.0+ |7.4.0+          |                |                |                |       |        |        |
|7.3|7.3.0+ |7.3.0+          |7.3.0+          |                |                |       |        |        |
|7.2|       |7.2+<sup>1</sup>|7.2+<sup>1</sup>|7.2+<sup>1</sup>|7.2+<sup>1</sup>|       |        |        |
|7.1|       |                |7.1.0+          |7.1.0+          |7.1.0+          |7.1.0+ |        |        |
|7.0|       |                |                |7.0.0+          |7.0.0+          |7.0.0+ |7.0.0+  |        |
|5.6|       |                |                |                |                |       |        |5.6.4+  |
|5.5|       |                |                |                |                |       |        |5.5.16+ |
|5.4|       |                |                |                |                |       |        |5.4.32  |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup> Windows 支持版本 7.2.1 及更高版本，而 Linux 和 macOS 支持版本 7.2.0 及更高版本。

## <a name="supported-operating-systems"></a>受支持的操作系统

列出的 Microsoft PHP 驱动程序版本支持以下 Windows 操作系统版本：

|驱动程序版本&nbsp;&#8594;<br />&#8595; 操作系统|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Windows Server 2019                 |是|是|是|   |   |   |   |   |
|Windows Server 2016                 |是|是|是|是|是|是|   |   |
|Windows Server 2012 R2              |是|是|是|是|是|是|是|是|
|Windows Server 2012                 |是|是|是|是|是|是|是|是|
|Windows Server 2008 R2 SP1          |   |   |   |   |   |   |是|是|
|Windows Server 2008 R2              |   |   |   |   |   |   |   |   |
|Windows Server 2008 SP2             |   |   |   |   |   |   |是|是|
|Windows 10                          |是|是|是|是|是|是|是|   |
|Windows 8.1                         |是|是|是|是|是|是|是|是|
|Windows 8                           |   |   |   |   |   |是|是|是|
|Windows 7 SP1                       |   |   |   |   |   |   |是|是|
|Windows Vista SP2                   |   |   |   |   |   |   |是|是|
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

以下 Linux 和 macOS 操作系统版本（仅限 64 位）可以与列出的 Microsoft PHP 驱动程序版本配合使用：

|驱动程序版本&nbsp;&#8594;<br />&#8595; 操作系统|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Ubuntu 20.10（64 位）               |是|   |   |   |   |   |   |   |
|Ubuntu 20.04（64 位）               |是|是|   |   |   |   |   |   |
|Ubuntu 19.10（64 位）               |   |是|   |   |   |   |   |   |
|Ubuntu 18.10（64 位）               |   |   |是|   |   |   |   |   |
|Ubuntu 18.04（64 位）               |是|是|是|是|   |   |   |   |
|Ubuntu 17.10（64 位）               |   |   |   |是|是|   |   |   |
|Ubuntu 16.04（64 位）               |是|是|是|是|是|是|是|   |
|Ubuntu 15.10（64 位）               |   |   |   |   |   |是|   |   |
|Ubuntu 15.04（64 位）               |   |   |   |   |   |   |是|   |
|Debian 10（64 位）                  |是|是|   |   |   |   |   |   |
|Debian 9（64 位）                   |是|是|是|是|是|   |   |   |
|Debian 8（64 位）                   |   |是|是|是|是|是|   |   |
|Red Hat Enterprise Linux 8（64 位） |是|是|   |   |   |   |   |   |
|Red Hat Enterprise Linux 7（64 位） |是|是|是|是|是|是|是|   |
|Suse Enterprise Linux 15（64 位）   |是|是|是|   |   |   |   |   |
|Suse Enterprise Linux 12（64 位）   |是|是|是|是|是|   |   |   |
|Alpine Linux 3.12（64 位）          |是|   |   |   |   |   |   |   |
|Alpine Linux 3.11（64 位）          |是|是<sup>1</sup>|   |   |   |   |   |   |
|macOS Big Sur（64 位）              |是|   |   |   |   |   |   |   |
|macOS Catalina（64 位）             |是|是|   |   |   |   |   |   |
|macOS Mojave（64 位）               |是|是|是|   |   |   |   |   |
|macOS High Sierra（64 位）          |   |是|是|是|   |   |   |   |
|macOS Sierra（64 位）               |   |   |是|是|是|是|   |   |
|macOS El Capitan（64 位）           |   |   |   |是|是|是|   |   |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup>Alpine Linux 支持是版本 5.8.0 的实验性支持。 版本 5.8.1 引入了生产支持。

## <a name="see-also"></a>另请参阅

[发行说明](release-notes-php-sql-driver.md)

[支持资源](support-resources-for-the-php-sql-driver.md)

[系统要求](system-requirements-for-the-php-sql-driver.md)
