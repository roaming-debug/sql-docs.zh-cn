---
title: " ADR 清理器重试超时（分钟）配置选项 | Microsoft Docs"
description: 介绍 ADR 清理器重试超时的 SQL Server 实例配置设置。
ms.custom: ''
ms.date: 06/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ADR cleaner retry timeout (min)
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 05502bbf760b0f8aa18d81739ce89e19e92397cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347469"
---
# <a name="adr-cleaner-retry-timeout-min-configuration-option"></a>ADR 清理器重试超时（分钟）配置选项

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

在 SQL Server 2019 中引入。

[加速数据库恢复](../../relational-databases/accelerated-database-recovery-concepts.md)需要此配置设置。 清理器是定期唤醒并清除不需要的页面版本的异步过程。

有时在获取对象级别锁时，清理器因其扫描过程中与用户工作负荷发生冲突而遇到问题。 它在单独的列表中跟踪此类页。 ADR 清理器重试超时值（默认值为 15）控制清理器在放弃扫描之前仅重试对象锁获取和清理所需的时间。 若要在中止的事务映射中保持已中止事务的增长，100% 成功地完成扫描是非常重要的。 如果在规定的超时时间内无法清除单独的列表，将放弃当前的扫描，并启动下一次扫描。

## <a name="remarks"></a>备注  

清理器在 SQL Server 2019 中是单线程的，因此一个 SQL Server 实例一次可以处理一个数据库。 如果实例包含多个启用了 ADR 的用户数据库，则不会将超时增加到较大的值，因为在一个数据库上进行重试时，可能会延迟另一个数据库上的清理。

## <a name="examples"></a>示例

下面的示例设置清理器重试超时。

```tsql
sp_configure 'show advanced options', 1;  
RECONFIGURE;
GO 
sp_configure 'ADR cleaner retry timeout', 15;  
RECONFIGURE;  
GO  
```  

## <a name="see-also"></a>另请参阅  

- [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [加速数据库恢复](../../relational-databases/accelerated-database-recovery-concepts.md)
- [管理加速数据库恢复](../../relational-databases/accelerated-database-recovery-management.md)