---
title: Oracle 发布服务器的数据类型映射
description: 介绍如何使用 SQL Server Management Studio (SSMS) 在 SQL Server 中指定 Oracle 发布服务器的数据类型映射。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Oracle publishing [SQL Server replication], data type mapping
- data types [SQL Server replication], Oracle publishing
- mapping data types [SQL Server replication]
ms.assetid: f172d631-3b8c-4912-bd0f-568366cd9870
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a7381574bccfa00aa58c1666d3aae2b842c32be1
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076509"
---
# <a name="specify-data-type-mappings-for-an-oracle-publisher"></a>指定 Oracle 发布服务器的数据类型映射
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中指定 Oracle 发布服务器的数据类型映射。 虽然已经为 Oracle 发布服务器提供了一组默认数据类型映射，但可能仍有必要为给定发布指定不同的映射。  
  
 **本主题内容**  
  
-   **指定 Oracle 发布服务器的数据类型映射，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 在“项目属性 - \<Article>”对话框的“数据映射”选项卡上指定数据类型映射 。 可以从新建发布向导和“发布属性 - \<Publication>”对话框的“项目”页中访问该对话框 。 有关使用该向导和访问该对话框的详细信息，请参阅[从 Oracle 数据库创建发布](../../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md)以及[查看和修改发布属性](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
#### <a name="to-specify-a-data-type-mapping"></a>指定数据类型映射  
  
1.  在新建发布向导或“发布属性 - \<Publication>”对话框的“项目”页上，选择一个表，然后单击“项目属性”  。  
  
2.  单击 **“设置突出显示的表项目的属性”** 。  
  
3.  在“项目属性 - \<Article>”对话框的“数据映射”选项卡上，从“订阅服务器数据类型”列中选择映射  ：  
  
    -   对于某些数据类型，只能有一种可能的映射，在这种情况下，属性网格中的相应列是只读的。  
  
    -   对于某些数据类型，有多种可供选择的映射类型。 除非您的应用程序需要使用其他映射，否则[!INCLUDE[msCoName](../../../includes/msconame-md.md)] 建议使用默认映射。 有关详细信息，请参阅 [Data Type Mapping for Oracle Publishers](../../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)。  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程，以编程方式指定自定义数据类型映射。 还可以设置在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 与非 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库管理系统 (DBMS) 间映射数据类型时使用的默认映射。 有关详细信息，请参阅 [Data Type Mapping for Oracle Publishers](../../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)。  
  
#### <a name="to-define-custom-data-type-mappings-when-creating-an-article-belonging-to-an-oracle-publication"></a>在创建属于 Oracle 发布的项目时定义自定义数据类型映射  
  
1.  如果尚不存在 Oracle 发布，请创建一个。  
  
2.  在分发服务器上，执行 [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)。 将 \@use_default_datatypes 的值指定为 0。 有关详细信息，请参阅 [定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
3.  在分发服务器上，执行 [sp_helparticlecolumns](../../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md) 以查看已发布项目中某列的现有映射。  
  
4.  在分发服务器上，执行 [sp_changearticlecolumndatatype](../../../relational-databases/system-stored-procedures/sp-changearticlecolumndatatype-transact-sql.md)。 为 \@publisher 指定 Oracle 发布服务器的名称，并指定 \@publication、\@article 和 \@column 以定义已发布的列。 为 \@type 指定要映射到的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型的名称，并在必要时指定 \@length、\@precision 和 \@scale。  
  
5.  在分发服务器上，执行 [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)。 这将创建用于从 Oracle 发布生成快照的视图。  
  
#### <a name="to-specify-a-mapping-as-the-default-mapping-for-a-data-type"></a>将映射指定为数据类型的默认映射  
  
1.  （可选）在分发服务器上，对任意一个数据库执行 [sp_getdefaultdatatypemapping](../../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md)。 指定 \@source_dbms、\@source_type、\@destination_dbms、\@destination_version 以及标识源 DBMS 所需的其他任何参数。 将使用输出参数返回有关目标 DBMS 中当前映射的数据类型的信息。  
  
2.  （可选）在分发服务器上，对任意一个数据库执行 [sp_helpdatatypemap](../../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)。 指定 \@source_dbms 以及筛选结果集所需的其他任何参数。 记下结果集中所需映射的 **mapping_id** 的值。  
  
3.  在分发服务器上，对任意一个数据库执行 [sp_setdefaultdatatypemapping](../../../relational-databases/system-stored-procedures/sp-setdefaultdatatypemapping-transact-sql.md)。  
  
    -   如果知道在步骤 2 中获取的 mapping_id 的所需值，请为 \@mapping_id 指定该值。  
  
    -   如果不知道 mapping_id，请指定 \@source_dbms、\@source_type、\@destination_dbms、\@destination_type 参数以及标识现有映射所需的其他任何参数。  
  
#### <a name="to-find-valid-data-types-for-a-given-oracle-data-type"></a>查找针对给定的 Oracle 数据类型的有效数据类型  
  
1.  在分发服务器上，对任何一个数据库执行 [sp_helpdatatypemap](../../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)。 将 \@source_dbms 的值指定为 ORACLE ，并指定筛选结果集所需的其他任何参数。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 示例 (Transact-SQL)  
 此示例将对其类型为 Oracle 数据类型 NUMBER 的列进行更改，以将该列映射到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型 **numeric**(38,38) 而非默认数据类型 **float** 中指定 Oracle 发布服务器的数据类型映射。  
  
 [!code-sql[HowTo#sp_changecolumndatatype](../../../relational-databases/replication/codesnippet/tsql/specify-data-type-mappin_1.sql)]  
  
 此示例查询将返回 Oracle 9 数据类型 **CHAR** 的默认映射及替代映射。  
  
 [!code-sql[HowTo#sp_helpcolumndatatype_char](../../../relational-databases/replication/codesnippet/tsql/specify-data-type-mappin_2.sql)]  
  
 此示例查询在未对 Oracle 9 数据类型 **NUMBER** 指定小数位数或精度时，返回该数据类型的默认映射。  
  
 [!code-sql[HowTo#sp_helpcolumndatatype_number](../../../relational-databases/replication/codesnippet/tsql/specify-data-type-mappin_3.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [Data Type Mapping for Oracle Publishers](../../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)   
 [异类数据库复制](../../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [配置 Oracle 发布服务器](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)  
  
  
