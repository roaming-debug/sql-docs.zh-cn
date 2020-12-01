---
description: 参数化筛选器 - 参数化行筛选器
title: 参数化行筛选器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], dynamic filters
- merge replication [SQL Server replication], dynamic filters
- parameterized filters [SQL Server replication]
- filters [SQL Server replication], dynamic
- merge replication parameterized filters [SQL Server replication]
- publications [SQL Server replication], parameterized filters
- parameterized filters [SQL Server replication], about parameterized filters
- filters [SQL Server replication], parameterized
- dynamic filters [SQL Server replication]
ms.assetid: b48a6825-068f-47c8-afdc-c83540da4639
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 656f36cce2c1b458f2eb85c734709691b59a82bf
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88423541"
---
# <a name="parameterized-filters---parameterized-row-filters"></a>参数化筛选器 - 参数化行筛选器
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  参数化行筛选器允许将不同分区的数据发送到不同的订阅服务器，而无需创建多个发布（在早期版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中，参数化筛选器称为“动态筛选器”）。 分区是表中行的子集；根据创建参数化行筛选器时选择的设置，已发布表中的每一行可以仅属于一个分区（生成不重叠的分区），也可以属于两个或多个分区（生成重叠分区）。  
  
 可以在订阅之间共享不重叠的分区，也可以对其进行限制，使得一个给定分区只有一个订阅。 本主题后面的“使用适当的筛选选项”部分介绍了控制分区行为的设置。 通过这些设置，可以根据应用程序和性能要求来调整参数化筛选。 通常，重叠分区的灵活性更大，而复制到单个订阅的不重叠分区的性能则更佳。  
  
 参数化筛选器在单个表中使用，通常与联接筛选器配合使用来将筛选扩展到相关表。 有关详细信息，请参阅 [Join Filters](../../../relational-databases/replication/merge/join-filters.md)。  
  
 若要定义或修改参数化行筛选器，请参阅 [定义和修改合并项目的参数化行筛选器](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)。  
  
## <a name="how-parameterized-filters-work"></a>参数化筛选器的工作机制  
 参数化行筛选器使用 WHERE 子句来选择要发布的适当数据。 不要在该子句中指定文字值（像在静态行筛选器中那样），而是指定以下一个或两个系统函数：SUSER_SNAME() 和 HOST_NAME()。 也可以使用用户定义函数，但是函数主体中必须包含 SUSER_SNAME() 或 HOST_NAME()，或者计算这些系统函数之一（如 `MyUDF(SUSER_SNAME()`）。 如果用户定义函数的主体中包含 SUSER_SNAME() 或 HOST_NAME()，则无法向该函数传递参数。  
  
 系统函数 SUSER_SNAME() 和 HOST_NAME() 不是合并复制特有的，但合并复制使用它们来进行参数化筛选：  
  
-   SUSER_SNAME() 返回连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例所用的登录信息。 在参数化筛选器中使用该函数时，它会返回合并代理连接到发布服务器所用的登录名（在创建订阅时指定登录名）。  
  
-   HOST_NAME() 返回连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例的计算机的名称。 在参数化筛选器中使用该函数时，默认情况下，它会返回运行合并代理的计算机的名称。 对于请求订阅，此名称为订阅服务器的名称；对于推送订阅，此名称为分发服务器的名称。  
  
     也可以使用非订阅服务器名称或分发服务器名称的值来覆盖此函数。 应用程序通常会用更有意义的值来覆盖此函数，如销售人员姓名或销售人员 ID。 有关详细信息，请参阅本主题中的“覆盖 HOST_NAME() 值”部分。  
  
 将系统函数返回的值与在要筛选的表中指定的列进行比较，并将相应的数据下载到订阅服务器。 此比较是在初始化订阅时（因此初始快照中仅包含相应的数据）和每次同步订阅时进行的。 默认情况下，如果发布服务器上的更改导致行移出分区，则该行将从订阅服务器中删除（可以使用 [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md) 的 `@allow_partition_realignment` 参数控制此行为）。  
  
> [!NOTE]  
>  对参数化筛选器进行比较时，将始终使用数据库排序规则。 例如，如果数据库排序规则不区分大小写，而表或列排序规则区分大小写，那么该比较将不区分大小写。  
  
### <a name="filtering-with-suser_sname"></a>使用 SUSER_SNAME() 进行筛选  
 下面以 **示例数据库中的** Employee 表 [!INCLUDE[ssSampleDBCoShort](../../../includes/sssampledbcoshort-md.md)] 为例进行说明。 此表包含 **LoginID** 列，该列中每个雇员的登录名的格式为“*domain\login*”。 若要筛选此表以便雇员仅接收与之相关的数据，请指定筛选子句：  
  
