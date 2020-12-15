---
description: SQL Server 中未安装 SQLXML
title: SQL Server 中未安装 SQLXML
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
ms.assetid: 3dbb4f65-41de-48b8-ad62-47c9d7932de3
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e3c588a22c8a55419fac480ed3e38186c37ee148
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97429577"
---
# <a name="sqlxml-is-not-installed-in-sql-server"></a>SQL Server 中未安装 SQLXML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 之前，SQLXML 4.0 随 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 一起发布，是所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本（[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 除外）默认安装的一部分。 从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 开始，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中不再包括 SQLXML 的最新版本 (SQLXML 4.0 SP1)。 若要安装 SQLXML 4.0 SP1，请从 [sqlxml 4.0 sp1 的安装位置进行](https://www.microsoft.com/download/details.aspx?id=30403)下载。  
  
 如果应用程序在上运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并且需要 SQLXML 4.0，则必须下载并安装 sqlxml 4.0 SP1。  
  
## <a name="sqlxml-40-sp1-behavior-with-new-data-types-using-sqloledb-and-sql-server-native-client-ole-db-provider"></a>在使用 SQLOLEDB 和 SQL Server Native Client OLE DB 访问接口的新数据类型时 SQLXML 4.0 SP1 的行为  
 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 引入了以下数据类型，使用 SQLXML 的开发人员可能需要使用：  
  
-   **日期**  
  
-   **时间**  
  
-   **DateTime2**  
  
-   **DateTimeOffset**  
  
 将 SQLXML 4.0 SP1 与 SQLOLEDB 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB from 一起使用时 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ，这些类型将以字符串的形式显示给开发人员。 SQLXML 4.0 SP1 将这四种新数据类型作为内置标量类型（与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB Provider 11.0 或更高版本一起使用时）启用。 在下载 SQLXML 4.0 SP1 之前，将这些类型映射到非字符串类型可能会导致截断某些数据。 例如，将 **DateTime2** 映射到 **xsd： date** 将导致数据被截断为 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 3.33 毫秒的 **日期时间** 精度。  
  
## <a name="see-also"></a>另请参阅  
 [SQLXML 4.0 编程概念](../../relational-databases/sqlxml/sqlxml-4-0-programming-concepts.md)  
  
  
