---
title: 具有可用性组的 Analysis Services
description: 如果将 Always On 可用性组用作高可用性解决方案，可以将该组中的数据库用作 Analysis Services 表格或多维解决方案中的数据源。
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
ms.assetid: 14d16bfd-228c-4870-b463-a283facda965
author: cawrites
ms.author: chadam
manager: erikre
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c0e02d7491d9afce4611e0d22c488b438c858ed5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438880"
---
# <a name="analysis-services-with-always-on-availability-groups"></a>含有 AlwaysOn 可用性组的 Analysis Services

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  AlwaysOn 可用性组是预定义的 SQL Server 关系数据库集合；当有条件触发任一个数据库进行故障转移时，这些数据库将执行集体故障转移，使请求重定向到同一可用性组中其他实例上的镜像数据库。 如果将可用性组用作高可用性解决方案，您可以将该组中的数据库用作 Analysis Services 表格或多维解决方案中的数据源。 使用可用性数据库时，以下所有 Analysis Services 操作都能按预期工作：处理或导入数据、直接查询关系数据（使用 ROLAP 存储或 DirectQuery 模式）以及写回。  
  
 处理和查询属于只读工作负荷。 您可以通过将这些工作负荷转移到可读辅助副本来改善性能， 但这需要执行额外配置。 请使用本主题中的核对清单来确保遵循所有步骤。  
  
##  <a name="prerequisites"></a><a name="bkmk_prereq"></a>先决条件  
 您必须具有针对所有副本的 SQL Server 登录名。 必须具有 **sysadmin** 权限才能配置可用性组、侦听器和数据库；但用户只需要 **db_datareader** 权限即可从 Analysis Services 客户端访问数据库。  
  
 请使用支持表格格式数据流 (TDS) 协议版本 7.4 或更高版本的数据访问接口（如 SQL Server Native Client 11.0），或 .NET Framework 4.02 中用于 SQL Server 的数据访问接口。  
  
 **（仅限只读工作负荷）** 。 必须为只读连接配置辅助副本角色，可用性组必须具备路由列表，并且 Analysis Services 数据源中的连接必须指定可用性组侦听器。 本主题中提供了相关说明。  
  
##  <a name="checklist-use-a-secondary-replica-for-read-only-operations"></a><a name="bkmk_UseSecondary"></a> 核对清单：使用辅助副本执行只读操作  
 除非您的 Analysis Services 解决方案包括写回功能，否则您可以配置数据源连接使用可读的辅助副本。 如果您的网络连接速度很快，那么辅助副本的数据滞后时间很短，可提供与主副本几乎相同的数据。 通过将辅助副本用于 Analysis Services 操作，您可以减少主副本上的读写争用，并且更充分地利用可用性组中的辅助副本。  
  
 默认情况下，允许对主副本进行读写和读意向访问，同时不允许连接辅助副本。 需要进行额外配置，才能建立与辅助副本的只读客户端连接。 配置要求对辅助副本设置属性，并运行定义只读路由列表的 T-SQL 脚本。 请使用以下过程来确保已执行以上两个步骤。  
  
