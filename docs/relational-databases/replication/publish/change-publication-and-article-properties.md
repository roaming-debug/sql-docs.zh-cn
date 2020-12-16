---
description: 更改发布和项目属性
title: 更改发布和项目属性 | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- modifying article properties
- modifying publication properties
- administering replication, properties
- publications [SQL Server replication], changing properties
- articles [SQL Server replication], properties
ms.assetid: f7df51ef-c088-4efc-b247-f91fb2c6ff32
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 49a92b0caf261b812e83a4a888930c8533d7cdf9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97432147"
---
# <a name="change-publication-and-article-properties"></a>更改发布和项目属性
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  在创建发布后，可以更改大多数发布和项目属性，但有些发布和项目属性要求重新生成快照和/或重新初始化订阅。 本主题提供有关更改时需要以上一种或两种操作的所有属性的信息。  
  
## <a name="publication-properties-for-snapshot-and-transactional-replication"></a>快照和事务复制的发布属性  
  
|描述|存储过程|属性|要求|  
|-----------------|----------------------|----------------|------------------|  
|更改快照格式。|**sp_changepublication**|**sync_method**|新建快照。|  
|更改快照位置。|**sp_changepublication**|**alt_snapshot_folder**<br /><br /> **snapshot_in_defaultfolder**|新建快照。|  
|更改快照位置。|**sp_changedistpublisher**|**working_directory**|新建快照。|  
|更改快照压缩。|**sp_changepublication**|**compress_snapshot**|新建快照。|  
|更改任何文件传输协议 (FTP) 快照选项。|**sp_changepublication**|**enabled_for_internet**<br /><br /> **ftp_address**<br /><br /> **ftp_login**<br /><br /> **ftp_password**<br /><br /> **ftp_port**<br /><br /> **ftp_subdirectory**|新建快照。|  
|更改快照前或快照后脚本位置。|**sp_changepublication**|**pre_snapshot_script**<br /><br /> **post_snapshot_script**|新建快照（更改脚本内容时也需要）。<br /><br /> 对订阅服务器应用新脚本需要进行重新初始化。|  
|启用或禁用对非 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 订阅服务器的支持。|**sp_changepublication**|**is_enabled_for_het_sub**|新建快照。|  
|更改排队更新订阅的冲突报告|**sp_changepublication**|**centralized_conflicts**|只有在没有活动订阅时才可以进行更改。|  
|更改排队更新订阅的冲突解决策略。|**sp_changepublication**|**conflict_policy**|只有在没有活动订阅时才可以进行更改。|  
  
## <a name="article-properties-for-snapshot-and-transactional-replication"></a>快照和事务复制的项目属性  
  
|描述|存储过程|属性|要求|  
|-----------------|----------------------|----------------|------------------|  
|删除项目|**sp_droparticle**|所有参数。|项目可以在创建订阅之前删除。 使用存储过程，可以删除项目的订阅；如果使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]，则必须删除整个订阅，然后再重新创建订阅并进行同步。 有关详细信息，请参阅[向现有发布添加项目和从中删除项目](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)。|  
|更改列筛选器。|**sp_articlecolumn**|`@column`<br /><br /> `@operation`|新建快照。<br /><br /> 重新初始化订阅。|  
|添加行筛选器。|**sp_articlefilter**|所有参数。|新建快照。<br /><br /> 重新初始化订阅。|  
|删除行筛选器。|**sp_articlefilter**|`@article`|新建快照。<br /><br /> 重新初始化订阅。|  
|更改行筛选器。|**sp_articlefilter**|`@filter_clause`|新建快照。<br /><br /> 重新初始化订阅。|  
|更改行筛选器。|**sp_changearticle**|**filter**|新建快照。<br /><br /> 重新初始化订阅。|  
|更改架构选项。|**sp_changearticle**|**schema_option**|新建快照。|  
|更改应用快照之前处理订阅服务器上的表的方式。|**sp_changearticle**|**pre_creation_cmd**|新建快照。|  
|更改项目状态|**sp_changearticle**|**status**|新建快照。|  
|更改 INSERT、UPDATE 和 DELETE 命令。|**sp_changearticle**|**ins_cmd**<br /><br /> **upd_cmd**<br /><br /> **del_cmd**|新建快照。<br /><br /> 重新初始化订阅。|  
|更改目标表名称|**sp_changearticle**|**dest_table**|新建快照。<br /><br /> 重新初始化订阅。|  
|更改目标表所有者（架构）。|**sp_changearticle**|**destination_owner**|新建快照。<br /><br /> 重新初始化订阅。|  
|更改数据类型映射（仅适用于 Oracle 发布）。|**sp_changearticlecolumndatatype**|`@type` <br /><br /> `@length` <br /><br /> `@precision`<br /><br /> `@scale`|新建快照。<br /><br /> 重新初始化订阅。|  
  
## <a name="publication-properties-for-merge-replication"></a>合并复制的发布属性  
  
