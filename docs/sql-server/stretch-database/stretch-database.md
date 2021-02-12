---
description: Stretch Database
title: Stretch Database
ms.date: 06/27/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database
ms.assetid: ce6db775-21a5-40bc-95a1-f560376d4ee2
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: c2d2acc7ba0ac0f57038b3a72f62e6b67383b1bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077388"
---
# <a name="stretch-database"></a>Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Stretch Database 可以既透明又安全地将冷数据迁移到 Microsoft Azure 云。  
  
 如果你想立即开始使用 Stretch Database，请参阅 [通过运行“启用数据库延伸向导”开始](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md)。  
  
## <a name="what-are-the-benefits-of-stretch-database"></a>Stretch Database 的优点是什么？  
 延伸数据库提供以下优势：  
  
 **可以通过经济高效的方式来使用冷数据**  
 使用 SQL Server Stretch Database 将冷暖事务数据从 SQL Server 动态延伸到 Microsoft Azure。 与典型的冷数据存储不同，数据将始终联机且可供查询。 可以提供更长的数据保留时限，而不会破坏大型表（如客户订单历史记录）的存储库。 受益于 Azure 的低成本，无需扩展价格不菲的本地存储。 你可以在 Azure 门户中选择定价层并配置设置，因此始终能够对价格和成本进行控制。 根据需要扩展或缩减。 有关详细信息，请访问 [SQL Server Stretch Database 定价](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) 。  
  
 **无需更改查询或应用程序**  
 无缝访问 SQL Server 数据，不管这些数据是位于本地还是延伸到云中。  可以设置策略来确定数据的存储位置，SQL Server 会在后台处理数据移动。 整个表始终处于联机状态，始终可供查询。 而且，Stretch Database 无需对现有查询或应用程序进行任何更改，数据的位置对应用程序来说是完全透明的。  
  
 **简化了本地数据维护过程**  
 降低了数据的本地维护和存储成本 对本地数据的备份运行速度更快，并在维护时段内完成。 自动运行对数据的云部分的备份。 你对本地存储的需求大大减少。 与在本地 SSD 中添加数据相比，使用 Azure 存储可将费用较低 80%。  
  
 **即使在迁移过程中，也会确保数据的安全性**  
 可以高枕无忧地将最重要的应用程序安全延伸到云中。 SQL Server 的 Always Encrypted 功能为数据提供动态加密。 行级别安全性 (RLS) 和其他高级 SQL Server 安全功能也能配合延伸数据库来保护数据。  
  
## <a name="what-does-stretch-database-do"></a>延伸数据库的作用是什么？  
 为 SQL Server 实例、数据库启用 Stretch Database 并且选择至少一个表以后，Stretch Database 就会开始以静默方式将你的冷数据迁移到 Azure。  
  
-   如果在单独的某个表中存储了冷数据，可以迁移整个表。  
  
-   如果表同时包含热数据和冷数据，可以指定筛选器函数来选择要迁移的行。

**不需更改现有查询和客户端应用。** 即使在数据迁移过程中，也可以持续顺畅访问本地数据和远程数据。 远程查询会出现轻微的延迟，但仅查询冷数据时，才会遇到这种延迟。

**Stretch Database 可以确保迁移过程中无数据丢失** （即使发生故障）。 它还可以通过重试逻辑来处理迁移过程中可能会发生的连接问题。 动态管理视图提供迁移的状态。

**可以暂停数据迁移** 以排查本地服务器上的问题或者最大程度地提供可用网络带宽。  
  
 ![Stretch Database 概述](../../sql-server/stretch-database/media/stretch-overview.png "Stretch Database 概述")  
  
## <a name="is-stretch-database-for-you"></a>Stretch Database 是否适合你？  
 如果与下表中的表述相符，则延伸数据库可以帮助你满足要求和解决问题。  
  
|如果是决策人|如果是数据库管理员|  
|--------------------------------|---------------------|  
|我必须长期保留事务数据。|我的表大小正在失控。|  
|有时候，我必须查询冷数据。|我的用户说他们想要访问冷数据，但只是偶尔使用。|  
|我安装了应用（包括较旧的应用），我不想对其进行更新。|我必须不断地购买和添加更多的存储。|  
|我想找到一种节省存储费用的方法。|我不能在 SLA 条件下备份或还原此类大型表。|  
  
## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>哪种类型的数据库和表符合延伸数据库条件？  
 Stretch Database 针对的是包含大量冷数据的事务数据库，这些冷数据通常存储在少量表中。 这些表可能包含 10 亿多行。  
  
 如果你使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的临时表功能，则可使用 Stretch Database 将所有或部分关联的历史表迁移到 Azure 经济高效的存储中。 有关详细信息，请参阅 [管理版本由系统控制的临时表中历史数据的保留期](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)。  
  
 使用 SQL Server 2016 升级顾问的一项功能 - 延伸数据库顾问 - 可以识别符合延伸数据库条件的数据库和表。 有关详细信息，请参阅 [通过运行 Stretch Database 顾问标识适用于 Stretch Database 的数据库和表](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md)。 若要了解有关潜在阻滞问题的详细信息，请参阅 [Stretch Database 的局限性](../../sql-server/stretch-database/limitations-for-stretch-database.md)。  

## <a name="test-drive-stretch-database"></a>试用 Stretch Database  
 **通过 AdventureWorks 示例数据库试用 Stretch Database。** 若要获取 AdventureWorks 示例数据库，必须从 [此处](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks)。 将示例数据库还原到 SQL Server 2016 实例后，解压缩示例文件，然后从 Stretch DB 文件夹打开 Stretch DB Samples 文件。 运行此文件中的脚本来检查启用 Stretch Database 之前和之后数据使用的空间、跟踪数据迁移的进度，以及确认你是否可以继续在数据迁移期间和之后查询现有数据和插入新数据。  
  
## <a name="next-step"></a>下一步  
 **确定适用于 Stretch Database 的数据库和表。** 下载数据迁移助手并运行评估，以标识为 Stretch Database 候选项的数据库和表。 有关详细信息，请参阅 [通过运行 Stretch Database 顾问标识适用于 Stretch Database 的数据库和表](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md)。  
  
  
