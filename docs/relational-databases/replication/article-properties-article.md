---
description: 项目属性 - &lt;项目&gt;
title: 项目属性 - &lt;项目&gt; | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.articleproperties.f1
helpviewer_keywords:
- Article Properties dialog box
ms.assetid: 6dd601a4-1233-43d9-a9f0-bc8d84e5d188
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 03b08fa667f8b0753c315a7cf66123618cb493f7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475858"
---
# <a name="article-properties---ltarticlegt"></a>项目属性 - &lt;项目&gt;
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  可通过新建发布向导和 **“发布属性”** 对话框访问 **“项目属性”** 对话框。 使用该对话框可以查看和设置所有类型项目的属性。 对于某些属性来说，只有在创建了发布时才能设置；而对于其他属性，只有在发布没有活动订阅时才能设置。 无法设置的属性将显示为只读。  
  
> [!NOTE]  
>  创建发布之后，某些属性更改要求新的快照。 如果发布具有多个订阅，某些更改还会要求重新初始化所有订阅。 有关详细信息，请参阅[更改发布和项目属性](../../relational-databases/replication/publish/change-publication-and-article-properties.md)。  
  
 **“项目属性”** 对话框中的每一个属性都包含相应的说明。 单击一个属性，即会在对话框的底部显示该属性的说明。 本主题提供众多属性的其他信息。 属性分为以下类别：  
  
-   适用于所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布的属性。  
  
-   适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的事务发布的属性。  
  
-   适用于合并发布的属性。  
  
-   适用于 Oracle 发布服务器中的事务发布和快照发布的属性。  
  
## <a name="options-for-all-publications"></a>用于所有发布的选项  
 **复制表分区方案** / **复制索引分区方案**  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 引入了表分区和索引分区功能，这些功能与通过行筛选器和列筛选器提供的分区复制功能无关。 **“复制表分区方案”** 选项和 **“复制索引分区方案”** 选项指定了是否应将分区方案复制到订阅服务器。 有关分区的详细信息，请参阅 [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md)。  
  
 **转换数据类型**  
 确定在订阅服务器上创建对象时是否从用户定义数据类型转换为基本数据类型。 用户定义数据类型包括 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]中引入的用户定义的 CLR 类型。 如果将这些数据类型复制到 **早期版本，请指定值为** True [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]；这样可确保在订阅服务器上正确处理这些数据类型。  
  
 **在订阅服务器上创建架构**  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 引入了架构，使用 CREATE SCHEMA 语句定义架构。 架构就是对象的所有者，其用于多部分名称中，例如 \<Database>.\<Schema>.\<Object>。如果数据库中有非 DBO 架构拥有的对象，则复制功能可以在订阅服务器上创建这些架构，从而可以创建发布的对象。  
  
 若要向早于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]版本复制数据，请执行以下操作：  
  
-   将此选项设置为 **False**，因为以前的版本不支持 CREATE SCHEMA。  
  
-   对于每一个架构，都向订阅数据库添加一个与架构同名的用户。  
  
 **“将 XML 转换为 NTEXT”** 、 **“将 MAX 数据类型转换为 NTEXT 和 IMAGE”** 、 **“将新的 datetime 转换为 NVARCHAR”** 、 **“将文件流转换为 MAX 数据类型”** 、 **“将大型 CLR 转换为 MAX 数据类型”** 、 **“将 hierarchyId 转换为 MAX 数据类型”** 以及 **“将 spatial 转换为 MAX 数据类型”** 。  
 确定是否按规定转换数据类型和属性。 如果要将这些数据类型复制到较低版本的 **中，则将此选项值指定为** “True” [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]， 从而确保它们可以在订阅服务器得到正确处理。  
  
 **目标对象名称**  
 在订阅数据库中创建的对象的名称。 对于启用了对等事务复制的发布中的项目，此选项不可更改。  
  
 **目标对象所有者**  
 在订阅数据库中创建对象时所处的架构。 默认值为发布数据库中对象所属的架构，但下列情况除外：  
  
-   对于合并发布中兼容级别低于 90 的项目：默认情况下，所有者保留为空，并且在订阅服务器上创建对象的过程中指定为 **dbo** 。  
  
-   对于 Oracle 发布中的项目：默认情况下，所有者指定为 **dbo**。  
  
-   对于使用字符模式快照（用于非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器以及 [!INCLUDE[ssEW](../../includes/ssew-md.md)] 订阅服务器）的发布中的项目：默认情况下，所有者保留为空。 所有者默认为与分发代理或合并代理连接到订阅服务器所使用的帐户关联的所有者。  
  
 对于启用了对等事务复制的发布中的项目，此选项不可更改。  
  
 **自动管理标识范围**  
 默认情况下，复制将管理发布服务器和各个订阅服务器上的所有标识列。 会给每个复制节点分配一定范围的标识值（使用 **“发布服务器范围大小”** 和 **“订阅服务器范围大小”** 选项指定），以确保给定的值只会在一个节点上使用。 有关详细信息，请参阅[复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)。  
  
