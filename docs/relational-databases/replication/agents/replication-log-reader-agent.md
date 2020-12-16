---
title: 复制日志读取器代理 | Microsoft Docs
description: 复制日志读取器代理会监视为事务复制配置的 SQL Server 数据库，并将事务复制到分发数据库。
ms.custom: ''
ms.date: 10/29/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Log Reader Agent, executables
- Log Reader Agent, parameter reference
- agents [SQL Server replication], Log Reader Agent
- command prompt [SQL Server replication]
ms.assetid: 5487b645-d99b-454c-8bd2-aff470709a0e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: f5e716be586d7fb152a3c229cc016e349d57c99b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467348"
---
# <a name="replication-log-reader-agent"></a>复制日志读取器代理
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  复制日志读取器代理是一个可执行文件，用于监视为事务复制配置的每个数据库的事务日志，以及将标记为进行复制的事务从事务日志复制到分发数据库中。  
  
> [!NOTE]  
>  可以按任意顺序指定参数。 如果没有指定可选参数，会使用基于默认代理配置文件的预定义值。  
  
## <a name="syntax"></a>语法  
  
```  
  
logread [-?]   
-Publisher server_name[\instance_name]   
-PublisherDB publisher_database   
[-Continuous]  
[-DefinitionFile def_path_and_file_name]  
[-Distributor server_name[\instance_name]]  
[-DistributorLogin distributor_login]  
[-DistributorPassword distributor_password]  
[-DistributorSecurityMode [0|1]]  
[-EncryptionLevel [0|1|2]]  
[-ExtendedEventConfigFile configuration_path_and_file_name]  
[-HistoryVerboseLevel [0|1|2]]  
[-KeepAliveMessageInterval keep_alive_message_interval_seconds]  
[-LoginTimeOut login_time_out_seconds]  
[-LogScanThreshold scan_threshold]  
[-MaxCmdsInTran number_of_commands]  
[-MessageInterval message_interval]
[-MultiSubnetFailover [0|1]]
[-Output output_path_and_file_name]  
[-OutputVerboseLevel [0|1|2|3|4]]  
[-PacketSize packet_size]  
[-PollingInterval polling_interval]  
[-ProfileName profile_name]   
[-PublisherFailoverPartner server_name[\instance_name] ]  
[-PublisherSecurityMode [0|1]]  
[-PublisherLogin publisher_login]  
[-PublisherPassword publisher_password]   
[-QueryTimeOut query_time_out_seconds]  
[-ReadBatchSize number_of_transactions]   
[-ReadBatchThreshold read_batch_threshold]  
[-RecoverFromDataErrors]  
```  
  
