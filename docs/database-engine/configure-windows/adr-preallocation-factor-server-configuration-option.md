---
title: ADR 预分配因素配置选项 | Microsoft Docs
description: 介绍 ADR 预分配因素的 SQL Server 实例配置设置。
ms.custom: ''
ms.date: 06/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ADR Preallocation Factor
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 9469b5de278c01b7b256ee348380ebd1ae19cde4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347463"
---
# <a name="adr-preallocation-factor-configuration-option"></a>ADR 预分配因素配置选项

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

在 SQL Server 2019 中引入。

[加速数据库恢复](../../relational-databases/accelerated-database-recovery-concepts.md)需要此配置设置。

加速数据库恢复 (ADR) 会保留数据的多个版本以便恢复。 这些版本是在各种数据操作语言 (DML) 操作中生成的。 这些版本存储在名为永久版本存储 (PVS) 的内部表中。 

## <a name="remarks"></a>备注  

如果在前台用户 DML 操作中将页面分配给 PVS 表，性能可能会降低。 为解决此问题，有一个后台会话会预分配页面并使其随时可用于 DML 事务。 当后台会话预分配足够多的页面，前台 PVS 分配的百分比接近 0 时，性能最佳。 如果百分比提高并影响性能，错误日志会包含带有 `PreallocatePVS` 标记的条目。

后台预分配的页数基于各种工作负载试探法，但在很大程度上是以 512 页的块为单位分配页面。 ADR 预分配系数是块的倍数。 默认情况下，该系数为 4。 这意味着它会在需要时一次性预分配 2048 个页面。 

尽管后台会话会考虑工作负载模式，但如果需要，可以增大此系数以提高性能。

> [!CAUTION]
> 如果 PVS 预分配增加过多，它将与系统中的其他分配争用，实际可能降低总体性能
>
> 在修改此设置之前，请测试系统的整体性能。

## <a name="examples"></a>示例  

下面的示例将预分配系数设置为 4。

```tsql
sp_configure 'show advanced options', 1;
RECONFIGURE;
GO 
sp_configure 'ADR Preallocation Factor', 4;
RECONFIGURE;
GO
```

## <a name="see-also"></a>另请参阅  

- [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [加速数据库恢复](../../relational-databases/accelerated-database-recovery-concepts.md)
- [管理加速数据库恢复](../../relational-databases/accelerated-database-recovery-management.md)