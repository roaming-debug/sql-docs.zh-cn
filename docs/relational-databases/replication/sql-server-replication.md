---
title: SQL Server 复制 | Microsoft Docs
description: 了解 SQL Server 中的复制，它们是在数据库之间复制和分发数据和数据库对象以及在数据库之间进行同步的技术。
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], about
- replication [SQL Server]
ms.assetid: 3a5f4592-3c61-4b4d-9ceb-39716aeeba41
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 898053b0114eb398c5b9650fa162779edb6af818
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479658"
---
# <a name="sql-server-replication"></a>SQL Server 复制
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  复制是一组技术，它将数据和数据库对象从一个数据库复制和分发到另一个数据库，然后在数据库之间进行同步以保持一致性。 使用复制，可以通过局域网和广域网、拨号连接、无线连接和 Internet 将数据分配到不同位置以及分配给远程或移动用户。  
  
 事务复制通常用于需要高吞吐量的服务器到服务器方案（包括：提高可伸缩性和可用性、数据仓库和报告、集成多个站点的数据、集成异类数据以及减轻批处理的负荷）。 合并复制主要是为可能存在数据冲突的移动应用程序或分步式服务器应用程序设计的。 常见应用场景包括：与移动用户交换数据、POS（消费者销售点）应用程序以及集成来自多个站点的数据。 快照复制用于为事务复制和合并复制提供初始数据集；在适合数据完全刷新时也可以使用快照复制。 利用这三种复制， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供功能强大且灵活的系统，以便使企业范围的数据同步。 [!INCLUDE[win8srv](../../includes/win8srv-md.md)] 和 [!INCLUDE[win8](../../includes/win8-md.md)]都支持复制到 SQLCE 3.5 和 SQLCE 4.0。  


## <a name="whats-new"></a>新增功能 
- SQL Server 2017 未向 SQL Server 复制引入重要的新功能。 
- SQL Server 2016 未向 SQL Server 复制引入重要的新功能。 

有关后向兼容性详细信息，请参阅[复制后向兼容性](replication-backward-compatibility.md) 


 ## <a name="replication-security"></a>复制安全性
  
-   [查看和修改复制安全设置](security/view-and-modify-replication-security-settings.md)  
-   [管理发布访问列表中的登录名](security/manage-logins-in-the-publication-access-list.md)  
  
## <a name="publishing-and-distribution"></a>发布和分发  
  
-   [配置发布和分发](configure-publishing-and-distribution.md)   
-   [查看和修改发布属性](publish/view-and-modify-publication-properties.md)   
-   [禁用发布和分发](disable-publishing-and-distribution.md)  
  
## <a name="publications-and-articles"></a>发布和项目 
  
-   [创建发布](publish/create-a-publication.md)    
-   [定义项目](publish/define-an-article.md)   
-   [查看和修改发布属性](publish/view-and-modify-publication-properties.md)   
-   [查看和修改项目属性](publish/view-and-modify-article-properties.md)    
-   [删除发布](publish/delete-a-publication.md)   
-   [删除项目](publish/delete-an-article.md)    
-   [从 Oracle 数据库创建发布](publish/create-a-publication-from-an-oracle-database.md)   
-   [设置订阅的过期期限](publish/set-the-expiration-period-for-subscriptions.md)  
-   [指定架构选项](publish/specify-schema-options.md)  
-   [复制架构更改](publish/replicate-schema-changes.md)    
-   [管理标识列](publish/manage-identity-columns.md)   
-   [设置合并发布的兼容级别](publish/set-the-compatibility-level-for-merge-publications.md)  
  
### <a name="snapshot-options"></a>快照选项  
  
-   [配置快照属性](publish/configure-snapshot-properties-replication-transact-sql-programming.md)    
-   [通过 FTP 传递快照](publish/deliver-a-snapshot-through-ftp.md) 
  
### <a name="filter-data"></a>筛选数据  
  
-   [定义和修改列筛选器](publish/define-and-modify-a-column-filter.md)    
-   [定义和修改静态行筛选器](publish/define-and-modify-a-static-row-filter.md)    
-   [定义和修改合并项目的参数化行筛选器](publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)    
-   [优化参数化行筛选器](publish/optimize-parameterized-row-filters.md)    
-   [定义和修改合并项目间的联接筛选器](publish/define-and-modify-a-join-filter-between-merge-articles.md)  
  
### <a name="transactional-replication-options"></a>事务复制选项  
  
-   [为事务项目的数据更改设置传播方法](publish/set-the-propagation-method-for-data-changes-to-transactional-articles.md)    
-   [允许更新事务发布的订阅](publish/enable-updating-subscriptions-for-transactional-publications.md)  
  
### <a name="merge-replication-options"></a>合并复制选项  
  
