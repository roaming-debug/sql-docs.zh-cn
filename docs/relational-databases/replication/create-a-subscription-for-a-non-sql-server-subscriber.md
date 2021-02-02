---
title: 为非 SQL 订阅服务器创建订阅
description: 介绍如何使用 SQL Server Management Studio (SSMS) 或 Transact-SQL (T-SQL) 在 SQL Server 中为非 SQL Server 订阅服务器创建订阅。
ms.custom: seo-lt-2019
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [SQL Server replication], non-SQL Server Subscribers
- Subscribers [SQL Server replication], non-SQL Server Subscribers
- non-SQL Server Subscribers, subscriptions
ms.assetid: 5020ee68-b988-4d57-8066-67d183e61237
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 36c15a9d7de2c613b3a25b16827bfcc037393524
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076219"
---
# <a name="create-a-subscription-for-a-non-sql-server-subscriber"></a>为非 SQL Server 订阅服务器创建订阅
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中为非 SQL Server 订阅服务器创建订阅。 事务复制和快照复制支持向非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器发布数据。 有关支持的订阅服务器平台的信息，请参阅 [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)中为非 SQL Server 订阅服务器创建订阅。  
  
 **本主题内容**  
  
-   **为非 SQL Server 订阅服务器创建订阅，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 若要为非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器创建订阅，请执行以下步骤：  
  
1.  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分发服务器上安装并配置适当的客户端软件和 OLE DB 访问接口。 有关详细信息，请参阅 [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md) 和 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)。  
  
2.  使用新建发布向导创建发布。 有关创建发布的详细信息，请参阅[创建发布](../../relational-databases/replication/publish/create-a-publication.md)和[从 Oracle 数据库创建发布](../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md)。 在新建发布向导中指定下列选项：  
  
    -   在 **“发布类型”** 页上，选择 **“快照发布”** 或 **“事务发布”** 。  
  
    -   在 **“快照代理”** 页上，清除 **“立即创建快照”** 。  
  
         在为非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器启用发布之后，再创建快照，这样可以确保快照代理生成适合非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器的快照和初始化脚本。  
  
3.  通过“发布属性 - \<PublicationName>”对话框为非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器启用发布。 有关此步骤的详细信息，请参阅 [Publication Properties, Subscription Options](../../relational-databases/replication/publication-properties-subscription-options.md) 。  
  
4.  使用新建订阅向导创建订阅。 本主题提供了有关此步骤的详细信息。  
  
5.  （可选）将 **pre_creation_cmd** 项目属性更改为在订阅服务器上保留表。 本主题提供了有关此步骤的详细信息。  
  
6.  为发布生成快照。 本主题提供了有关此步骤的详细信息。  
  
7.  同步订阅。 有关详细信息，请参阅 [同步推送订阅](../../relational-databases/replication/synchronize-a-push-subscription.md)。  
  
#### <a name="to-enable-a-publication-for-non-sql-server-subscribers"></a>为非 SQL Server 订阅服务器启用发布  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中连接到发布服务器，然后展开服务器节点。  
  
2.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。  
  
3.  右键单击发布，再单击 **“属性”** 。  
  
4.  在 **“订阅选项”** 页上，为选项 **“允许非 SQL Server 订阅服务器”** 选择 **True** 值。 选择此选项可更改多个属性，从而可以使发布与非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器兼容。  
  
    > [!NOTE]  
    >  选择 **True** 会将 **pre_creation_cmd** 项目属性的值设置为“drop”。 此设置指定，如果复制与项目中某个表的名称相匹配，则复制应删除订阅服务器上对应的表。 如果要保留订阅服务器上的现有表，则可以对每个项目使用 [sp_changearticle](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) 存储过程，并为 **pre_creation_cmd** 指定值“none”： `sp_changearticle @publication= 'MyPublication', @article= 'MyArticle', @property='pre_creation_cmd', @value='none'`。  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] 系统将会提示为发布创建新快照。 如果不想在此时创建快照，可以在以后使用下一个“如何”过程中介绍的步骤创建快照。  
  
#### <a name="to-create-a-subscription-for-a-non-sql-server-subscriber"></a>为非 SQL Server 订阅服务器创建订阅  
  
1.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。  
  
2.  右键单击相应的发布，再单击 **“新建订阅”** 。  
  
3.  在 **“分发代理位置”** 页上，确保已选中 **“在分发服务器上运行所有代理”** 。 非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器不支持在订阅服务器上运行代理。  
  
