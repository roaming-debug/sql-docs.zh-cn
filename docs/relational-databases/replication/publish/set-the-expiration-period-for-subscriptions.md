---
description: 设置订阅的过期期限
title: 设置订阅的过期期限 | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [SQL Server replication], expiration
- expiration [SQL Server replication]
- retention periods [SQL Server replication]
- deactivating subscriptions
ms.assetid: 542f0613-5817-42d0-b841-fb2c94010665
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 677d469c12dd2f72988d469b2fc5541ad0900996
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468918"
---
# <a name="set-the-expiration-period-for-subscriptions"></a>设置订阅的过期期限
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中设置订阅的过期期限。 订阅的过期期限决定了订阅在经过多长时间后过期并被删除。 有关详细信息，请参阅 [Subscription Expiration and Deactivation](../../../relational-databases/replication/subscription-expiration-and-deactivation.md)。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [建议](#Recommendations)  
  
-   **设置订阅的过期期限，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建议  
  
-   订阅过期期限也称为“发布保持期” 。 合并复制元数据的清除依赖于此设置：  
  
    -   在到达保持期之前，复制无法清除发布数据库和订阅数据库中的元数据。 在为保持期指定较大值时务必谨慎，因为这可能对复制性能产生负面影响。 如果能很有把握地预测出所有订阅服务器都将在该时间段内定期同步，则建议使用较低的设置。  
  
         合并发布的保持期具有 24 小时的宽限期，以适应处于不同时区中的订阅服务器。 例如，如果将保持期设置为 1 天，则实际的保持期为 48 小时。  
  
    -   可以指定订阅永不过期，但是强烈建议您不要使用此值，因为这样将无法清除元数据。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可在“发布属性 - \<Publication>”对话框的“常规”页面上设置订阅的过期期限 。 有关访问此对话框的详细信息，请参阅 [View and Modify Publication Properties](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
#### <a name="to-set-the-expiration-period-for-subscriptions"></a>设置订阅的过期期限  
  
1.  在“发布属性 - \<Publication>”对话框的“常规”页面上的“订阅过期”部分中，指定订阅是否应过期  。  
  
2.  如果它们应该过期，请指定一个过期时间段。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 您可以使用复制存储过程在创建发布时设置此值或以后修改此值。  
  
#### <a name="to-set-the-expiration-period-for-a-subscription-to-a-snapshot-or-transactional-publication"></a>为快照或事务发布设置订阅过期时间  
  
1.  在发布服务器上，执行 [sp_addpublication](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)。 以小时为单位为 \@retention 指定所需的订阅过期时间。 默认的过期时间为 336 个小时。 有关详细信息，请参阅 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)。  
  
#### <a name="to-set-the-expiration-period-for-a-subscription-to-a-merge-publication"></a>为合并发布设置订阅过期时间  
  
1.  在发布服务器上，执行 [sp_addmergepublication](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)。 为 \@retention 指定所需的订阅过期时间值。 为 \@retention_period_unit 指定此过期时间的表示单位，此单位可以是以下单位之一：  
  
    -   **1** = 周  
  
    -   **2** = 月  
  
    -   **3** = 年  
  
     默认的过期时间为 14 天。 有关详细信息，请参阅 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)。  
  
#### <a name="to-change-the-expiration-period-for-a-subscription-to-a-snapshot-or-transactional-publication"></a>更改快照或事务发布的订阅过期时间  
  
1.  在发布服务器上，执行 [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)。 为 \@property 指定保留期，并以小时为单位为 \@value 指定新的订阅过期时间  。  
  
#### <a name="to-change-the-expiration-period-for-a-subscription-to-a-merge-publication"></a>更改合并发布的订阅过期时间  
  
1.  在发布服务器上，执行 [sp_helpmergepublication](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)，并指定 \@publication 和 \@publisher 。 记下结果集中的 **retention_period_unit** 值，此值可能为以下值之一：  
  
    -   **0** = 天  
  
    -   **1** = 周  
  
    -   **2** = 月  
  
    -   **3** = 年  
  
2.  在发布服务器上，执行 [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)。 为 \@property 指定保留期，并以步骤 1 中的保持期单位为 \@value 指定新的订阅过期时间  。  
  
3.  （可选）在发布服务器上，执行 [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)。 为 \@property 指定 retention_period_unit，并为 \@value 指定新的订阅过期时间单位  。  
  
## <a name="see-also"></a>另请参阅  
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [订阅过期和停用](../../../relational-databases/replication/subscription-expiration-and-deactivation.md)  
  
  
