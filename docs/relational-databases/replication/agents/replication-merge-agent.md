---
title: 复制合并代理 | Microsoft Docs
description: 复制合并代理将数据库表中保存的初始快照应用于订阅服务器、合并增量数据更改，并协调冲突。
ms.custom: ''
ms.date: 10/29/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Merge Agent, executables
- Merge Agent, parameter reference
- agents [SQL Server replication], Merge Agent
- command prompt [SQL Server replication]
ms.assetid: fe1e7f60-b0c8-45e9-a5e8-4fedfa73d7ea
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4053b827b51eda8f238e6cada863773d625d1f59
ms.sourcegitcommit: 821e7039a342bf76306d66c61db247dc2caabc46
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "96999222"
---
# <a name="replication-merge-agent"></a>Replication Merge Agent
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  复制合并代理是一个将数据库表中保存的初始快照应用于订阅服务器的实用工具可执行文件。 它还合并自初始快照创建后发布服务器上发生的增量数据更改，并根据配置的规则或通过使用创建的自定义冲突解决程序来协调冲突。  
  
> [!NOTE]  
>  可以按任意顺序指定参数。 如果未指定可选参数，将使用本地计算机上预定义的注册表设置中的值。  
  
## <a name="syntax"></a>语法  
  
```  
  
replmerg [-?]   
-Publisher server_name[\instance_name]  
-PublisherDB publisher_database  
-Publication publication  
-Subscriber server_name[\instance_name]  
-SubscriberDB subscriber_database  
[-AltSnapshotFolder alt_snapshot_folder_path]  
[-Continuous]  
[-DefinitionFile def_path_and_file_name]  
[-DestThreads number_of_destination_threads]  
[-Distributor server_name[\instance_name]]  
[-DistributorLogin distributor_login]  
[-DistributorPassword distributor_password]  
[-DistributorSecurityMode [0|1]]  
[-DownloadGenerationsPerBatch download_generations_per_batch]  
[-DownloadReadChangesPerBatch download_read_changes_per_batch]  
[-DownloadWriteChangesPerBatch download_write_changes_per_batch]  
[-DynamicSnapshotLocation dynamic_snapshot_location]  
[-EncryptionLevel [0|1|2]]  
[-ExchangeType [1|2|3]]  
[-FastRowCount [0|1]]  
[-FileTransferType [0|1]]  
[-ForceConvergenceLevel [0|1|2 (Publisher|Subscriber|Both)]]  
[-FtpAddress ftp_address]  
[-FtpPassword ftp_password]  
[-FtpPort ftp_port]  
[-FtpUserNameftp_user_name]  
[-HistoryVerboseLevel [0|1|2|3]]  
[-Hostname host_name]  
[-InteractiveResolution [0|1]]  
[-InternetLogin internet_login]  
[-InternetPassword internet_password]  
[-InternetProxyLogin internet_proxy_login]  
[–InternetProxyPassword internet_proxy_password]  
[-InternetProxyServer internet_proxy_server]  
[-InternetSecurityMode [0|1]]  
[-InternetTimeout internet_timeout]  
[-InternetURL internet_url]  
[-KeepAliveMessageInterval keep_alive_message_interval_seconds]  
[-LoginTimeOut login_time_out_seconds]  
[-MakeGenerationInterval make_generation_interval_seconds]  
[-MaxBcpThreads number_of_threads]  
[-MaxDownloadChanges number_of_download_changes]  
[-MaxUploadChanges number_of_upload_changes]  
[-MetadataRetentionCleanup [0|1]]  
[-Output]  
[-OutputVerboseLevel [0|1|2]]  
[-ParallelUploadDownload [0|1]]  
[-PacketSize packet_size]   
[-PollingInterval polling_interval]  
[-ProfileName profile_name]  
[-PublisherFailoverPartner server_name[\instance_name] ]  
[-PublisherLogin publisher_login]  
[-PublisherPassword publisher_password]  
[-PublisherSecurityMode [0|1]]  
[-QueryTimeOut query_time_out_seconds]  
[-SrcThreads number_of_source_threads]  
[-StartQueueTimeout start_queue_timeout_seconds]  
[-SubscriberConflictClean [0|1]]  
[-SubscriberDatabasePath subscriber_path]  
[-SubscriberDBAddOption [0|1|2|3]]  
[-SubscriberLogin subscriber_login]  
[-SubscriberPassword subscriber_password   
[-SubscriberSecurityMode [0|1]]  
[-SubscriberType [0|1|2|3|4|5|6|7|8|9]]  
[-SubscriptionType [0|1|2]]  
[-SyncToAlternate [0|1]]  
[-T [101|102]]  
[-UploadGenerationsPerBatch upload_generations_per_batch]  
[-UploadReadChangesPerBatch upload_read_changes_per_batch]  
[-UploadWriteChangesPerBatch upload_write_changes_per_batch]  
[-UseInprocLoader]  
[-Validate [0|1|2|3]]  
[-ValidateInterval validate_interval]  
```  
  
