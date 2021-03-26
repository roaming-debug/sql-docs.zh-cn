---
description: 了解如何管理事务大小，以确保不会在你的应用程序中引入阻止其他用户的锁定。
title: 管理事务大小
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 82900342-bc80-445f-98a4-468a303aae1e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2289e35116fc9ae39210db1b2af7dcb6e7dfad7a
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793037"
---
# <a name="managing-transaction-size"></a>管理事务大小

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

执行事务时尽可能使事务保持简短，这一点很重要。 默认模式为自动提交（可以使用 [setAutoCommit](reference/setautocommit-method-sqlserverconnection.md) 方法启用或禁用此模式），它将提交每个操作。 这对于大多数开发人员来说是最易于使用的模式。

当您使用手动事务时，确保您的代码尽可能快地提交事务。 让事务保持打开状态会阻止其他用户访问数据。 例如，在 catch 块中放入一个回滚调用和在 `finally` 块中放入一个提交调用，这可能是一种良好的编程做法。 不过，此做法要视你的应用程序设计而定。

减小事务大小可提高并发性。 例如，如果你启动手动事务并在一个拥有 20,000 行的表中修改 10,000 行，则所有其他用户都将无法访问此表中一半的行数据，即使他们只是读取数据也不例外。 将您的修改量降至 2,000 行后，则其他用户可以访问表中 90% 的行数据。

此外，如果预计应用程序会遇到阻止问题，则务必使用锁定超时设置。 可以使用 [setLockTimeout](reference/setlocktimeout-method-sqlserverdatasource.md) 方法设置超时。 锁定超时的默认值为 -1，这意味着它将无限期阻止，同时等待锁定。 可以将锁定超时设置为 30 秒，这样，如果其他连接进行了阻止，则被阻止的连接会在 30 秒后超时。

## <a name="see-also"></a>另请参阅

[通过 JDBC 驱动程序提升性能和可靠性](improving-performance-and-reliability-with-the-jdbc-driver.md)
