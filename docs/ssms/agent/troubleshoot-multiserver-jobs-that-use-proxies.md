---
description: 排除使用代理的多服务器作业的故障
title: 排除使用代理的多服务器作业的故障
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- proxies [SQL Server Agent], multiserver jobs
- jobs [SQL Server Agent], multiserver jobs using proxies
ms.assetid: fc579bd3-010c-4f72-8b5c-d0cc18a1f280
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 620069c07ebd5f355f93b486a0430182d80ea613
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466368"
---
# <a name="troubleshoot-multiserver-jobs-that-use-proxies"></a>排除使用代理的多服务器作业的故障
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

如果分布式作业的步骤与某个代理关联，则该作业将在目标服务器上该代理帐户的上下文中运行。 如果从主服务器下载使用代理帐户的作业步骤时失败，则请检查 **msdb** 数据库中 **sysdownloadlist** 表的 **error_message** 列是否存在下列错误消息：  
  
-   “该作业步骤需要代理帐户，但是目标服务器上禁用了代理匹配功能。”  
  
    若要解决此错误，请将 \HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL. _\<n\>_ \SQLServerAgent\AllowDownloadedJobsToMatchProxyName 注册表子项设置为“1 (true)”  。 默认情况下，此子项设置为“0 (False)”   。 MSSQL.\<*n*> 的值 是实例名；例如 MSSQL.1 或 MSSQL.3 。  
  
-   “找不到代理。”  
  
    若要解决此错误，请确保目标服务器上已存在与运行作业步骤的主服务器代理帐户同名的代理帐户。  
  
> [!CAUTION]  
> [!INCLUDE[ssNoteRegistry](../../includes/ssnoteregistry-md.md)]  
  
## <a name="see-also"></a>另请参阅  
[创建多服务器环境](../../ssms/agent/create-a-multiserver-environment.md)  
