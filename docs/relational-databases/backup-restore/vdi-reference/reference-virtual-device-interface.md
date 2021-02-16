---
title: 虚拟设备接口引用
titlesuffix: SQL Server VDI reference
description: 本文概述了 SQL Server 备份的虚拟设备接口引用。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9e4e415ec768344208f7f9c2573ce0ad9a910135
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341753"
---
# <a name="virtual-device-interface-vdi-reference"></a>虚拟设备接口 (VDI) 引用

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

本节包含供第三方备份软件供应商使用的 SQL Server 应用程序编程接口规范。

## <a name="overview"></a>概述

虚拟设备接口 (VDI) 提供最高的联机备份吞吐量和最小的事务工作负载降级，以及最快的恢复时间。 它使第三方供应商能够实现与 SQL Server 本地备份/还原相同的性能特征，并提供全方位的备份/还原功能。 VDI 是在 SQL Server 7.0 中引入的并在更高版本中受支持且得到增强。

VDI 支持两种主要类型的备份技术：

- 常规的联机备份：读取备份集的全部内容并将其传输到备份媒体。

- 快照备份：使用底层拆分镜像或写入时复制技术。

通过 VDI 完成的常规联机备份可以利用 SQL Server 中备份和还原的全部功能。 快照备份仅限于完整数据库和文件/文件组备份。 但是，快照备份可以使用传统的数据库差异、文件差异和事务日志备份进行前滚。

## <a name="next-steps"></a>后续步骤

查看此部分中的 VDI 参考文档。

下载完整规范和支持的源文件：

[SQL Server 2005 虚拟备份设备接口 (VDI) 规范](https://www.microsoft.com/download/details.aspx?id=17282)