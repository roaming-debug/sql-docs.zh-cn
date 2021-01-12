---
description: 变更数据捕获函数 (Transact-SQL)
title: " (Transact-sql) 更改数据捕获函数 |Microsoft Docs"
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], functions
ms.assetid: e5270557-aca3-44ab-8715-daccd498b88d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: de8544cec3cb1dd3ee5e3c78eff73c4bfe3b0fd3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098770"
---
# <a name="change-data-capture-functions-transact-sql"></a>变更数据捕获函数 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  变更数据捕获用于记录应用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表的插入、更新和删除操作，同时采用易于使用的关系格式提供有关更改的详细信息。 此操作将为已修改的行捕获反映了所跟踪源表列结构的列信息，同时还捕获将更改应用到目标环境所需的元数据。 以下函数用于返回有关更改的信息。   

:::row:::
    :::column:::
        [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.fn_cdc_has_column_changed &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-has-column-changed-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.fn_cdc_increment_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-increment-lsn-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.fn_cdc_decrement_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-decrement-lsn-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.fn_cdc_is_bit_set &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-is-bit-set-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.fn_cdc_get_column_ordinal &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-get-column-ordinal-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.fn_cdc_map_lsn_to_time &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-lsn-to-time-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.fn_cdc_get_max_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-get-max-lsn-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.fn_cdc_map_time_to_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.fn_cdc_get_min_lsn &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-get-min-lsn-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
  
## <a name="see-also"></a>另请参阅  
 [变更数据捕获表 (Transact-SQL)](../../relational-databases/system-tables/change-data-capture-tables-transact-sql.md)   
 [更改数据捕获存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/change-data-capture-stored-procedures-transact-sql.md)   
 [关于变更数据捕获 (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
