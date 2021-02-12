---
title: Microsoft Drivers for PHP for SQL Server 概述
description: 了解用于 Microsoft SQLSRV Drivers for PHP for SQL Server 和 PDO_SQSRV Drivers for PHP for SQL Server 的概述，以及如何在 PHP 应用程序中使用这些驱动程序来访问数据库。
ms.custom: ''
ms.date: 03/27/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 66559249-34c0-409d-b919-9b5bf0c4c9ec
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 687f8a87ea03ef661b96af27a574bb5cea09f847
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045267"
---
# <a name="overview-of-the-microsoft-drivers-for-php-for-sql-server"></a>Microsoft Drivers for PHP for SQL Server 概述

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 是 PHP 扩展，用于提供对 SQL Server 2005 及更高版本（包括 Azure SQL 数据库）的数据访问。 该扩展提供的过程接口（附带 SQLSRV 驱动程序）和面向对象的接口（附带 PDO_SQLSRV 驱动程序）用于访问自 SQL Server 2005 以来的所有 SQL Server 版本（包括 Express）中的数据。 支持自 SQL Server 2008 以来的 3.1 版本及更高版本的驱动程序。 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] API 包括对 Windows 身份验证、事务、参数绑定、流式传输、元数据访问和错误处理的支持。  
  
若要使用 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]，必须在运行 PHP 的同一台计算机上安装正确版本的 SQL Server Native Client 或 Microsoft ODBC Driver。  有关详细信息，请参阅 [Microsoft Drivers for PHP for SQL Server 的系统要求](../../connect/php/system-requirements-for-the-php-sql-driver.md)。  
  
## <a name="in-this-section"></a>本节内容  
  
|主题|说明|  
|---------|---------------|  
| ![Download-DownArrow-Circled](../../ssms/media/download-icon.png)[下载 Driver for PHP for SQL Server 的具体步骤](download-drivers-php-sql-server.md) | 下载 Microsoft Drivers for PHP for SQL Server 的链接。 |
|[Microsoft Drivers for PHP for SQL Server 发行说明](../../connect/php/release-notes-php-sql-driver.md)|列出版本 4.0、3.2、3.1、3.0 和 2.0 中的新增功能。|  
|[Microsoft Drivers for PHP for SQL Server 的支持资源](../../connect/php/support-resources-for-the-php-sql-driver.md)|在开发使用 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的应用程序时，提供可能有用的资源的链接。|  
|[文档中相关的代码示例](../../connect/php/about-code-examples-in-the-documentation.md)|在运行本文档中的代码示例时，提供可能有用的信息。|  
| &nbsp; | &nbsp; |

## <a name="reference"></a>引用

[SQLSRV 驱动程序 API 参考](../../connect/php/sqlsrv-driver-api-reference.md)  
[PDO_SQLSRV 驱动程序参考](../../connect/php/pdo-sqlsrv-driver-reference.md)  
[常量（Microsoft Drivers for PHP for SQL Server）](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)  

## <a name="see-also"></a>另请参阅

[开始使用 Microsoft Drivers for PHP for SQL Server](../../connect/php/getting-started-with-the-php-sql-driver.md)  
[Microsoft Drivers for PHP for SQL Server 编程指南](../../connect/php/programming-guide-for-php-sql-driver.md)  
[示例应用程序（SQLSRV 驱动程序）](../../connect/php/example-application-sqlsrv-driver.md)  
