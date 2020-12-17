---
description: 目标服务器（“目标服务器状态”选项卡）
title: 目标服务器（“目标服务器状态”选项卡）
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.target.status.f1
ms.assetid: 010a4cab-d878-4889-8ac8-7d91db6345d6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 5767396a17a222ea0e90658eb7bc97171f0df409
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440408"
---
# <a name="target-servers-target-server-status-tab"></a>目标服务器（“目标服务器状态”选项卡）
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

使用此页可查看此主服务器的目标服务器的状态。  
  
## <a name="options"></a>选项  
**目标服务器**  
查看目标服务器的名称。  
  
**本地时间**  
按照目标服务器所在的时区来查看其日期和时间。  
  
**上次轮询时间**  
查看目标服务器上一次轮询主服务器的本地日期和时间。  
  
**未读指令**  
查看目标服务器尚未收到的指令数。  
  
**Status**  
查看目标服务器的状态。  
  
**强制轮询**  
单击此按钮可强制所选的目标服务器轮询主服务器。  
  
**强制脱离**  
单击此按钮可强制所选的目标服务器脱离主服务器。  
  
**发布指令**  
将指令发布到所选的目标服务器。  
  
**启用自动刷新**  
选择此选项可自动刷新显示的信息。  
  
**刷新间隔**  
指定刷新此页上信息的频率。  
  
## <a name="see-also"></a>另请参阅  
[企业范围的自动化管理](../../ssms/agent/automated-administration-across-an-enterprise.md)  