|描述|存储过程|属性|要求|  
|-----------------|----------------------|----------------|------------------|  
|更改快照格式|**sp_changemergepublication**|**sync_mode**|新建快照。|  
|更改快照位置。|**sp_changemergepublication**|**alt_snapshot_folder**<br /><br /> **snapshot_in_defaultfolder**|新建快照。|  
|更改快照位置。|**sp_changedistpublisher**|**working_directory**|新建快照。|  
|更改快照压缩|**sp_changemergepublication**|**compress_snapshot**|新建快照。|  
|更改所有 FTP 快照选项|**sp_changemergepublication**|**enabled_for_internet**<br /><br /> **ftp_address**<br /><br /> **ftp_login**<br /><br /> **ftp_password**<br /><br /> **ftp_port**<br /><br /> **ftp_subdirectory**|新建快照。|  
|更改快照前或快照后脚本。|**sp_changemergepublication**|**pre_snapshot_script**<br /><br /> **post_snapshot_script**|新建快照（更改脚本内容时也需要）。<br /><br /> 对订阅服务器应用新脚本需要进行重新初始化。|  
|添加联接筛选器或逻辑记录。|**sp_addmergefilter**|所有参数。|新建快照。<br /><br /> 重新初始化订阅。|  
|删除联接筛选器或逻辑记录。|**sp_dropmergefilter**|所有参数。|新建快照。<br /><br /> 重新初始化订阅。|  
|更改联接筛选器或逻辑记录。|**sp_changemergefilter**|`@property`<br /><br /> `@value`|新建快照<br /><br /> 重新初始化订阅。|  
|禁用参数化筛选器（启用参数化筛选器不需要任何特殊操作）。|**sp_changemergepublication**|**false** 的 **false** 值|新建快照。<br /><br /> 重新初始化订阅。|  
|启用或禁用预计算分区。|**sp_changemergepublication**|**use_partition_groups**|新建快照。|  
|启用或禁用 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] 分区优化。|**sp_changemergepublication**|**keep_partition_changes**|重新初始化订阅。|  
|启用或禁用订阅服务器分区验证。|**sp_changemergepublication**|**validate_subscriber_info**|重新初始化订阅。|  
|将发布兼容级别更改为 80sp3 或更低。|**sp_changemergepublication**|**publication_compatibility_level**|新建快照。|  
  
## <a name="article-properties-for-merge-replication"></a>合并复制的项目属性  
  
|描述|存储过程|属性|要求|  
|-----------------|----------------------|----------------|------------------|  
|删除在发布中使用最新参数化筛选器的项目。|**sp_dropmergearticle**|所有参数|新建快照。<br /><br /> 重新初始化订阅。|  
|删除在联接筛选器或逻辑记录中处于父级的项目（这对删除联接有副作用）。|**sp_dropmergearticle**|所有参数|新建快照。<br /><br /> 重新初始化订阅。|  
|删除所有其他环境中的项目。|**sp_dropmergearticle**|所有参数|新建快照。|  
|包括以前未发布的列筛选器。|**sp_mergearticlecolumn**|`@column`<br /><br /> `@operation`|新建快照。<br /><br /> 重新初始化订阅。|  
|添加、删除或更改行筛选器。|**sp_changemergearticle**|**subset_filterclause**|新建快照。<br /><br /> 重新初始化订阅。<br /><br /> 如果添加、删除或更改参数化筛选器，则订阅服务器上挂起的更改在重新初始化期间将无法上载到发布服务器。 若要上载挂起的更改，请在更改筛选器前同步所有订阅。<br /><br /> 如果项目未包含在任何联接筛选器中，您可以用不同的行筛选器删除并再次添加此项目，而无需重新初始化整个订阅。 有关添加和删除项目的详细信息，请参阅[向现有发布添加项目和从中删除项目](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)。|  
|更改架构选项。|**sp_changemergearticle**|**schema_option**|新建快照。|  
|将跟踪从列级更改为行级（从行级跟踪更改为列级跟踪不需要任何特殊操作）。|**sp_changemergearticle**|**false** 的 **false** 值|新建快照。<br /><br /> 重新初始化订阅。|  
|更改在将订阅服务器上所编写的语句应用于发布服务器之前是否检查权限。|**sp_changemergearticle**|**check_permissions**|新建快照。<br /><br /> 重新初始化订阅。|  
|启用或禁用仅下载订阅（更改为其他上载选项或从其他上载选项更改都不需要任何特殊操作）。|**sp_changemergearticle**|将 **2** 的值更改为 **2**，或者从此值更改|重新初始化订阅。|  
|更改目标表所有者。|**sp_changemergearticle**|**destination_owner**|新建快照。<br /><br /> 重新初始化订阅。|  
  
## <a name="see-also"></a>另请参阅  
 [复制管理常见问题解答](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)   
 [创建并应用快照](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [重新初始化订阅](../../../relational-databases/replication/reinitialize-subscriptions.md)   
 [sp_addmergefilter (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md)   
 [sp_articlecolumn (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_articlefilter (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)   
 [sp_changearticle (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_changearticlecolumndatatype (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-changearticlecolumndatatype-transact-sql.md)   
 [sp_changedistpublisher (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_changemergearticle (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [sp_changemergefilter (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-changemergefilter-transact-sql.md)   
 [sp_changemergepublication (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_changepublication (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_droparticle (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_dropmergearticle (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md)   
 [sp_dropmergefilter (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)   
 [sp_mergearticlecolumn (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md)  
  
  
