---
title: 配置复制快照文件夹（非默认端口）
titleSuffix: SQL Server on Linux
description: 了解如何在 Linux 上配置快照文件夹共享（非默认端口），以实现 SQL Server 复制。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/24/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: f51dd4ebdb3a797a54eb488e8d99c02017dc70e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346480"
---
# <a name="configure-replication-with-non-default-ports-sql-server-linux"></a>使用非默认端口配置复制 (SQL Server Linux)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

可以通过 Linux 上的 SQL Server 实例（会侦听使用 network.tcpport mssql-conf 设置配置的任何端口）来配置复制。 如果满足以下条件，则需要在配置过程中将端口追加到服务器名称后：

1. 复制设置涉及 Linux 上的 SQL Server 实例
2. 任何实例（Windows 或 Linux）都在侦听非默认端口。 

可以通过在实例上运行 @@servername 来查找实例的服务器名称。 请勿使用 IP 地址而不是服务器名称。 为发布服务器、分发服务器或订阅服务器使用 IP 地址可能会导致错误。

> [!NOTE]
> 在使用非默认端口的 Linux 上创建 SQL Server 复制仅适用于 SQL Server 2019 及更高版本。

## <a name="examples"></a>示例

“Server1”侦听 Linux 上的端口 1500。 若要配置“Server1”以进行分发，请搭配 `@distributor` 运行 `sp_adddistributor`。 例如： 

```sql
exec sp_adddistributor @distributor = 'Server1,1500'
```

“Server1”侦听 Linux 上的端口 1500。 若要为分发服务器配置发布服务器，请搭配 `@publisher` 运行 `sp_adddistpublisher`。 例如：

```sql
exec sp_adddistpublisher @publisher = 'Server1,1500' ,  ,  
```

“Server2”侦听 Linux 上的端口 6549。 若要将“Server2”配置为订阅服务器，请搭配 `@subscriber` 运行 `sp_addsubscription`。 例如：

```sql
exec sp_addsubscription @subscriber = 'Server2,6549' ,  ,  
```

“Server3”侦听 Windows 上的端口 6549，其中服务器名称为 Server3，实例名称为 MSSQL2017。 若要将“Server3”配置为订阅服务器，请配合 `@subscriber` 运行 `sp_addsubscription`。 例如：

```sql
exec sp_addsubscription @subscriber = 'Server3/MSSQL2017,6549',  ,  
```

## <a name="next-steps"></a>后续步骤

[概念：Linux 上的 SQL Server 复制](sql-server-linux-replication.md)

[复制存储过程](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)。

