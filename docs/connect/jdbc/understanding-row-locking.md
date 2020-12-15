---
description: 了解行锁
title: 了解行锁 | Microsoft Docs
ms.custom: ''
ms.date: 12/08/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 63c76a2f-f2b9-461f-8904-acbda0169ac3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e5305f3feaa80d0a83dd1e7bfd97088492608ae5
ms.sourcegitcommit: 7f76975c29d948a9a3b51abce564b9c73d05dcf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96901055"
---
# <a name="understanding-row-locking"></a>了解行锁

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 行锁。 这样，就可以在同时在数据库中执行修改的多个用户之间实施并发控制。 默认情况下，事务和锁是在每个连接的基础上进行管理的。 例如，如果应用程序打开两个 JDBC 连接，则一个连接获得的锁不能与另一个连接共享。 一个连接所获得的锁不能与另一个连接所持有的锁相冲突。

> [!NOTE]  
> 如果使用行锁定，则将锁定提取缓冲区中的所有行，这样，如果提取大小的设置非常大，则可能影响并发性能。

锁定用于确保事务完整性和数据库一致性。 锁定可以防止用户读取其他用户正在更改的数据，并防止多个用户同时更改相同的数据。 如果不使用锁定，数据库中的数据可能在逻辑上变得不正确，而针对这些数据进行查询可能会产生想不到的结果。

> [!NOTE]  
> 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的行锁的详细信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的[“[!INCLUDE[ssDE](../../includes/ssde_md.md)] 中的锁定”](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine)。

## <a name="see-also"></a>另请参阅

[通过 JDBC 驱动程序管理结果集](../../connect/jdbc/managing-result-sets-with-the-jdbc-driver.md)
