---
title: xp_cmdshell 服务器配置选项
description: 了解“xp_cmdshell”选项。 了解它如何控制 SQL Server 是否可以运行“xp_cmdshell”扩展存储过程。 了解如何启用它。
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- xp_cmdshell
ms.assetid: c147c9e1-b81d-49c8-b800-3019f4d86a13
author: markingmyname
ms.author: maghan
ms.custom: contperfq4
ms.date: 06/12/2020
ms.openlocfilehash: 004a7b0a50a657632bb2b9970f0558857d416494
ms.sourcegitcommit: ae474d21db4f724523e419622ce79f611e956a22
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92257973"
---
# <a name="xp_cmdshell-server-configuration-option"></a>xp_cmdshell 服务器配置选项

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本文介绍如何启用 xp_cmdshell SQL Server 配置选项。 此选项使系统管理员能够控制是否可以在系统上执行 [xp_cmdshell 扩展存储过程](../../relational-databases/system-stored-procedures/xp-cmdshell-transact-sql.md)。 新安装中默认禁用 xp_cmdshell 选项。

启用此选项前，请务必考虑潜在的安全隐患。

- 新开发的代码不应使用 xp_cmdshell 存储过程，通常应将它保留为禁用状态。
- 某些旧版应用程序要求启用 xp_cmdshell。 如果无法将这些应用程序修改为避免使用此存储过程，则可以按下面所述启用它。

> [!NOTE]  
> 如果必须使用 xp_cmdshell，最佳安全做法建议只在需要它的实际任务的持续时间内启用。

如果需要启用 xp_cmdshell，可以使用[基于策略的管理](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)或运行 sp_configure 系统存储过程，如以下代码示例所示：  
  
``` sql
-- To allow advanced options to be changed.  
EXECUTE sp_configure 'show advanced options', 1;  
GO  
-- To update the currently configured value for advanced options.  
RECONFIGURE;  
GO  
-- To enable the feature.  
EXECUTE sp_configure 'xp_cmdshell', 1;  
GO  
-- To update the currently configured value for this feature.  
RECONFIGURE;  
GO  
```  
  
## <a name="next-steps"></a>后续步骤

- [xp_cmdshell 扩展存储过程](../../relational-databases/system-stored-procedures/xp-cmdshell-transact-sql.md)
- [服务器配置选项 (SQL Server)](server-configuration-options-sql-server.md)
- [使用基于策略的管理来管理服务器](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)  