```  
LoginID = SUSER_SNAME()  
```  
  
 例如，其中一个雇员的值为“adventure-works\john5”。 合并代理连接到发布服务器时，使用创建订阅时指定的登录名（此例中为“adventure-works\john5”）。 然后，合并代理将 SUSER_SNAME() 返回的值与表中的值进行比较，并仅下载 **LoginID** 列中值为“adventure-works\john5”的行。  
  
### <a name="filtering-with-host_name"></a>使用 HOST_NAME() 进行筛选  
 下面以 **HumanResources.Employee** 表为例进行说明。 假定此表包含列 **ComputerName** ，该列中每个雇员的计算机名称的格式为“*name_computertype*”。 若要筛选此表以便雇员仅接收与之相关的数据，请指定筛选子句：  
  
```  
ComputerName = HOST_NAME()  
```  
  
 例如，其中一个雇员的值为“john5_laptop”。 合并代理连接到发布服务器时，会将 HOST_NAME() 返回的值与表中的值进行比较，并仅下载 **ComputerName** 列中包含值为“john5_laptop”的行。  
  
 也可以在筛选器中组合这些函数。 例如，如果要确保雇员仅在使用其登录名登录其计算机时接收数据，则筛选子句可以是：  
  
```  
LoginID = SUSER_SNAME() AND ComputerName = HOST_NAME()  
```  
  
 除非覆盖 HOST_NAME() 值，否则使用 HOST_NAME() 进行筛选通常仅用于请求订阅。 函数返回的值为运行合并代理的计算机的名称。 对于请求订阅，每个订阅的值都不同，但对于推送订阅，每个订阅的值都相同（对于推送订阅，所有合并代理都在分发服务器上运行）。  
  
> [!IMPORTANT]  
>  函数 HOST_NAME() 的值可以覆盖；因此不能使用包含 HOST_NAME() 的筛选器来控制对数据分区的访问。 若要控制对数据分区的访问，请将 SUSER_SNAME()、SUSER_SNAME() 和 HOST_NAME() 结合使用，或者使用静态行筛选器。  
  
#### <a name="overriding-the-host_name-value"></a>覆盖 HOST_NAME() 值  
 如前所述，HOST_NAME() 在默认情况下会返回连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例的计算机的名称。 使用参数化筛选器时，通常通过在创建订阅时提供值来覆盖此值。 这样，HOST_NAME() 函数将返回指定的值而非计算机的名称。  
  
> [!NOTE]  
>  如果覆盖 HOST_NAME()，则对 HOST_NAME() 函数的所有调用将全部返回指定的值。 请确保其他应用程序未依赖于返回计算机名称的 HOST_NAME()。  
  
 下面以 **HumanResources.Employee** 表为例进行说明。 此表包含 **EmployeeID** 列。 若要筛选此表以便每个雇员仅接收与他们相关的数据，请指定筛选子句：  
  
 `EmployeeID = CONVERT(int,HOST_NAME())`  
  
 例如，为雇员 Pamela Ansman-Wolfe 分配的雇员 ID 为 280。 为此雇员创建订阅时，为 HOST_NAME() 值指定雇员 ID 值（在此例中为 280）。 合并代理连接到发布服务器时，会将 HOST_NAME() 返回的值与表中的值进行比较，并仅下载 **EmployeeID** 列中值为 280 的行。  
  
> [!IMPORTANT]
>  HOST_NAME() 函数会返回 **nchar** 值，因此如果筛选子句中的列为数值数据类型（如前面示例所示），则必须使用 CONVERT。 出于性能方面的考虑，建议您不要对参数化行筛选器子句（如 `CONVERT(nchar,EmployeeID) = HOST_NAME()`）中的列名应用函数。 建议改为使用示例中所示的方法： `EmployeeID = CONVERT(int,HOST_NAME())`。 此子句可用于 [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) 的 `@subset_filterclause` 参数，但通常不能在新建发布向导中使用（该向导会执行筛选子句以对其进行验证，而此验证会失败，因为计算机名称无法转换为 int中，参数化筛选器称为“动态筛选器”）。 如果使用的是新建发布向导，建议在向导中指定 `CONVERT(nchar,EmployeeID) = HOST_NAME()` ，然后在为发布创建快照之前，使用 [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) 将子句更改为 `EmployeeID = CONVERT(int,HOST_NAME())` 。  
  
 **覆盖 HOST_NAME() 值**  
  
 使用下列方法之一覆盖 HOST_NAME() 值：  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]：在新建订阅向导的“HOST\_NAME\(\) 值”页中，指定一个值。 有关创建订阅的详细信息，请参阅[订阅发布](../../../relational-databases/replication/subscribe-to-publications.md)。  
  