> [!NOTE]  
>  以下步骤假定存在 AlwaysOn 可用性组和数据库。 如果要配置新组，请使用“新建可用性组向导”创建该组并联接数据库。 该向导检查先决条件，为每一步提供指导，并执行初始同步。 有关详细信息，请参阅[使用可用性组向导 (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)。  
  
#### <a name="step-1-configure-access-on-an-availability-replica"></a>第 1 步：配置对可用性副本的访问  
  
1.  在对象资源管理器中，连接到承载主副本的服务器实例，然后展开服务器树。  
  
    > [!NOTE]  
    >  这些步骤来自[配置对可用性副本的只读访问 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)，为执行此任务提供了其他信息和说明。  
  
2.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
3.  单击要更改其副本的可用性组。 展开 **“可用性副本”** 。  
  
4.  右键单击次要副本，然后单击“属性”  。  
  
5.  在 **“可用性副本属性”** 对话框中，更改辅助角色的连接访问设置，如下所示：  
  
    -   在“可读次要副本”  下拉列表中，选择“仅读意向”  。  
  
    -   在 **“主角色中的连接”** 下拉列表中，选择 **“允许所有连接”** 。 这是默认值。  
  
    -   或者，在 **“可用性模式”** 下拉列表中，选择 **“同步提交”** 。 此步骤不是必需的，但设置它可以确保主副本与辅助副本之间存在数据奇偶校验。  
  
         计划的故障转移也需要该属性。 如果出于测试目的要执行计划的手动故障转移，请将主副本和辅助副本的 **“可用性模式”** 同时设置为 **“同步提交”** 。  
  
#### <a name="step-2-configure-read-only-routing"></a>第 2 步：配置只读路由  
  
1.  连接到主副本。  
  
    > [!NOTE]  
    >  这些步骤来自 [为可用性组配置只读路由 (SQL Server)](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)，为执行此任务提供了其他信息和说明。  
  
2.  打开一个查询窗口，在其中粘贴以下脚本。 此脚本执行三项操作：启用对辅助副本的可读连接（默认禁用该连接）、设置只读路由 URL，以及创建确定连接请求的定向先后顺序的路由列表。  如果已经在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]中设置了属性，则第一个语句（用于允许可读连接）是多余的，但为完整起见仍包括在该脚本中。  
  
    ```  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433'));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER02.contoso.com:1433'));  
  
    ALTER AVAILABILITY GROUP [AG1]   
    MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER02','COMPUTER01')));  
  
    ALTER AVAILABILITY GROUP [AG1]   
    MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER01','COMPUTER02')));  
    GO  
    ```  
  
3.  修改该脚本，用对您的部署有效的值取代占位符：  
  
    -   用承载主副本的服务器实例的名称取代“Computer01”。  
  
    -   用承载辅助副本的服务器实例的名称取代“Computer02”。  
  
    -   用所在域的名称取代“contoso.com”；如果所有计算机都位于相同的域中，则可在该脚本中省略该域。 如果侦听器使用的是默认端口，可保留该端口号。 侦听器实际使用的端口列在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]的属性页中。  
  
4.  执行脚本。  
  
     下一步，在使用您刚配置的组中的数据库的 Analysis Services 模型中创建数据源。  
  
##  <a name="create-an-analysis-services-data-source-using-an-always-on-availability-database"></a><a name="bkmk_ssasAODB"></a> 使用 AlwaysOn 可用性数据库创建 Analysis Services 数据源  
 本节说明如何创建连接到可用性组中的数据库的 Analysis Services 数据源。 您可以使用这些说明来配置与主副本（默认）的连接，或配置与基于上一节中的步骤配置的可读辅助副本的连接。 AlwaysOn 配置设置连同在客户端中设置的连接属性将共同决定是使用主要副本还是次要副本。  
  
1.  在 [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)] 的 Analysis Services 多维和数据挖掘模型项目中，右键单击“数据源”  并选择“新建数据源”  。 单击 **“新建”** 创建新的数据源。  
  
     或者，对于表格模型项目，单击“模型”菜单，然后单击 **“从数据源导入”** 。  
  
2.  在连接管理器的“访问接口”中，选择支持表格格式数据流 (TDS) 协议的访问接口。 SQL Server Native Client 11.0 支持此协议。  
  
3.  在连接管理器的“服务器名称”中，输入“可用性组侦听器”  的名称，然后选择该组中可用的数据库。  
  
     可用性组侦听器将客户端连接重定向到主副本以处理读写请求，或重定向到辅助副本（如果在连接字符串中指定了读意向）。 由于副本角色将在故障转移期间改变（此时主副本将变为辅助副本，辅助副本将变为主副本），所以您应该始终指定该侦听器，这样才能相应地重定向客户端连接。  
  
     要确定可用性组侦听程序的名称，可以询问数据库管理员，或连接到可用性组中的某个实例并查看其 AlwaysOn 可用性配置。   
  
