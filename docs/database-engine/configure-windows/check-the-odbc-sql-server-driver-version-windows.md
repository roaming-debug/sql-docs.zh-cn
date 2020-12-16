---
title: 检查 ODBC SQL Server 驱动程序版本 (Windows) | Microsoft Docs
description: 了解如何使用 Windows ODBC 数据源管理器来查看计算机上安装的 ODBC 驱动程序版本。
ms.custom: ''
ms.date: 11/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- driver version number [ODBC]
- ODBC drivers, version number
ms.assetid: 43451080-a562-4231-b1d4-1ba35ca0ea79
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: e55654dedc42c00e0abfaaa4a4bbe37d311fa73f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480368"
---
# <a name="check-the-odbc-sql-server-driver-version-windows"></a>检查 ODBC SQL Server 驱动程序版本 (Windows)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  您的计算机可能包含来自 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 和其他公司的多种 ODBC 驱动程序。 本主题介绍如何使用 Windows **“ODBC 数据源管理器”** 来查看已安装的 ODBC 驱动程序的版本。  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-check-the-odbc-sql-server-driver-version-32-bit-odbc"></a>检查 ODBC SQL Server 驱动程序版本（32 位 ODBC）  
  
-   在 **“ODBC 数据源管理器”** 中，单击 **“驱动程序”** 选项卡。  
  
     有关 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 项的信息显示在 **“版本”** 列中。  


> [!NOTE]  
>  对于与 SQL 数据库的 Azure Active Directory 身份验证的连接，请安装最新的驱动程序，例如 [ODBC Driver 17 for SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md)。   

  
## <a name="see-also"></a>另请参阅  
 [打开 ODBC 数据源管理器](../../database-engine/configure-windows/open-the-odbc-data-source-administrator.md)  
  
