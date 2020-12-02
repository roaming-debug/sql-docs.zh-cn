---
description: Oracle CDC 实例
title: Oracle CDC 实例 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: ed71e8c4-e013-4bf2-8b6c-1e833ff2a41d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 256df16a5ae5a21720d3add261b1fb164c5be7b9
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88496166"
---
# <a name="the-oracle-cdc-instance"></a>Oracle CDC 实例

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Oracle CDC 实例是 Oracle CDC 服务为处理从单个 Oracle 源数据库捕获的更改而创建的进程。 Oracle CDC 实例从 **cdc.xdbcdc_config** 表检索其配置并且在 **cdc.xdbcdc_state** 表中维护其状态。 这些表是用于定义 Oracle CDC 实例的 CDC 数据库的一部分。 有关 xdbcdc 数据库和表的详细信息，请参阅 [The CDC Databases](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_CDCdatabase)。  
  
 下面介绍 Oracle CDC 实例执行的任务：  
  
-   **处理服务启动验证**：在启动时，CDC 实例从 **xdbcdc_config** 表加载其配置并且执行一系列状态验证，这些验证确保 CDC 实例持久化状态是一致的并且可以开始处理更改。  
  
-   **准备变更捕获**：在成功通过验证后，Oracle CDC 实例将扫描当前定义的所有捕获实例，并且准备 Oracle LogMiner 查询以及变更捕获所需的其他支持结构。 此外，Oracle 实例将重新加载上次 Oracle CDC 实例运行时保存的内部捕获状态。  
  
-   **捕获来自 Oracle 的更改**：Oracle CDC 实例将通过 Oracle LogMiner 实用工具将来自 Oracle 的更改存入池中，根据事务提交对更改进行排序，然后更改事务中的时间并且将更改写入 CDC 数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更改表中。  
  
-   **处理服务关闭**：CDC Oracle 实例的生命周期由 Oracle CDC 服务管理。 当 Oracle CDC 实例请求关闭时，它执行以下任务：  
  
    -   停止从 Oracle 事务日志进行的读取。  
  
    -   停止将已完成的 Oracle 事务写入 CDC 数据库。  
  
    -   等待最长 30 秒（如果必要），直到当前事务完成向 CDC 的写入。 如果经过了超过 30 秒的时间，则写入将被取消并且事务将回滚（以便在重新启动 CDC 实例时重试）。  
  
    -   在单独的线程中，在 30 秒的上限内尽可能多地将内存中缓存的记录写入临时事务表（按照从最旧的事务到最新的事务的顺序），然后更新 **xdbcdc_state** 表并提交所有更改。  
  
-   处理配置更改：针对来自 CDC 服务的配置更改或者通过在 **cdc.xdbcdc_config** 表中检测到新版本来通知 Oracle CDC 实例。 大多数更改不需要重新启动 Oracle CDC 实例（例如，添加或删除捕获实例）。 但是，某些更改（例如更改 Oracle 连接字符串和访问凭据）则要求重新启动 CDC 实例。  
  
-   **处理恢复**：在某一 Oracle CDC 实例启动时，其内部状态将从 **xdbcdc_state** 和 **xdbcdc_staged_transactions** 表还原。 一旦状态还原后，CDC 实例将照常运行。  
  
## <a name="see-also"></a>另请参阅  
 [错误处理](../../integration-services/change-data-capture/error-handling.md)  
  
  
