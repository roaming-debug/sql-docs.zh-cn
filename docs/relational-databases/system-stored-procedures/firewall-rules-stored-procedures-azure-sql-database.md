---
description: " (Azure SQL 数据库的防火墙规则存储过程) "
title: 防火墙规则存储过程
titleSuffix: Azure SQL Database
ms.date: 07/28/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- firewall rules stored procedures
- firewall_rules, setting
- firewall_rules, Azure SQL Database
- firewall systems, Azure SQL Database
ms.assetid: 3d4c2585-00de-46b5-8eee-0efb71cb3aea
author: VanMSFT
ms.author: vanto
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: bd4507821972afb818d2a68ee4c4e48efcd79891
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165226"
---
# <a name="firewall-rules-stored-procedures-azure-sql-database"></a> (Azure SQL 数据库的防火墙规则存储过程) 
[!INCLUDE [asdb-asa](../../includes/applies-to-version/asdb-asa.md)]

  此部分包含以下用于设置或删除防火墙规则的存储过程。 [!INCLUDE[tsql_md](../../includes/tsql-md.md)] 防火墙规则可以与和一起 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] 使用 [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] 。 有关详细信息，请参阅 [配置 AZURE SQL 数据库防火墙规则-概述](/azure/azure-sql/database/firewall-configure)。

:::row:::
    :::column:::
        [sp_set_firewall_rule（Azure SQL 数据库）](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)
    :::column-end:::
    :::column:::
        [Azure SQL Database &#40;sp_delete_firewall_rule&#41;](../../relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sp_set_database_firewall_rule（Azure SQL 数据库）](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)
    :::column-end:::
    :::column:::
        [Azure SQL Database &#40;sp_delete_database_firewall_rule&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database.md)
    :::column-end:::
:::row-end:::

&nbsp;
  
对于 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，请使用窗口防火墙规则。 有关详细信息，请参阅 [为数据库引擎访问配置 Windows 防火墙](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)。   