## <a name="arguments"></a>参数  
 **-?**  
 输出所有可用的参数。  
  
 **-Publisher** _server_name_[ **\\** _instance_name_]  
 发布服务器的名称。 为该服务器上的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认实例指定 server_name。 为该服务器上的 _server_name_ **\\** _instance_name_ instance_name [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认实例指定 server_name。  
  
 **-PublisherDB** _publisher_database_  
 发布服务器数据库的名称。  
  
 **-Publication** _publication_  
 发布的名称。 只有将发布设置为总是使快照可用于新订阅或重新初始化的订阅时，此参数才有效。  
  
 **-Subscriber** _server_name_[ **\\** _instance_name_]  
 订阅服务器的名称。 为该服务器上的 *默认实例指定* server_name [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 为该服务器上的 _server_name_ **\\** _instance_name_ instance_name [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认实例指定 server_name。  
  
 **-SubscriberDB** _subscriber_database_  
 订阅服务器数据库的名称。  
  
 **-AltSnapshotFolder** _alt_snapshot_folder_path_  
 包含订阅的初始快照的文件夹路径。  
  
 **-Continuous**  
 指定代理是否尝试连续轮询已复制的事务。 如果指定了该参数，即使没有事务挂起，代理也将在轮询间隔期间轮询来自源的已复制事务。  
  
 **-DestThreads** _number_of_destination_threads_  
 指定合并代理用于在目标上应用更改的目标线程数。 在上载过程中，目标是发布服务器；在下载过程中，目标是订阅服务器。 默认值为 4。  
  
 **-DefinitionFile** _def_path_and_file_name_  
 代理定义文件的路径。 代理定义文件中包含代理的命令提示符参数。 文件的内容被当作可执行文件进行分析。 使用双引号 (") 指定包含任意字符的参数值。  
  
 **-Distributor** _server_name_[ **\\** _instance_name_]  
 分发服务器名称。 为该服务器上的 *默认实例指定* server_name [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 为该服务器上的 _server_name_ **\\** _instance_name_ instance_name [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认实例指定 server_name。 对于分发服务器（推送）分发，名称默认为本地计算机上 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的默认实例的名称。  
  
 **-DistributorLogin** _distributor_login_  
 分发服务器登录名。  
  
 **-DistributorPassword** _distributor_password_  
 分发服务器密码。  
  
 -DistributorSecurityMode [ 0\| 1]    
 指定分发服务器的安全模式。 值 **0** 指示 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证模式（默认设置），值 **1** 指示 Windows 身份验证模式。  
  
 **-DownloadGenerationsPerBatch** _download_generations_per_batch_  
 将更改从发布服务器下载到订阅服务器时要在单个批次中处理的生成数。 生成的定义是每个项目中属于一个逻辑组的更改。 可靠的通信链接的默认值为 100。 不可靠的通信链接的默认值为 10。  
  
 **-DownloadReadChangesPerBatch** _download_read_changes_per_batch_  
 将发布服务器上的更改下载到订阅服务器时要在单个批次中读取的更改数。 默认值为 100。  
  
 **-DownloadWriteChangesPerBatch** _download_write_changes_per_batch_  
 将发布服务器上的更改下载到订阅服务器时要在单个批次中应用的更改数。 默认值为 100。  
  
 **-DynamicSnapshotLocation** _dynamic_snapshot_location_  
 当发布使用参数化行筛选器时所筛选数据快照文件的位置。  
  
 -EncryptionLevel [ 0 \| 1 \| 2 ]     
 是合并代理在建立连接时使用的传输层安全性 (TLS)（旧称为“安全套接字层 (SSL)”）加密的级别。  
  
|EncryptionLevel 值|说明|  
|---------------------------|-----------------|  
|**0**|指定不使用 TLS。|  
|**1**|指定使用 TLS，但代理不验证 TLS/SSL 服务器证书是否由受信任的颁发者进行签名。|  
|**2**|指定使用 TLS，并验证证书。|  

 > [!NOTE]  
 >  有效的 TLS/SSL 证书是使用 SQL Server 的完全限定的域名进行定义。 为了在将 -EncryptionLevel 设置为 2 时成功连接代理，请在本地 SQL Server 上创建别名。 “Alias Name”参数应为服务器名称，“Server”参数应设置为 SQL Server 的完全限定名称。

 有关详细信息，请参阅[查看和修改复制安全设置](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)。  
  
 -ExchangeType [ 1\| 2\| 3]     
> [!WARNING]
>  [!INCLUDE[ssNoteDepFutureDontUse](../../../includes/ssnotedepfuturedontuse-md.md)] 要限制上载，请改用 sp_addmergearticle 的 \@subscriber_upload_options 。  
  
 指定同步过程中数据交换的类型，可以是下列值之一：  
  
|ExchangeType 值|说明|  
|------------------------|-----------------|  
|**1**|代理应将订阅服务器上的数据更改上载到发布服务器。|  
|**2**|代理应将发布服务器上的数据更改下载到订阅服务器。|  
|**3** （默认值）|代理应首先将订阅服务器上的数据更改上载到发布服务器，然后将发布服务器上的数据更改下载到订阅服务器。 您必须将此选项与 Web 同步一起使用。|  
  
 仅下载类型的项目使得您可以控制发布中单个项目的同步行为并可提高性能。 有关详细信息，请参阅[使用仅下载项目优化合并复制性能](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md)。  
  
 若要使用 **ExchangeType** 将合并复制的上载和下载阶段分隔为单独的会话，则必须先在将 **ExchangeType** 设为 1 的情况下运行合并代理，然后将该值设为 2 再运行一次合并代理。 不以这两个参数运行合并代理将导致系统删除元数据并需要您重新初始化订阅（不上载）。  
  
 **-FastRowCount** [**0**\|**1**]  
 指定应为行计数验证使用何种行计数计算类型。 值 **1** （默认值）表示快速方法。 值 **0** 表示完全行计数方法。  
  
 **-FileTransferType** [**0**\|**1**]  
 指定文件传输类型。 值为 **0** ，表示 UNC（通用命名约定），值为 1，表示 FTP（文件传输协议）。  
  
 -ForceConvergenceLevel [0\|1\|2 ( Publisher\| Subscriber\| Both)]        
 指定合并代理应使用的收敛级别，可以为以下值之一：  
  
|ForceConvergenceLevel 值|说明|  
|---------------------------------|-----------------|  
|**0** （默认值）|默认。 执行不具有附加收敛的标准合并。|  
|**1**|强制所有生成进行收敛。|  
|**2**|强制所有生成进行收敛并更正损坏的沿袭。 当指定此值时，请指定应更正何处的沿袭：发布服务器、订阅服务器，还是发布服务器和订阅服务器。|  
  
 **-FtpAddress** _ftp_address_  
 分发服务器的 FTP 服务网络地址。 如果不指定，则使用 **Distributor** 。  
  
 **-FtpPassword** _ftp_password_  
 用于连接到 FTP 服务的用户密码。  
  
 **-FtpPort** _ftp_port_  
 分发服务器 FTP 服务的端口号。 如果不指定，则使用 FTP 服务的默认端口号 (21)。  
  
 **-FtpUserName** _ftp_user_name_  
 用于连接到 FTP 服务的用户名。 如果不指定，则使用 anonymous。  
  
 **-HistoryVerboseLevel** [**1**\|**2**\|**3**]  
 指定在合并操作期间记录的历史记录数量。 选择 **1** 可将历史日志记录对性能的影响减至最小。  
  
|HistoryVerboseLevel 值|说明|  
|-------------------------------|-----------------|  
|**0**|记录最终的代理状态消息、最终的会话详细信息和任何错误。|  
|**1**|记录每个会话状态的增量会话详细信息，包括完成百分比、最终代理状态消息、最终会话详细信息以及任何错误。|  
|**2**|默认。 记录每个会话状态的增量会话详细信息和项目级别会话详细信息，包括完成百分比、最终代理状态消息、最终会话详细信息以及任何错误。 同时还会记录代理状态消息。|  
|**3**|除了将更多地记录代理进度消息之外，其他与 **-HistoryVerboseLevel** = **2** 时相同。|  
  
 **-Hostname** _host_name_  
 本地计算机的网络名。 默认为本地计算机名称。  
  
 **-InteractiveResolution** [**0**\|**1**]  
 指定在同步期间发生冲突时是否使用交互式冲突解决方法。 默认值为 **0**，指示不使用交互式冲突解决方法。  
  
 **-InternetLogin** _internet_login_  
 指定连接到需要进行身份验证的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制侦听器 ISAPI DLL 时所使用的登录名。  
  
 **-InternetPassword** _internet_password_  
 指定连接到需要进行身份验证的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制侦听器 ISAPI DLL 时所使用的密码。  
  
 **-InternetProxyLogin**  *internet_proxy_login*  
 指定连接到需要进行身份验证的代理服务器（在 *internet_proxy_server* 中定义）时所使用的登录名。  
  
 **–InternetProxyPassword**  *internet_proxy_password*  
 指定连接到需要进行身份验证的代理服务器（在 *internet_proxy_server* 中定义）时所使用的密码。  
  
 **-InternetProxyServer**  *internet_proxy_server*  
 指定当访问 *internet_url* 中指定的 HTTP 资源时要使用的代理服务器。  
  
 **-InternetSecurityMode** [**0**\|**1**]  
 指定在 Web 同步期间连接到 Web 服务器时所使用的 IIS 安全模式。 值为 **0** ，表示为基本身份验证，值为 **1** ，表示为 Windows 集成身份验证（默认）。  
  
 **-InternetTimeout** _internet_timeout_  
 与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制侦听器 ISAPI DLL 的连接超时之前等待的秒数。  
  
 **-InternetURL** _internet_url_  
 指定用于连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复制侦听器 ISAPI DLL 的 URL。 必须指定此属性。  
  
 **-KeepAliveMessageInterval** _keep_alive_message_interval_seconds_  
 在历史记录线程检查目前是否有连接在等待服务器响应之前等待的秒数。 在执行长时间运行的批处理时，减小该值可避免检查代理将合并代理标记为可疑。 默认值为 **300** 秒。  
  
 **-LoginTimeOut** _login_time_out_seconds_  
 登录超时前等待的秒数。 默认值为 15 秒。  
  
 **-MakeGenerationInterval** _make_generation_interval_seconds_  
 等待创建生成或更改批的秒数或下载到客户端的秒数。 默认值为 **1** 秒。  
  
 Makegeneration 是准备要下载到订阅服务器的发布服务器更改的进程，这可能是下载期间的性能瓶颈。 如果 makegeneration 进程已按 **-MakeGenerationInterval** 指定的间隔运行，则将为当前同步会话跳过该进程。 此操作将有助于同步并发，在订阅服务器不希望下载更改时尤为有用。  
  
 **-MaxBcpThreads** _number_of_threads_  
 指定可以并行执行的大容量复制操作的数量。 同时存在的线程和 ODBC 连接的最大数目为 **MaxBcpThreads** 与在发布数据库的系统表 **sysmergeschemachange** 中列出的大容量复制请求数这二者中的较小者。 **MaxBcpThreads** 的值必须大于 0，并且不存在任何硬编码的上限。 默认值为 **1**。  
  
 **-MaxDownloadChanges** _number_of_download_changes_  
 指定应从发布服务器下载到订阅服务器的已更改行的最大数量。 下载的行数可能会由于以下原因大于指定的最大值：处理了所有生成或可能运行了并行的目标线程，这两种情况在第一次传递中均会处理至少 100 个更改。 默认情况下将发送所有已准备好下载的更改。  
  
 **-MaxUploadChanges** _number_of_upload_changes_  
 指定应从订阅服务器上载到发布服务器的已更改行的最大数量。 上载的行数可能会由于以下原因大于指定的最大值：处理了所有生成或可能运行了并行的目标线程，这两种情况在第一次传递中均会处理至少 100 个更改。 默认情况下将发送所有已准备好上载的更改。  
  
 **-MetadataRetentionCleanup** [**0**\|**1**]  
 指定是否基于发布的保持期从 [MSmerge_genhistory](../../../relational-databases/system-tables/msmerge-genhistory-transact-sql.md)、 [MSmerge_contents](../../../relational-databases/system-tables/msmerge-contents-transact-sql.md)、 [MSmerge_tombstone](../../../relational-databases/system-tables/msmerge-tombstone-transact-sql.md)、 [MSmerge_past_partition_mappings](../../../relational-databases/system-tables/msmerge-past-partition-mappings-transact-sql.md)和 [MSmerge_current_partition_mappings](../../../relational-databases/system-tables/msmerge-current-partition-mappings.md) 中删除元数据。 默认值为 **1**，指示应执行清除操作。 值为 **0** 指示不应自动执行清除操作。  
  
 **-Output** _output_path_and_file_name_  
 代理输出文件的路径。 如果未提供文件名，则向控制台发送该输出。 如果指定的文件名已存在，会将输出追加到该文件。  
  
 **-OutputVerboseLevel** [**0**\|**1**\|**2**]  
 指定输出是否应提供详细内容。 如果详细级别为 0，则只输出错误消息。 如果详细级别为 **1**，则输出所有的进度报告消息。 如果详细级别为 2（默认），则输出所有错误消息和进度消息，这对调试很有帮助。  
  
 **-ParallelUploadDownload** [**0**\|**1**]  
 指定合并代理是否应并行处理上载到发布服务器和下载到订阅服务器的更改，这对于具有高带宽的大容量环境很有用。 不过，将 ParallelUploadDownload 设置为 1 时，请注意下面的警告 。
 
此参数已加入可能很快弃用的参数的列表。 建议将 ParallelUploadDownload 参数设置为 0，并避免使用“大容量服务器到服务器”合并代理配置文件，因为此配置文件中已将 ParallelUploadDownload 设置为 1 。

> [!WARNING]
>  [!INCLUDE[ssNoteDepFutureDontUse](../../../includes/ssnotedepfuturedontuse-md.md)]
  
 **-PacketSize**  
 数据包大小（按字节计）。 默认值为 4096（字节）。  
  
 **-PollingInterval** _polling_interval_  
 查询发布服务器或订阅服务器有无数据更改的频率（以秒为单位）。 默认值为 60 秒。  
  
 **-ProfileName** _profile_name_  
 指定用于代理参数的代理配置文件。 如果 **ProfileName** 为 NULL，则将禁用代理配置文件。 如果未指定 **ProfileName** ，则使用该代理类型的默认配置文件。 有关信息，请参阅[复制代理配置文件](../../../relational-databases/replication/agents/replication-agent-profiles.md)。  
  
 **-PublisherFailoverPartner** _server_name_[ **\\** _instance_name_]  
 指定参加与发布数据库进行的数据库镜像会话的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移伙伴实例。 有关详细信息，请参阅 [数据库镜像和复制 (SQL Server)](../../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)。  
  
 **-PublisherLogin** _publisher_login_  
 发布服务器登录名。 如果 **PublisherSecurityMode** 为 **0** （对于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证），则必须指定此参数。  
  
 **-PublisherPassword** _publisher_password_  
 发布服务器密码。 如果 **PublisherSecurityMode** 为 **0** （对于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证），则必须指定此参数。  
  
 **-PublisherSecurityMode** [**0**\|**1**]  
 指定发布服务器的安全模式。 值 **0** 指示 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证（默认值），值 **1** 指示 Windows 身份验证模式。  
  
 **-QueryTimeOut** _query_time_out_seconds_  
 查询超时前等待的秒数。默认值为 300 秒。 当此值大于 1800 时，合并代理还将使用 **QueryTimeout** 的值来确定等待生成分区快照的时间。  
  
 **-SrcThreads** _number_of_source_threads_  
 指定合并代理用于枚举来自源的更改的源线程数。 在上载过程中，源是订阅服务器；在下载过程中，源是发布服务器。 默认值为 **3**。  
  
 **-StartQueueTimeout** _start_queue_timeout_seconds_  
 当运行的并发合并进程数达到由 sp_addmergepublication 的 \@max_concurrent_merge 属性设置的限制时，合并代理等待的最大秒数 。 如果在达到最大秒数之后合并代理仍在等待，则合并代理将退出。 值 0 表示代理将无限期地等待，但是可以将其取消。  
  
 **-SubscriberDatabasePath** _subscriber_database_path_  
 如果 **SubscriberType** 为 **2** （允许建立到 Jet 数据库的连接，而无需 ODBC 数据源名称 (DSN)），则为 Jet 数据库（.mdb 文件）的路径。  
  
 -SubscriberDBAddOption [0\| 1\| 2\| 3]      
 指定是否存在现有的订阅服务器数据库。  
  
|SubscriberDBAddOption 值|说明|  
|---------------------------------|-----------------|  
|**0**|使用现有数据库（默认值）。|  
|**1**|创建一个新的空订阅服务器数据库。|  
|**2**|创建一个新的数据库并将其附加到指定文件。|  
|**3**|创建一个新数据库，附加该数据库，并启用在文件中可能存在的所有订阅。|  
  
> [!NOTE]  
>  如果使用值 **2** 或 **3**，则必须在 **SubscriberDatabasePath** 选项中指定订阅服务器的数据库路径。  
  
 **-SubscriberLogin** _subscriber_login_  
 订阅服务器登录名。 如果 **SubscriberSecurityMode** 为 **0** （对于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证），则必须指定此参数。  
  
 **-SubscriberPassword** _subscriber_password_  
 订阅服务器密码。 如果 **SubscriberSecurityMode** 为 **0** （对于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证），则必须指定此参数。  
  
 -SubscriberSecurityMode [ 0\| 1]    
 指定订阅服务器的安全模式。 值 **0** 指示 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证（默认值），值 **1** 指示 Windows 身份验证模式。  
  
 -SubscriberConflictClean [ 0\| 1]    
 如果在同步期间清除了订阅服务器上的冲突表，则值 **1** 指示订阅服务器上的冲突表已清除。 此参数仅用于对使用分散冲突日志记录的发布的订阅。  
  
 -SubscriberType [ 0\| 1\| 3\| 4\| 5\| 6\| 7\| 8]          
 指定合并代理所使用的订阅服务器连接的类型。 仅支持将此参数设置为默认值 **0** 。  
  
 -SubscriptionType[ 0\| 1\| 2]     
 指定分发的订阅类型。 值为 **0** ，表示推送订阅（默认值），值为 **1** ，表示请求订阅，值为 **2** ，表示匿名订阅。  
  
 -SyncToAlternate [ 0\|1]   
 指定合并代理是否在订阅服务器和备用发布服务器之间进行同步。 值为 **1** 表示它是备用发布服务器。 默认值为 **0**。  
 
 -T [101\|102]   
 可实现更多合并代理附加功能的跟踪标志。 设置为值 101 可获取更多详细的日志记录信息，帮助确定合并复制同步过程的每个步骤所花费的时间。 值 102 与跟踪标志 101 写入相同的统计信息，不同的是会将信息写入 <Distribution server>..msmerge_history 表 。 通过使用 `-output` 和 `-outputverboselevel` 参数，在使用跟踪标志 101 时启用合并代理日志记录。  例如，将以下参数添加到合并代理，然后重启代理：`-T 101, -output, -outputverboselevel`。 
 
 **-UploadGenerationsPerBatch** _upload_generations_per_batch_  
 将订阅服务器上的更改上载到发布服务器时要在单个批次处理的生成数。 生成的定义是每个项目中属于一个逻辑组的更改。 可靠的通信链接的默认值为 **100**。 不可靠的通信链接的默认值为 **1**。  
  
 **-UploadReadChangesPerBatch** _upload_read_changes_per_batch_  
 将订阅服务器上的更改上载到发布服务器时要在单个批次中读取的更改数。 默认值为 **100**。  
  
 **-UploadWriteChangesPerBatch** _upload_write_changes_per_batch_  
 将订阅服务器上的更改上载到发布服务器时要在单个批次中应用的更改数。 默认值为 **100**。  
  
 **-UseInprocLoader**  
 通过让合并代理在将快照文件应用于订阅服务器时使用 BULK INSERT 命令，提高初始快照的性能。 不推荐使用此参数，因为它与 XML 数据类型不兼容。 如果不复制 XML 数据，则可以使用此参数。 此参数不能用于字符模式快照。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 如果使用此参数，订阅服务器上的  服务帐户必须具有快照 .bcp 数据文件所在目录的读取权限。 如果不使用此参数，则代理加载的 ODBC 驱动程序将从文件中读取，从而不使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户的安全上下文。  
  
 **-Validate** [**0**\|**1**\|**2**\|**3**]  
 指定是否应在合并会话结束时执行验证，以及如果要执行验证，应执行哪种类型的验证。 建议值为 **3** 。  
  
|Validate 值|说明|  
|--------------------|-----------------|  
|**0** （默认值）|不执行验证。|  
|**1**|只验证行计数。|  
|**2**|验证行计数和校验和。|  
|**3**|验证行计数和二进制校验值。|  
  
> [!NOTE]  
>  如果订阅服务器与发布服务器上的数据类型不同，则使用二进制校验和或校验和进行的验证可能会错误地报告失败。 有关详细信息，请参阅[验证已复制的数据](../../../relational-databases/replication/validate-data-at-the-subscriber.md)的“数据验证的注意事项”部分。  
  
 **-ValidateInterval** _validate_interval_  
 在连续模式下对订阅进行验证的频率（单位为分钟）。 默认值为 **60** 分钟。  
  
## <a name="remarks"></a>备注  
  
> [!IMPORTANT]  
>  如果您安装的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理是通过本地系统帐户而非域用户帐户（默认值）运行，则该服务只能访问本地计算机。 如果以 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理身份运行的合并代理已配置为在登录到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]时使用 Windows 身份验证模式，则合并代理将失败。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认设置为  身份验证。  
  
 若要启动合并代理，请从命令提示符下执行 **replmerg.exe** 。 有关信息，请参阅 [复制代理可执行文件](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)。  
  
 ### <a name="troubleshooting-merge-agent-performance"></a>对合并代理性能进行故障排除 
 在连续模式下运行时，不删除当前会话的合并代理历史记录。 长期运行的代理可能导致合并历史记录表中包含大量条目，这会影响性能。 要解决此问题，请切换到计划模式，或继续使用连续模式但是创建一个专用作业来定期重新启动合并代理，或降低历史记录级别的详细程度以减少行数从而降低性能影响。  
 
  在某些情况下，复制合并代理可能需要较长时间才能复制更改。 若要确定合并复制同步过程中花费时间最多的步骤，请结合使用跟踪标志 101 和合并代理日志记录。 为此，请对合并代理参数使用以下参数，然后重启代理：  <br/>-T 101   <br/>-output   <br/>-outputverboselevel

此外，如果必须将统计信息写入 <Distribution server>..msmerge_history 表，请使用跟踪标志 -T 102。
  
## <a name="see-also"></a>另请参阅  
 [复制代理管理](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  