-   [定义合并表项目间的逻辑记录关系](publish/define-a-logical-record-relationship-between-merge-table-articles.md)    
-   [指定合并复制属性](merge/specify-merge-replication-properties.md)    
-   [指定合并项目冲突解决程序](publish/specify-a-merge-article-resolver.md)    

  
## <a name="manage-subscriptions"></a>管理订阅  
  
-   [创建请求订阅](create-a-pull-subscription.md)    
-   [查看和修改请求订阅属性](view-and-modify-pull-subscription-properties.md)    
-   [删除请求订阅](delete-a-pull-subscription.md)    
-   [创建推送订阅](create-a-push-subscription.md)   
-   [查看和修改推送订阅属性](view-and-modify-push-subscription-properties.md)   
-   [删除推送订阅](delete-a-push-subscription.md)   
-   [指定同步计划](specify-synchronization-schedules.md)    
-   [创建事务发布的可更新订阅](publish/create-an-updatable-subscription-to-a-transactional-publication.md)  
-   [为非 SQL Server 订阅服务器创建订阅](create-a-subscription-for-a-non-sql-server-subscriber.md)  
  
## <a name="synchronize-subscriptions"></a>同步订阅  
  
-   [创建并应用初始快照](create-and-apply-the-initial-snapshot.md)   
-   [为包含参数化筛选器的合并发布创建快照](create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)    
-   [从备份初始化事务订阅（复制 Transact-SQL 编程）](initialize-a-transactional-subscription-from-a-backup.md)    
-   [手动初始化订阅](initialize-a-subscription-manually.md)    
-   [同步请求订阅](synchronize-a-pull-subscription.md)    
-   [同步推送订阅](synchronize-a-push-subscription.md)   
-   [重新初始化订阅](reinitialize-a-subscription.md)    
-   [在同步期间执行脚本（复制 Transact-SQL 编程）](execute-scripts-during-synchronization-replication-transact-sql-programming.md)    
-   [为合并项目实现业务逻辑处理程序](implement-a-business-logic-handler-for-a-merge-article.md)  
-   [调试业务逻辑处理程序（复制编程）](debug-a-business-logic-handler-replication-programming.md)    
-   [控制同步期间触发器和约束的行为（复制 Transact-SQL 编程）](control-behavior-of-triggers-and-constraints-in-synchronization.md)    
-   [为合并项目实现自定义冲突解决程序](implement-a-custom-conflict-resolver-for-a-merge-article.md)  
  
## <a name="administration"></a>管理 
  
-   [处理复制代理配置文件](agents/work-with-replication-agent-profiles.md)   
-   [在订阅服务器上验证数据](validate-data-at-the-subscriber.md)    
-   [通过参数化筛选器为合并发布管理分区](publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)    
-   [将数据批量加载到合并发布中的表（复制 Transact-SQL 编程）](bulk-load-data-into-tables-in-a-merge-publication.md)    
-   [清除合并元数据（复制 Transact-SQL 编程）](administration/clean-up-merge-metadata-replication-transact-sql-programming.md)    
-   [执行合并项目的虚更新（复制 Transact-SQL 编程）](administration/perform-a-dummy-update-for-a-merge-article-replication-transact-sql-programming.md)    
-   [查看分发数据库中复制的命令和其他信息（复制 Transact-SQL 编程）](monitor/view-replicated-commands-and-information-in-distribution-database.md)    
-   [为事务复制启用协调备份（复制 Transact-SQL 编程）](administration/enable-coordinated-backups-for-transactional-replication.md)   
-   [管理对等拓扑（复制 Transact-SQL 编程）](administration/administer-a-peer-to-peer-topology-replication-transact-sql-programming.md)    
-   [停止复制拓扑（复制 Transact-SQL 编程）](administration/quiesce-a-replication-topology-replication-transact-sql-programming.md)    
-   [为 Oracle 发布服务器配置事务集作业（复制 Transact-SQL 编程）](administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
-   [升级复制脚本（复制 Transact-SQL 编程）](administration/upgrade-replication-scripts-replication-transact-sql-programming.md)  
  
## <a name="monitor"></a>监视
  
-   [允许非管理员使用复制监视器](monitor/allow-non-administrators-to-use-replication-monitor.md)    
-   [以编程方式监视复制](monitor/programmatically-monitor-replication.md)    
-   [查看分发数据库中复制的命令和其他信息（复制 Transact-SQL 编程）](monitor/view-replicated-commands-and-information-in-distribution-database.md)    
-   [查看合并发布的冲突信息（复制 Transact-SQL 编程）](./view-and-resolve-data-conflicts-for-merge-publications.md) 
-   [为事务复制测量滞后时间和验证连接](monitor/measure-latency-and-validate-connections-for-transactional-replication.md)  
  