4.  在 **“订阅服务器”** 页上，单击 **“添加订阅服务器”** ，再单击 **“添加非 SQL Server 订阅服务器”** 。  
  
5.  在 **“添加非 SQL Server 订阅服务器”** 对话框中，选择订阅服务器的类型。  
  
6.  在 **“数据源名称”** 中输入值：  
  
    -   对于 Oracle，该值是您配置的透明网络底层 (TNS) 的名称。  
  
    -   对于 IBM，该值可以是任意名称。 通常指定订阅服务器的网络地址。  
  
     此向导并不验证此步骤中输入的数据源名称和步骤 9 中指定的凭据。 直到为订阅运行分发代理后复制才会使用它们。 请确保所有的值已通过使用客户端工具（如 Oracle 的 **sqlplus** ）连接到订阅服务器进行了测试。 有关详细信息，请参阅 [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md) 和 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)。  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] 在该向导的 **“订阅服务器”** 页上，订阅服务器现在显示在 **“订阅服务器”** 列中，并在 **“订阅数据库”** 列中显示只读 **“(默认目标)”** ：  
  
    -   对于 Oracle，一个服务器最多只有一个数据库，因此不必指定数据库。  
  
    -   对于 IBM DB2，该数据库是在 DB2 连接字符串的“初始目录”  属性中指定的，DB2 连接字符串可以在此过程后面介绍的 **“其他连接选项”** 字段中输入。  
  
8.  在 **“分发代理安全性”** 页上，单击订阅服务器旁边的属性按钮 ( **...** ) 来打开 **“分发代理安全性”** 对话框。  
  
9. 在 **“分发代理安全性”** 对话框中：  
  
    -   在 **“进程帐户”** 、 **“密码”** 和 **“确认密码”** 字段中输入运行分发代理的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户和密码，与分发服务器建立本地连接。  
  
         该帐户需要下列最低权限：分发数据库中 **db_owner** 固定数据库角色的成员、发布访问列表 (PAL) 的成员、快照共享上的读取权限、OLE DB 访问接口的安装目录上的读取权限。 有关 PAL 的详细信息，请参阅[保护发布服务器](../../relational-databases/replication/security/secure-the-publisher.md)。  
  
    -   在 **“连接到订阅服务器”** 下的 **“登录名”** 、 **“密码”** 和 **“确认密码”** 字段中，输入用于连接到订阅服务器的登录名和密码。 该登录名应该已配置好且应该具有足够的权限可以在订阅数据库中创建对象。  
  
    -   在 **“其他连接选项”** 字段中，以连接字符串的形式为订阅服务器指定任意连接选项（Oracle 不需要其他选项）。 应使用分号分隔每个选项。 下面是 DB2 连接字符串的示例（分行符是为了阅读方便）：  
  
        ```  
        Provider=DB2OLEDB;Initial Catalog=MY_SUBSCRIBER_DB;Network Transport Library=TCP;Host CCSID=1252;  
        PC Code Page=1252;Network Address=MY_SUBSCRIBER;Network Port=50000;Package Collection=MY_PKGCOL;  
        Default Schema=MY_SCHEMA;Process Binary as Character=False;Units of Work=RUW;DBMS Platform=DB2/NT;  
        Persist Security Info=False;Connection Pooling=True;  
        ```  
  
         字符串中的大多数选项都特定于正在配置的 DB2 服务器，但是应始终将“将二进制数作为字符处理”  选项设置为 **False**。 需要为“初始目录”  选项指定一个值以标识订阅数据库。  
  
10. 在 **“同步计划”** 页上，从 **“代理计划”** 菜单中为分发代理选择一个计划（计划通常为 **“连续运行”** ）。  
  
11. 在 **“初始化订阅”** 页上，指定是否应该初始化订阅。如果应该，指定何时初始化：  
  
    -   仅在已创建了所有对象并且在订阅数据库中添加了所有需要的数据时才能清除 **“初始化”** 。  
  
    -   在 **“初始化时间”** 列的下拉列表中选择 **“立即”** ，可以让分发代理在此向导完成后将快照文件传输到订阅服务器。 如果选择 **“首先同步”** ，代理则会在下次计划运行的时候传输文件。  
  
12. 在 **“向导操作”** 页上，为订阅编写脚本（可选）。 有关详细信息，请参阅 [Scripting Replication](../../relational-databases/replication/scripting-replication.md)。  
  