## <a name="options-for-transactional-publications"></a>用于事务发布的选项  
 **复制 INSERT、UPDATE 和 DELETE 存储过程**  
 如果在此对话框的 **“语句传递”** 部分中选择使用存储过程将更改传播到订阅服务器（默认设置），则需要选择是否将这些过程复制到各个订阅服务器。 如果选择 **False**，则必须手动复制这些过程，否则在尝试传递更改时，分发代理将失败。  
  
 **Statement delivery**  
 此部分中的选项适用于所有表，包括以表的形式复制的索引视图。 除非您的应用程序需要其他功能，否则[!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议使用默认选项。 默认情况下，事务复制通过安装在每个订阅服务器上的一组存储过程，将更改传播到订阅服务器。 对发布服务器上的表进行插入、更新或删除操作时，该操作将转换为对订阅服务器上的存储过程的调用。  
  
 **“传递语句”** 选项用于指定是否使用存储过程，以及如果使用存储过程，传递给该过程的参数应使用何种格式。 通过 **“存储过程”** 选项，可以使用复制自动创建的过程，也可以替换已创建的自定义过程。  
  
 有关详细信息，请参阅[指定如何传播事务项目的更改](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
 **复制**  
 此选项仅适用于存储过程。 它将确定是否复制存储过程的定义（CREATE PROCEDURE 语句）或其执行代码。 如果复制存储过程的执行代码，则在初始化订阅时，会将过程定义复制到订阅服务器；当在发布服务器上执行该过程时，复制功能将在订阅服务器上执行相应的过程。 对于执行较大的批处理操作的情况，这样可以显著地提高性能。 有关详细信息，请参阅 [Publishing Stored Procedure Execution in Transactional Replication](../../relational-databases/replication/transactional/publishing-stored-procedure-execution-in-transactional-replication.md)。  
  
## <a name="options-for-merge-publications"></a>用于合并发布的选项  
 合并发布的“项目属性”对话框有两个选项卡：“属性”和“解决程序” 。  
  
### <a name="properties-tab"></a>“属性”选项卡  
 **同步方向**  
 确定是否可以从使用客户端订阅类型的订阅服务器上载更改：  
  
-   **双向** （默认设置）：可以将更改下载到订阅服务器，也可以将更改上载到发布服务器。  
  
-   **仅下载到订阅服务器，禁止订阅服务器更改**：可以将更改下载到订阅服务器，但不能将更改上载到发布服务器。 使用触发器可以防止在订阅服务器上进行更改。  
  
-   **仅下载到订阅服务器，允许订阅服务器更改**：可以将更改下载到订阅服务器，但不能将更改上载到发布服务器。  
  
 有关详细信息，请参阅[使用仅下载项目优化合并复制性能](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md)。  
  
 **分区选项**  
 指定参数化筛选器创建的分区的类型。 有关详细信息，请参阅 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)的“设置‘分区选项’”部分。  
  
 **跟踪级别**  
 确定是否将同一行或同一列的更改视为冲突。  
  
 **验证 INSERT 权限**/ **验证 UPDATE 权限**/ **验证 DELETE 权限**  
 确定在同步期间是否对订阅服务器登录进行检查，以验证其是否具有对发布数据库中已发布表的 INSERT、UPDATE 或 DELETE 权限。 默认值为 **False** ，因为合并复制不要求授予这些权限；对已发布表的访问权限是通过发布访问列表 (PAL) 来控制的。 有关 PAL 的详细信息，请参阅[保护发布服务器](../../relational-databases/replication/security/secure-the-publisher.md)。  
  
 如果希望一个或多个订阅服务器只能将某些更改上载到已发布数据，而不能上载其他更改，则可以要求检查权限。 例如，您可以将某个订阅服务器添加到 PAL，但是不向该订阅服务器授予对发布数据库中表的任何权限。 然后，可以将“验证 DELETE 权限”设置为 **True**，这样，该订阅服务器将能够上载插入和更新操作，但不能上载删除操作。  
  
 **多列 UPDATE**  
 当合并复制执行更新时，它会更新一个 UPDATE 语句中所有更改的列，并将未更改的列重置为其原始值。 在上述情况下，另一种方法是发出多个 UPDATE 语句，每个更改的列各使用一个 UPDATE 语句。 多列 UPDATE 语句通常效率更高，不过，如果表的触发器设置为响应特定列的更新，但触发器会由于这些列在发生更新时进行重置而响应不正常，则应考虑将此选项设置为 **False** 。  
  
> [!IMPORTANT]  
>  不推荐使用此选项，在未来的版本中将删除它。  
  
### <a name="resolver-tab"></a>“冲突解决程序”选项卡  
 **使用默认冲突解决程序**  
 如果选择默认冲突解决程序，则解决冲突时会基于分配给每个订阅服务器的优先级，或基于写入到发布服务器的更改的先后次序，具体取决于所使用的订阅类型。 有关详细信息，请参阅 [检测并解决合并复制冲突](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)。  
  
 **使用自定义冲突解决程序(已在分发服务器上注册)**  
 如果选择使用项目冲突解决程序（ [!INCLUDE[msCoName](../../includes/msconame-md.md)] 提供的冲突解决程序或您编写的冲突解决程序），则必须从该列表框中选择相应的冲突解决程序。 有关详细信息，请参阅 [高级合并复制冲突的检测和解决](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)。  
  
 如果冲突解决程序需要任何输入信息，请在 **“输入冲突解决程序所需的信息”** 文本框中指定该信息。 有关 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 自定义冲突解决程序所需输入内容的详细信息，请参阅 [Microsoft COM-Based Resolvers](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md)。  
  
 **允许订阅服务器在按需同步时交互式解决冲突**  
 如果订阅服务器将使用按需同步（合并复制时的默认设置），并且您希望交互式地解决冲突，请选择此选项。 您可以在新建订阅向导的 **“同步计划”** 页上指定按需同步。 若要交互式地解决冲突，请使用交互式冲突解决程序用户界面。 有关详细信息，请参阅 [Interactive Conflict Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md)。  
  
 **在进行合并之前要求验证数字签名**  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 提供的所有基于 COM 的冲突解决程序均已签名。 选择此选项可以在进行同步时验证冲突解决程序是否有效。  
  
## <a name="options-for-oracle-publications"></a>用于 Oracle 发布的选项  
 Oracle 发布的“项目属性”对话框有两个选项卡：“属性”和“数据映射” 。 Oracle 发布并不能支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布所支持的所有属性。 有关详细信息，请参阅 [Design Considerations and Limitations for Oracle Publishers](../../relational-databases/replication/non-sql/design-considerations-and-limitations-for-oracle-publishers.md)。  
  
### <a name="properties-tab"></a>“属性”选项卡  
 **复制 INSERT、UPDATE 和 DELETE 存储过程**  
 如果项目位于事务发布中，并且您在此对话框的 **“语句传递”** 部分中选择使用存储过程将更改传播到订阅服务器（默认设置），则需要选择是否将这些过程复制到各个订阅服务器。 如果选择 **False**，则必须手动复制这些过程，否则在尝试传递更改时，分发代理将失败。  
  
 **目标对象所有者**  
 如果输入的值不是 **dbo**，请注意以下事项：  
  
-   如果运行 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本的订阅服务器，则必须确保在订阅服务器上创建一个与所输入值同名的架构。 有关详细信息，请参阅 [CREATE SCHEMA (Transact-SQL)](../../t-sql/statements/create-schema-transact-sql.md)。  
  
-   如果运行早于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]的版本的订阅服务器，则对于每一个架构，都需要向订阅数据库添加一个与架构同名的用户。  
  
 **表空间名称**  
 在 Oracle 服务器实例上创建复制更改跟踪表时所在的表空间。 有关详细信息，请参阅[管理 Oracle 表空间](../../relational-databases/replication/non-sql/manage-oracle-tablespaces.md)。  
  
 **Statement delivery**  
 此部分中的选项适用于事务发布中的所有表。 除非您的应用程序需要其他功能，否则[!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议使用默认选项。 默认情况下，事务复制通过安装在每个订阅服务器上的一组存储过程，将更改传播到订阅服务器。 对发布服务器上的表进行插入、更新或删除操作时，该操作将转换为对订阅服务器上的存储过程的调用。  
  
 **“传递语句”** 选项用于指定是否使用存储过程，以及如果使用存储过程，传递给该过程的参数应使用何种格式。 通过 **“存储过程”** 选项，可以使用复制自动创建的过程，也可以替换已创建的自定义过程。  
  
 有关详细信息，请参阅[指定如何传播事务项目的更改](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。  
  
### <a name="data-mapping-tab"></a>“数据映射”选项卡  
 **列名**  
 发布服务器上列的名称（只读）。  
  
 **发布服务器数据类型**  
 发布服务器上列的 Oracle 数据类型（只读）。 只能在 Oracle 数据库中直接更改该数据类型。 有关详细信息，请参阅 Oracle 文档。  
  
 **订阅服务器数据类型**  
 在复制数据时 Oracle 数据类型所映射的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。  
  
-   对于某些数据类型，只能有一种可能的映射，在这种情况下，属性网格中的相应列是只读的。  
  
-   对于某些数据类型，有多种可供选择的映射类型。 除非您的应用程序需要使用其他映射，否则[!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议使用默认映射。 有关详细信息，请参阅 [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)。  
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [查看和修改发布属性](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [创建并应用初始快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [重新初始化订阅](../../relational-databases/replication/reinitialize-a-subscription.md)   
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