4.  还是在连接管理器中，单击左侧导航窗格中的 **“全部”** ，查看数据访问接口的属性网格。  
  
     若要配置与次要副本的只读客户端连接，请将“应用程序意向”  设置为“READONLY”  。 否则，保持默认设置 **READWRITE** ，将连接重定向到主副本。  
  
5.  在“模拟信息”中，选择“使用特定 Windows 用户名和密码”  ，然后输入对数据库至少具有 **db_datareader** 权限的 Windows 域用户帐户。  
  
     不要选择 **“使用当前用户的凭据”** 或 **“继承”** 。 您可以选择 **“使用服务帐户”** ，但前提是该帐户对数据库具有读权限。  
  
     完成数据源设置并关闭数据源向导。  
  
6.  向连接字符串添加 **MultiSubnetFailover=Yes** ，以便更快地检测和连接到活动服务器。 有关此属性的详细信息，请参阅[高可用性和灾难恢复的 SQL Server Native Client Support](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)。  
  
     此属性不显示在属性网格中。 要添加该属性，请右键单击数据源并选择“查看代码”  。 将 `MultiSubnetFailover=Yes` 添加到连接字符串。  
  
 现在已经定义了数据源。 接下来可以构建模型，可以从数据源视图入手；如果是表格模型，则可以创建关系。 在您需要从可用性数据库中检索数据时（例如在您准备处理或部署解决方案时），您可以测试配置以验证是否从辅助副本检索数据。  
  
##  <a name="test-the-configuration"></a><a name="bkmk_test"></a> 测试配置  
 在 Analysis Services 中配置了辅助副本并创建了数据源连接后，您可以确认处理和查询命令是否重定向到辅助副本。 还可以执行计划的手动故障转移，以验证适用于此应用场景的恢复计划。  
  
#### <a name="step-1-confirm-the-data-source-connection-is-redirected-to-the-secondary-replica"></a>第 1 步：确认数据源连接重定向到辅助副本  
  
1.  启动 SQL Server Profiler 并连接到承载辅助副本的 SQL Server 实例。  
  
     当跟踪运行时， **SQL:BatchStarting** 和 **SQL:BatchCompleting** 事件将显示从数据库引擎实例上执行的 Analysis Services 中发出的查询。 这些事件已默认选定，您只需启动跟踪即可。  
  
2.  在 [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)]中，打开 Analysis Services 项目或包含要测试的数据源连接的解决方案。 确保数据源指定的是可用性组侦听器而不是组中的实例。  
  
     此步骤非常重要， 如果指定服务器实例名称，则不会路由到辅助副本。  
  
3.  排列应用程序窗口，使 SQL Server Profiler 和 [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)] 并排显示。  
  
4.  部署该解决方案，并在部署完成后停止跟踪。  
  
     在跟踪窗口中，您应能看到来自应用程序 **Microsoft SQL Server Analysis Services** 的事件。 应能看到从承载辅助副本的服务器实例上的数据库中检索数据的 **SELECT** 语句，证明是通过该侦听器连接到辅助副本。  
  
#### <a name="step-2-perform-a-planned-failover-to-test-the-configuration"></a>第 2 步：执行计划的故障转移以测试配置  
  
1.  在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] 中，检查主副本和辅助副本，确保两个副本均配置为同步提交模式并且当前保持同步。  
  
     以下步骤假定将辅助副本配置为同步提交。  
  
     要验证同步，请打开与托管主要副本和次要副本的每个实例的连接，展开“数据库”文件夹，确保每个副本中的数据库名称旁边均附有“(已同步)”  和“(正在同步)”  。  
  
    > [!NOTE]  
    >  这些步骤来自 [执行可用性组的计划手动故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)，为执行此任务提供了其他信息和说明。  
  
2.  在 SQL Server Profiler 中，启动每个副本的跟踪并且并排查看跟踪。 在以下步骤中，您需要比较各个跟踪，确认用于从 Analysis Services 执行处理或查询的 SQL 查询从一个副本切换到另一个副本。  
  