#### <a name="to-retain-tables-at-the-subscriber"></a>在订阅服务器上保留表  
  
-   默认情况下，为非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器启用发布会将 **pre_creation_cmd** 项目属性的值设置为“drop”。 此设置指定，如果复制与项目中某个表的名称相匹配，则复制应删除订阅服务器上对应的表。 如果要保留订阅服务器上的现有表，则可以对每个项目使用 [sp_changearticle](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) 存储过程，并为 **pre_creation_cmd** 指定值“none”。 `sp_changearticle @publication= 'MyPublication', @article= 'MyArticle', @property='pre_creation_cmd', @value='none'` 列中的一个值匹配。  
  
#### <a name="to-generate-a-snapshot-for-the-publication"></a>为发布生成快照  
  
1.  展开 **“复制”** 文件夹，再展开 **“本地发布”** 文件夹。  
  
2.  右键单击发布，再单击 **“查看快照代理状态”** 。  
  
3.  在“查看快照代理状态 - \<Publication>”对话框中，单击“启动”。   
  
 快照代理生成快照后，将显示一条消息，如“[100%] 已生成 17 个项目的快照”。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程以编程的方式创建对非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器的推送订阅。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
#### <a name="to-create-a-push-subscription-for-a-transactional-or-snapshot-publication-to-a-non-sql-server-subscriber"></a>创建对非 SQL Server 订阅服务器的事务发布或快照发布的推送订阅。  
  
1.  在发布服务器和分发服务器上安装非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器的最新 OLE DB 访问接口。 有关 OLE DB 访问接口的复制要求，请参阅 [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)、 [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md)和 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)。  
  
2.  在发布服务器的发布数据库中，通过执行 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) 验证发布是否支持非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器。  
  
    -   如果 **enabled_for_het_sub** 的值为 1，则支持非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器。  
  
    -   如果 **enabled_for_het_sub** 的值为 0，则执行 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)，并将 `@property` 指定为 enabled_for_het_sub，将 `@value` 指定为 true。  
  
        > [!NOTE]  
        >  在将 **enabled_for_het_sub** 更改为 **true** 之前，必须删除发布的任何现有订阅。 当发布还支持更新订阅时，无法将 **enabled_for_het_sub** 设置为 **true** 。 更改 **enabled_for_het_sub** 将影响其他发布属性。 有关详细信息，请参阅 [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)。  
  
3.  在发布服务器的发布数据库中，执行 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)。 指定 `@publication`、`@subscriber`、指定 `@destination_db` 的 `(default destination)` 值、`@subscription_type` 的推送值、并将 `@subscriber_type` 的值指定为 3 （指定 OLE DB 提供程序）。  
  
4.  在发布服务器的发布数据库中，执行 [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)。 指定下列各项：  
  
    -   `@subscriber` 和 `@publication` 参数。  
  
    -   `@subscriber_db` 的 **（默认目标）** 值，  
  
    -   `@subscriber_provider`、`@subscriber_datasrc`、`@subscriber_location`、`@subscriber_provider_string`、`@subscriber_catalog` 的非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据源的属性。  
  
    -   分发服务器中的分发代理运行时所使用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 凭据：`@job_login` 和 `@job_password`。  
  
       > [!NOTE]  
       > 使用 Windows 集成身份验证进行的连接始终使用由 `@job_login` 和 `@job_password` 指定的 Windows 凭据。 分发代理始终使用 Windows 集成身份验证与分发服务器建立本地连接。 默认情况下，该代理将使用 Windows 集成身份验证连接到订阅服务器。  
  
    -   为 `@subscriber_security_mode` 指定值 **0**，并为 `@subscriber_login` 和 `@subscriber_password` 指定 OLE DB 提供程序登录信息。  
  
    -   该订阅的分发代理作业计划。 有关详细信息，请参阅 [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md)。  
  
    > [!IMPORTANT]  
    >  用远程分发服务器在发布服务器上创建推送订阅时，为所有参数（包括 *job_login* 和 *job_password*）提供的值将以纯文本格式发送到分发服务器。 在执行此存储过程之前，应该对发布服务器及其远程分发服务器之间的连接进行加密。 有关详细信息，请参阅[启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
## <a name="see-also"></a>另请参阅  
 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)   
 [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md)   
 [其他非 SQL Server 订阅服务器](../../relational-databases/replication/non-sql/other-non-sql-server-subscribers.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [复制安全最佳做法](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