-   复制 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 编程：为 [sp_addmergesubscription &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)（对于推送订阅）或 [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)（对于请求订阅）的 `@hostname` 参数指定一个值。  
  
-   合并代理：在命令行中或通过代理配置文件指定 **-Hostname** 参数的值。 有关合并代理的详细信息，请参阅 [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)。 有关代理配置文件的详细信息，请参阅 [Replication Agent Profiles](../../../relational-databases/replication/agents/replication-agent-profiles.md)。  
  
## <a name="initializing-a-subscription-to-a-publication-with-parameterized-filters"></a>使用参数化筛选器初始化发布的订阅  
 在合并发布中使用参数化行筛选器时，复制将使用由两部分构成的快照初始化各个订阅。 有关详细信息，请参阅 [Snapshots for Merge Publications with Parameterized Filters](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  
## <a name="using-the-appropriate-filtering-options"></a>使用适当的筛选选项  
 使用参数化筛选器时，您可以控制两个关键方面：  
  
-   如何通过合并复制处理筛选器，可以通过以下两个发布设置之一控制： **use partition groups** 和 **keep partition changes**。  
  
-   数据如何在订阅服务器之间共享，必须通过项目设置 **partition options** 来反映。  
  
 若要设置筛选选项，请参阅 [Optimize Parameterized Row Filters](../../../relational-databases/replication/publish/optimize-parameterized-row-filters.md)。  
  
### <a name="setting-use-partition-groups-and-keep-partition-changes"></a>设置“use partition groups”和“keep partition changes”  
 **use partition groups** 选项和 **keep partition changes** 选项通过在发布数据库中存储其他元数据提高具有筛选项目的发布的同步性能。 **use partition groups** 选项通过使用预计算分区功能进一步提高性能。 如果发布中的项目符合一组要求，则此选项在默认情况下将设置为 **true** 。 有关这些要求的详细信息，请参阅[使用预计算分区优化参数化筛选器性能](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)。 如果项目不符合使用预计算分区的要求，则 **keep partition changes** 选项设置为 **true**。  
  
### <a name="setting-partition-options"></a>设置“partition options”  
 创建项目时，根据订阅服务器共享已筛选表中的数据的方式，指定 **partition options** 属性的值。 可以使用 [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)、 [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)和 **“项目属性”** 对话框，将该属性设置为四个值中的一个。 可以使用 **“添加筛选器”** 或 **“编辑筛选器”** 对话框（可以通过新建发布向导和 **“发布属性”** 对话框访问），将该属性设置为两个值中的一个。 下表总结了可用的值：  
  
|说明|“添加筛选器”和“编辑筛选器”中的值|“项目属性”中的值|存储过程中的值|  
|-----------------|-----------------------------------------|---------------------------------|--------------------------------|  
|分区中的数据重叠，并且订阅服务器可以更新参数化筛选器中引用的列。|**此表中的行将转到多个订阅**|**重叠**|**0**|  
|分区中的数据重叠，而订阅服务器无法更新参数化筛选器中引用的列。|N/A*|**重叠，不允许分区外的数据更改**|**1**|  
|分区中的数据不重叠，并且数据由所有订阅共享。 订阅服务器无法更新参数化筛选器中引用的列。|N/A*|**不重叠，由所有订阅共享**|**2**|  
|分区中的数据不重叠，每个分区只有一个订阅。 订阅服务器无法更新参数化筛选器中引用的列。**|**此表中的行将仅转到一个订阅**|**不重叠，一个订阅**|**3**|  
  
 \*如果基础筛选选项设置为 **0**、**1** 或 **2**，则“添加筛选器”和“编辑筛选器”对话框中会显示“此表中的行将转到多个订阅”。  
  
 **如果你指定此选项，则相应项目中的每个数据分区只能有一个订阅。 如果创建了另一个订阅，而这个新订阅的筛选条件解析到的分区与现有订阅的分区相同，则会删除现有订阅。  
  
> [!IMPORTANT]  
>  必须根据订阅服务器共享数据的方式来设置 **partition options** 值。 例如，如果指定分区不重叠，每个分区一个订阅，但数据随后在另一台订阅服务器上被更新，则合并代理在同步期间会失败，且可能无法收敛。  
  
#### <a name="selecting-the-appropriate-partition-option"></a>选择适当的分区选项  
 不重叠分区与预计算分区协同工作，可以在接受某些功能限制的情况下提高性能。 预计算分区可以加快下载到订阅服务器的速度，但会降低上载速度。 不重叠分区则可以将与预计算分区相关联的上载开销降至最低。 使用的参数化筛选器和联接筛选器越复杂，不重叠分区的性能优势就越明显。  
  
 决定应在发布中使用的分区选项时，请考虑下列方案。  
  
-   [!INCLUDE[ssSampleDBCoShort](../../../includes/sssampledbcoshort-md.md)] 有一个移动销售团队，该团队中的每个销售人员分别负责给定邮政编码内的客户。 该应用程序要求，如果某个客户从一个销售区域移到另一个销售区域，应更新其对应的邮政编码，以便将该客户分配给另一个销售人员。 参数化筛选器基于客户的邮政编码，通过更新，从一个销售人员的分区中删除邮政编码并将该邮政编码插入到另一个销售人员的分区中。 这需要能够更新参数化筛选器中引用的列的重叠分区。 此选项具有最大的灵活性，但性能不如不重叠的分区。  
  
-   雇佣机构具有提供给每个省/市/自治区各县的地区办事处的数据。 数据是不重叠的；机构总部的表中的每一行仅包含在一个分区中，但该分区将发送到同一个县的多个办事处。 订阅之间共享分区的不重叠分区选项可以实现此示例中的要求，它可以在满足应用要求同时，提供优于重叠分区的性能。  
  
-   如果具有不重叠分区，且只有一个订阅接收和更新分区中的数据，则可以获得更多的性能收益。 此方案通常用于销售点系统和现场工作团队应用（在这些应用中，主要在订阅服务器上收集数据，然后将数据上载到发布服务器）。 例如运输应用中的 **Package** 表：将每个包都装载到卡车上后，在 **Package** 表中包的状态将会更改，并且该更改将复制回总部。 司机不会更新两辆不同卡车上同一个包的状态，因此 **Package** 表对于每个分区一个订阅的不重叠分区是一个很好的候选项。  
  
#### <a name="considerations-for-nonoverlapping-partitions"></a>不重叠分区的注意事项  
 在使用不重叠分区时，请注意下列事项。  
  
##### <a name="general-considerations"></a>一般注意事项  
  
-   发布必须使用预计算分区。  
  
-   一行只能属于一个分区。  
  
-   项目不能是逻辑记录的一部分。  
  
-   不支持备用同步伙伴（不推荐使用此功能）。  
  
-   订阅服务器无法更新参数化筛选器中引用的列。  
  
-   如果订阅服务器上的插入不属于分区，则不会删除该插入。 但是，不会将其复制到其他订阅服务器。  
  
-   在某些具有重叠分区的情况下，合并代理插入数据时，标识范围会有所调整。 对于不重叠分区，范围只能在插入期间由有权调整订阅数据库中标识范围的用户进行调整。 用户或者必须拥有表，或者必须是 **sysadmin** 固定服务器角色、 **db_owner** 固定数据库角色或 **db_ddladmin** 固定数据库角色的成员。  
  
##### <a name="additional-considerations-for-nonoverlapping-partitions-with-a-single-subscription-per-partition"></a>每个分区一个订阅的不重叠分区的其他注意事项  
  
-   项目只能存在于一个发布中；项目不能重新发布。  
  
-   发布必须允许订阅服务器启动快照处理。 有关详细信息，请参阅 [Snapshots for Merge Publications with Parameterized Filters](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  
##### <a name="additional-considerations-for-join-filters"></a>联接筛选器的其他注意事项  
  
-   在联接筛选器层次结构中，具有重叠分区的项目无法显示在具有不重叠分区的项目之上。 也就是说，如果子项目使用不重叠分区，父项目也必须使用不重叠分区。 有关联接筛选器的信息，请参阅 [Join Filters](../../../relational-databases/replication/merge/join-filters.md)。  
  
-   如果联接筛选器中的不重叠分区为子项目，则该联接筛选器的 **join unique key** 属性必须设置为 1。 有关详细信息，请参阅 [Join Filters](../../../relational-databases/replication/merge/join-filters.md)。  
  
-   项目应只有一个参数化筛选器或联接筛选器。 允许具有参数化筛选器，并允许其作为联接筛选器中的父项目。 不允许具有参数化筛选器，并且不允许其作为联接筛选器中的子项目。 也不允许具有多个联接筛选器。  
  
-   如果发布服务器上的两个表有联接筛选器关系，并且子表的行在父表中没有对应的行，则插入缺少的父行不会导致相关行下载到订阅服务器（这些行将随重叠分区下载）。 例如，如果 **SalesOrderDetail** 表的行在 **SalesOrderHeader** 表中没有对应的行，则在 **SalesOrderHeader** 中插入缺少的行后，该行将下载到订阅服务器，但 **SalesOrderDetail** 中对应的行不会下载到订阅服务器。  
  
## <a name="see-also"></a>另请参阅  
 [基于时间的行筛选器的最佳做法](../../../relational-databases/replication/merge/best-practices-for-time-based-row-filters.md)   
 [筛选已发布数据](../../../relational-databases/replication/publish/filter-published-data.md)   
 [为合并复制筛选已发布数据](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)  
  
  
