---
description: 在企业中优化自动化管理
title: 在企业中优化自动化管理
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- performance [SQL Server], automated administration
- tuning automated administration [SQL Server]
- monitoring performance [SQL Server], automated administration
ms.assetid: 671fed35-3859-4b0d-8f38-4dd07436857e
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 37f5647c83c7355566561b5d65ecb67f7d668b7d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97422712"
---
# <a name="tune-automated-administration-across-an-enterprise"></a>在企业中优化自动化管理

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

使用 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的多服务器管理利用了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的自我优化功能。 因此，在正常情况下，不需要额外进行作业优化。 但当您运行作业、生成警报和通知操作员时，网络负荷会相应增加。 您可以优化企业中的自动化管理以尽量减少这些活动导致的网络通信流量。  

## <a name="see-also"></a>另请参阅

[监视数据流引擎的性能](../../integration-services/performance/performance-counters.md)