## <a name="arguments"></a>参数  
 **-?**  
 显示用法信息。  
  
 **-Publisher** _server_name_[ **\\** _instance_name_]  
 发布服务器的名称。 为该服务器上的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认实例指定 server_name。 为该服务器上的 _server_name_ **\\** _instance_name_ instance_name [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认实例指定 server_name。  
  
 **-PublisherDB** _publisher_database_  
 发布服务器数据库的名称。  
  
 **-Continuous**  
 指定代理是否尝试持续轮询所复制的事务。 如果指定了该参数，即使没有事务挂起，代理轮询也将在轮询间隔期间轮询来自源的已复制事务。  
  
 **-DefinitionFile** _def_path_and_file_name_  
 代理定义文件的路径。 代理定义文件中包含代理的命令提示符参数。 文件的内容被当作可执行文件进行分析。 使用双引号 (") 指定包含任意字符的参数值。  
  
 **-Distributor** _server_name_[ **\\** _instance_name_]  
 分发服务器名称。 为该服务器上的 *默认实例指定* server_name [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 为该服务器上的 _server_name_ **\\** _instance_name_ instance_name [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认实例指定 server_name。  
  
 **-DistributorLogin** _distributor_login_  
 分发服务器登录名。  
  
 **-DistributorPassword** _distributor_password_  
 分发服务器密码。  
  
 -DistributorSecurityMode [ 0\| 1]    
 指定分发服务器的安全模式。 值为 **0** ，表示为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证模式（默认值），值为 **1** ，表示为 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 身份验证模式。  
  
 -EncryptionLevel [ 0 \| 1 \| 2 ]     
 是日志读取器代理在建立连接时使用的传输层安全性 (TLS)（旧称为“安全套接字层 (SSL)”）加密的级别。  
  
|EncryptionLevel 值|说明|  
|---------------------------|-----------------|  
|**0**|指定不使用 TLS。|  
|**1**|指定使用 TLS，但代理不验证 TLS/SSL 服务器证书是否由受信任的颁发者进行签名。|  
|**2**|指定使用 TLS，并验证证书。|  

 > [!NOTE]  
 >  有效的 TLS/SSL 证书是使用 SQL Server 的完全限定的域名进行定义。 为了在将 -EncryptionLevel 设置为 2 时成功连接代理，请在本地 SQL Server 上创建别名。 “Alias Name”参数应为服务器名称，“Server”参数应设置为 SQL Server 的完全限定名称。
 
 有关详细信息，请参阅[查看和修改复制安全设置](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)。  
  
 **-ExtendedEventConfigFile** _configuration_path_and_file_name_  
 为扩展事件 XML 配置文件指定路径和文件名。 通过该扩展事件 XML 配置文件，您可以配置会话并且启用事件跟踪。  
  
 -HistoryVerboseLevel [ 0\| 1\| 2]     
 指定在日志读取器运行期间记录的历史记录数量。 选择 1 可将历史日志记录对性能的影响减小到最低限度。  
  
|HistoryVerboseLevel 值|说明|  
|-------------------------------|-----------------|  
|**0**||  
|**1**|默认。 总是更新具有相同状态（启动、进行中、成功等）的上一历史记录消息。 如果不存在状态相同的上一记录，将插入新记录。|  
|**2**|除非记录为空闲消息或长时间运行的作业消息等信息（此时将更新上一记录），否则插入新的历史记录。|  
  
 **-KeepAliveMessageInterval** _keep_alive_message_interval_seconds_  
 在历史记录线程检查目前是否有连接在等待服务器响应之前等待的秒数。 在执行长时间运行的批处理时，减小该值可避免检查代理将日志读取器代理标记为可疑。 默认值为 300 秒。  
  
 **-LoginTimeOut** _login_time_out_seconds_  
 登录超时前等待的秒数。默认值为 15 秒。  
  
 **-LogScanThreshold** _scan_threshold_  
 仅限内部使用。  
  
 **-MaxCmdsInTran** _number_of_commands_  
 指定在日志读取器将命令写入到分发数据库时可分组到一个事务中的语句的最大数目。 如果使用此参数，在发布服务器上的大事务（包含许多命令）应用于订阅服务器时，日志读取器代理和分发代理可将这些大事务拆分为若干个较小的事务。 指定此参数可以减少分发服务器的争用问题并缩短发布服务器与订阅服务器之间的滞后时间。 由于初始事务是以较小的单元应用的，订阅服务器可以在初始事务结束之前访问一个较大的逻辑发布服务器事务的行，因而会破坏事务的原子性。 默认值为 0 ，这将保持发布服务器的事务边界。  
  
> [!NOTE]
>  对于非 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 发布，会忽略此参数。 有关详细信息，请参阅 [Performance Tuning for Oracle Publishers](../../../relational-databases/replication/non-sql/performance-tuning-for-oracle-publishers.md)的“配置事务集作业”部分。  
  
 **-MessageInterval** _message_interval_  
 用于历史日志记录的时间间隔。 记录完上一个历史事件后，如达到 **MessageInterval** 值，会开始记录新的历史事件。  
  
 如果源上没有可用的已复制事务，代理将向分发服务器报告无事务消息。 此选项可指定代理在报告另一条无事务消息前将等待多长时间。 在上次处理已复制事务后，如果代理在源上没有检测到任何可用的事务，则总是会报告一条无事务消息。 默认值为 60 秒。  
 
 -MultiSubnetFailover [0\|1] 指定是否启用 MultiSubnetFailover 属性。 如果你的应用程序要连接到不同子网上的 AlwaysOn 可用性组 (AG)，则将 MultiSubnetFailover 设置为 1 (true) 会加快检测（当前）活动服务器以及与服务器的连接。
  
 **-Output** _output_path_and_file_name_  
 代理输出文件的路径。 如果未提供文件名，则向控制台发送该输出。 如果指定的文件名已存在，会将输出追加到该文件。  
  
 -OutputVerboseLevel [ 0\| 1\| 2 \| 3 \| 4 ]       
 指定输出是否应提供详细内容。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|仅输出错误消息。|  
|**1**|输出所有代理进度报告消息。|  
|**2** （默认值）|输出所有错误消息和代理进度报告消息。|  
|**3**|输出每个复制的命令的前 100 个字节。|  
|**4**|输出所有复制的命令。|  
  
 进行调试时，值 2-4 颇为有用。  
  
 **-PacketSize** _packet_size_  
 数据包大小（按字节计）。 默认值为 4096（字节）。  
  
 **-PollingInterval** _polling_interval_  
 对日志进行已复制事务查询的频率（以秒计）。 默认值为 5 秒。  
  
 **-ProfileName** _profile_name_  
 指定用于代理参数的代理配置文件。 如果 **ProfileName** 为 NULL，则将禁用代理配置文件。 如果未指定 **ProfileName** ，则使用该代理类型的默认配置文件。 有关信息，请参阅[复制代理配置文件](../../../relational-databases/replication/agents/replication-agent-profiles.md)。  
  
 **-PublisherFailoverPartner** _server_name_[ **\\** _instance_name_]  
 指定参加与发布数据库进行的数据库镜像会话的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移伙伴实例。 有关详细信息，请参阅 [数据库镜像和复制 (SQL Server)](../../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)。  
  
 -PublisherSecurityMode [ 0\| 1]    
 指定发布服务器的安全模式。 值 **0** 指示 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证（默认值），值 **1** 指示 Windows 身份验证模式。  
  
 **-PublisherLogin** _publisher_login_  
 发布服务器登录名。  
  
 **-PublisherPassword** _publisher_password_  
 发布服务器密码。  
  
 **-QueryTimeOut** _query_time_out_seconds_  
 查询超时前等待的秒数。默认值为 1800 秒。  
  
 **-ReadBatchSize** _number_of_transactions_  
 每个处理周期从发布数据库的事务日志中读取的最大事务数目，默认值为 500，最多为 10000。 代理不断读取批次中的事务，直到从该日志中读取所有事务为止。 Oracle 发布服务器不支持该参数。  
  
 **-ReadBatchThreshold** _number_of_commands_  
 在复制命令由分发代理发送给订阅服务器之前，从事务日志读取的复制命令的数目。 默认值为 0。 如果未指定此参数，日志读取器代理会一直读取完此日志，或者读取到 **-ReadBatchSize** 中指定的数字（事务数）为止。  
  
 **-RecoverFromDataErrors**  
 指定日志读取器代理在从非 SQL Server 发布服务器发布的列数据中遇到错误时应继续运行。 默认情况下，这类错误可导致日志读取器代理失败。 在使用 **-RecoverFromDataErrors** 后，出错的列数据将复制为 NULL 或者适当的非 Null 值，并在 [MSlogreader_history](../../../relational-databases/system-tables/mslogreader-history-transact-sql.md) 表中记录警告消息。 仅 Oracle 发布服务器支持此参数。  
  
## <a name="remarks"></a>备注  
  
> [!IMPORTANT]  
>  如果您安装的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理是通过本地系统帐户而不是域用户帐户（默认值）运行，则该服务仅可访问本地计算机。 如果以 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理身份运行的日志读取器代理已配置为在登录到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]时使用 Windows 身份验证模式，则日志读取器代理将失败。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认设置为  身份验证。 有关更改安全帐户的信息，请参阅 [View and Modify Replication Security Settings](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)。  
  
 若要启动日志读取器代理，请从命令提示符下执行 **logread.exe** 。 有关信息，请参阅[复制代理可执行文件概念](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)。  
  
## <a name="change-history"></a>更改历史记录  
  
|更新的内容|  
|---------------------|  
|添加了 -ExtendedEventConfigFile 参数。|  
|添加了 -MultiSubnetFailover 参数。|
  
## <a name="see-also"></a>另请参阅  
 [复制代理管理](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  
