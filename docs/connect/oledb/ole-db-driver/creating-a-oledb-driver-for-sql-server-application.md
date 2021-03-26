---
title: 创建适用于 SQL Server 的 OLE DB 驱动程序应用程序
description: 了解创建 OLE DB Driver for SQL Server 应用程序以及查找其他资源所需的步骤。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, application creation
- applications [OLE DB Driver for SQL Server]
- OLE DB, creating applications
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fdf209a8bdb34dc5f22a60ee190a118d8a608995
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793089"
---
# <a name="creating-an-ole-db-driver-for-sql-server-application"></a>创建适用于 SQL Server 的 OLE DB 驱动程序应用程序

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  创建 OLE DB Driver for SQL Server 应用程序涉及以下步骤：

1. 建立与数据源的连接。
2. 执行命令。
3. 结果处理。

> [!NOTE]
> 请尽可能使用 Windows 身份验证。 如果 Windows 身份验证不可用，请在运行时提示用户输入其凭据。 不要将凭据存储在一个文件中。 如果必须保存凭据，应当用 [Win32 cryptoAPI](/windows/win32/seccng/cng-portal) 对它们加密。

## <a name="in-this-section"></a>本节内容

- [建立与数据源的连接](establishing-a-connection-to-a-data-source.md)
- [执行命令](executing-a-command.md)
- [处理结果](processing-results.md)
- [关于 OLE DB 属性](about-ole-db-properties.md)
- [将 OUTPUT 子句与适用于 SQL Server 的 OLE DB 驱动程序中的 OLE DB 结合使用](using-the-output-clause-with-ole-db-in-oledb-driver-for-sql-server.md)

## <a name="see-also"></a>另请参阅

[适用于 SQL Server 的 OLE DB 驱动程序编程](../ole-db/oledb-driver-for-sql-server-programming.md)