3.  从 Analysis Services 内部执行处理或查询命令。 由于您已将数据源配置为只读连接，所以应能看到在辅助副本上执行该命令。  
  
4.  在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] 中，连接到辅助副本。  
  
5.  依次展开“Always On 高可用性”  节点和“可用性组”  节点。  
  
6.  右键单击要进行故障转移的可用性组，然后选择“故障转移”  命令。 这将启动故障转移可用性组向导。 使用该向导选择要将哪一副本用作新的主副本。  
  
7.  确认故障转移已成功：  
  
    -   在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]中，展开可用性组以查看所指定的主副本和辅助副本。 以前充当主副本的实例现在应该变为辅助副本。  
  
    -   查看面板，确定是否检测到任何运行状况问题。 右键单击可用性组，然后选择“显示面板”  。  
  
8.  花一两分钟的时间等待故障转移在后端完成。  
  
9. 在 Analysis Services 解决方案中重复处理或查询命令，然后在 SQL Server Profiler 中并排观察跟踪状况。 您应能发现在另一个实例（即现在的新辅助副本）上执行处理的证据。  
  
##  <a name="what-happens-after-a-failover-occurs"></a><a name="bkmk_whathappens"></a> 故障转移之后的情形  
 在故障转移期间，辅助副本转换为主角色，以前的主副本转换为辅助角色。 所有客户端连接都已终止，可用性组侦听器的所有权随主副本角色移至新的 SQL Server 实例，并且该侦听器终结点绑定到新实例的虚拟 IP 地址和 TCP 端口。 有关详细信息，请参阅本主题后面的 [关于对可用性副本的客户端连接访问 (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)之类的系统数据库。  
  
 如果执行过程中出现故障转移，Analysis Services 中会出现以下错误并显示在日志文件或输出窗口中：“OLE DB 错误: OLE DB 或 ODBC 错误通讯链接失败; 08S01; TPC 提供程序: 现有连接已被远程主机强行关闭。 ; 08S01。”  
  
 稍等片刻然后重试，该错误应自行解决。 如果为可读辅助副本正确配置了可用性组，那么在您重试处理后，处理将在新的辅助副本上继续执行。  
  
 若错误反复出现，则很可能是因为配置有误。 您可以尝试重新运行 T-SQL 脚本，以解决辅助副本上可能存在的路由列表、只读路由 URL 和读意向问题。 还应该确认主副本允许所有连接。  
  
##  <a name="writeback-when-using-an-always-on-availability-database"></a><a name="bkmk_writeback"></a> 使用 AlwaysOn 可用性数据库时执行写回  
 写回是 Analysis Services 中用来支持 Excel 中的假设分析的一项功能。 该功能还常用于自定义应用程序中的预算和预测任务。  
  
 要支持写回功能，需要 READWRITE 客户端连接。 在 Excel 中，如果在只读连接上尝试写回，将出现以下错误：“无法从外部数据源检索数据。” “无法从外部数据源检索数据。”  
  
 如果配置某个连接始终访问只读辅助副本，您现在必须配置一个使用 READWRITE 连接访问主副本的新连接。  
  
 为此，应该在 Analysis Services 模型中另外创建一个数据源，以便支持读写连接。 创建另一个数据源时，请使用你在只读连接中指定的相同的侦听器名称和数据库，但应保留支持 READWRITE 连接的默认设置，而不要修改“应用程序意向”  。 您现在可以向基于读写数据源的数据源视图添加新的事实表或维度表，然后针对新表启用写回功能。  
  
## <a name="see-also"></a>另请参阅  
 [可用性组侦听程序、客户端连接和应用程序故障转移 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [活动次要副本：可读次要副本（AlwaysOn 可用性组）](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [针对 AlwaysOn 可用性组运行问题的 AlwaysOn 策略 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)   
 [创建数据源（SSAS 多维）](/analysis-services/multidimensional-models/create-a-data-source-ssas-multidimensional)   
 [启用维度写回](/analysis-services/multidimensional-models/bi-wizard-enable-dimension-writeback)  
  
