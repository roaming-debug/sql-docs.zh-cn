---
title: 使用测试服务器的注意事项 | Microsoft Docs
description: 了解如何使用测试服务器优化生产服务器中的数据库。 这是数据库引擎优化顾问的重要优点。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- overhead [Database Engine Tuning Advisor]
- tuning overhead [SQL Server]
- reducing production server tuning load
- Database Engine Tuning Advisor [SQL Server], test servers
- xp_msver
- test servers [Database Engine Tuning Advisor]
- production servers [SQL Server]
- offload tuning overhead [SQL Server]
ms.assetid: 94e6c3e5-1f09-4616-9da2-4e44d066d494
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a286b61d3efa53aa862b5c69fbd3782d6c59cdd3
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505331"
---
# <a name="considerations-for-using-test-servers"></a>使用测试服务器的注意事项
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用测试服务器优化生产服务器中的数据库是 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问的显著优势。 使用此功能，可以将优化开销转移到测试服务器，而无需将实际数据从生产服务器复制到测试服务器。  
  
> [!NOTE]  
>  [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问图形用户界面 (GUI) 不支持测试服务器优化功能。  
  
 若要成功使用此功能，请查看下列部分中列出的注意事项。  
  
## <a name="setting-up-the-test-serverproduction-server-environment"></a>设置测试服务器/生产服务器环境  
  
-   要使用测试服务器优化生产服务器上的数据库的用户必须同时存在于两个服务器上，否则此方案无效。  
  
-   必须启用扩展存储过程 **xp_msver**，才能使用测试服务器/生产服务器方案。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问使用此扩展存储过程来提取在优化测试服务器时要使用的处理器数和生产服务器的可用内存。 如果未启用 **xp_msver** ，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问假定运行 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问的计算机具有硬件特征。 如果运行 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问的计算机没有硬件特征，则假定具有一个处理器和 1024 MB 的内存。 在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]时，该扩展存储过程默认是打开的。 有关详细信息，请参阅 [外围应用配置器](../../relational-databases/security/surface-area-configuration.md) 和 [xp_msver (Transact-SQL)](../../relational-databases/system-stored-procedures/xp-msver-transact-sql.md)。  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问需要测试服务器和生产服务器中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为同一版本。 如果有两个不同的版本，则优先使用测试服务器中的版本。 例如，如果测试服务器运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标准版，则即使生产服务器运行 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 企业版， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 优化顾问也不会建议使用索引视图、分区和联机操作。  
  
## <a name="about-test-serverproduction-server-behavior"></a>关于测试服务器/生产服务器行为  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问将考虑生产服务器与测试服务器之间的硬件差异。 所得到的建议与在生产服务器上进行优化时相同。  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问可能会在生产服务器中施加一些负载，以收集元数据并创建优化所需的统计信息。  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问不会将实际数据从生产服务器复制到测试服务器中。 仅复制数据库的元数据和必要的统计信息。  
  
-   所有会话信息都存储在生产服务器上的 **msdb** 中。 这使用户可以利用任何可用的测试服务器进行优化，并且有关所有会话的信息都可以从一个地方（生产服务器）获得。  
  
## <a name="issues-related-to-the-shell-database"></a>与 Shell 数据库有关的问题  
  
-   执行优化后， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问应删除优化期间在测试服务器中创建的所有元数据。 包括 shell 数据库。 如果您要使用相同的生产服务器和测试服务器执行一系列优化会话，那么您可能希望保留此 shell 数据库以节省时间。 请在 XML 输入文件中指定 **TuningOptions** 父元素下的 **RetainShellDB** 子元素以及其他子元素。 使用这些选项将使 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问保留 Shell 数据库。 有关详细信息，请参阅[ XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)。  
  
-   在成功完成测试服务器/生产服务器的优化会话后，即使未指定 **RetainShellDB** 子元素，Shell 数据库也会保留在测试服务器中。 这些无用的 Shell 数据库可能会妨碍后续的会话优化操作，因此，在执行其他测试服务器/生产服务器优化会话之前，应先删除这些 Shell 数据库。 此外，如果优化会话意外结束，则测试服务器中的 Shell 数据库及其内部对象可能会保留在测试服务器中。 在启动新的测试服务器/生产服务器优化会话之前，也应删除这些数据库和对象。  
  
## <a name="issues-related-to-the-tuning-process"></a>与优化过程有关的问题  
  
-   用户必须在优化日志中检查由生产服务器和测试服务器之间的差异导致的优化错误，以及由于将元数据从生产服务器复制到测试服务器而导致的错误。 例如，用户登录名可能在测试服务器上不存在。 如果用户登录名在测试服务器上不存在，则可能无法优化工作负荷中由该用户登录名执行的那些事件。 使用 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问 GUI 可以查看优化日志。 有关详细信息，请参阅 [查看和使用数据库引擎优化顾问的输出](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)  
  
-   如果因 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问在测试服务器中创建的 Shell 数据库中的对象丢失而导致 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问无法优化多个事件，则用户必须检查优化日志。 无法优化的事件列在日志中。 若要成功优化测试服务器上的数据库，用户必须创建 shell 数据库中所缺少的对象，然后启动新的优化会话。  
  
-   如果测试服务器中已存在同名数据库，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问不会复制元数据，但会根据需要继续执行优化并收集统计信息。 如果在调用 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问之前，用户已在测试服务器中创建了一个数据库并复制了适当的元数据，则此功能将十分有用。  
  
-   如果为生产服务器上的数据库启用了 DATE_CORRELATION_OPTIMIZATION 选项，则在优化测试服务器时，元数据和与此选项关联的数据不会完全编入脚本。 采用测试服务器/生产服务器方案进行优化时，可能会出现下列问题：  
  
    -   对于服务器上使用 DATE_CORRELATION_OPTIMIZATION 选项的各个查询，用户可以执行不同的查询计划。  
  
    -   [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问可能会建议删除执行建议脚本中 DATE_CORRELATION_OPTIMIZATION 选项的索引视图。  
  
     因此，您可能希望忽略 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问针对保留相关统计信息的索引视图所提出的任何建议，因为 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问只了解其开销，而不了解其优势。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问可能不建议你选择某些索引（例如 **datetime** 列的聚集索引），而这些索引在启用 DATE_CORRELATION_OPTIMIZATION 时可能会非常有用。  
  
     若要确定视图是否基于关联统计信息，请选择 **sys.views** 目录视图的 [is_date_correlation_view](../../relational-databases/system-catalog-views/sys-views-transact-sql.md) 列。  
  
  
