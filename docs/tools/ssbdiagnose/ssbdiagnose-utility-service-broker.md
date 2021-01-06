---
title: ssbdiagnose 实用工具 (Service Broker)
description: ssbdiagnose 实用工具可报告 Service Broker 会话或 Service Broker 服务配置中的问题。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- Service Broker, runtime reports
- Service Broker, command prompt utilities
- troubleshooting [Service Broker], conversations
- troubleshooting [Service Broker], configurations
- command prompt utilities [Service Broker]
- Service Broker, troubleshooting
- Service Broker, configuration reports
- Service Broker, tools
- troubleshooting [Service Broker], runtime
- conversations [Service Broker], troubleshooting
- troubleshooting [Service Broker], ssbdiagnose utility
- tools [Service Broker], ssbdiagnose
- Service Broker, ssbdiagnose utility
- ssbdiagnose
ms.assetid: 0c1636e8-a3db-438e-be4c-1ea40d1f4877
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 8590f3677d27c367782999877cb6892251785278
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641275"
---
# <a name="ssbdiagnose-utility-service-broker"></a>ssbdiagnose 实用工具 (Service Broker)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

**ssbdiagnose** 实用工具可报告 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会话或 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 服务配置中的问题。 可为两个服务或单个服务执行配置检查。 检查出的问题在命令提示符窗口以人工读取文本的形式报告，或输出为可重定向到文件或其他程序的格式化 XML。

## <a name="syntax"></a>语法  
  
```  
  
ssbdiagnose   
[ [ -XML ]  
    [ -LEVEL { ERROR | WARNING | INFO } ]  
  [-IGNORE error_id ] [ ...n]  
    [ <baseconnectionoptions> ]  
  { <configurationreport> | <runtimereport> }  
]  
| -?  
  
<configurationreport> ::=  
    CONFIGURATION  
  { [ FROM SERVICE service_name  
      [ <fromconnectionoptions> ]  
      [ MIRROR <mirrorconnectionoptions> ]  
    ]  
    [ TO SERVICE service_name[, broker_id ]  
      [ <toconnectionoptions> ]  
      [ MIRROR <mirrorconnectionoptions> ]  
    ]  
  }  
    ON CONTRACT contract_name  
  [ ENCRYPTION { ON | OFF | ANONYMOUS } ]  
  
<runtime_report> ::=  
    RUNTIME  
    [-SHOWEVENTS ]  
        [ -NEW  
         [ -ID { conversation_handle  
                | conversation_group_id  
                 | conversation_id  
                  }  
        ] [ ...n]  
        ]  
    [ -TIMEOUT timeout_interval ]  
    [ <runtimeconnectionoptions> ]  
  
<baseconnectionoptions> ::=  
  <connectionoptions>  
  
<fromconnectionoptions> ::=  
  <connectionoptions>  
  
<toconnectionoptions> ::=  
  <connectionoptions>  
  
<mirrorconnectionoptions> ::=  
  <connectionoptions>  
  
<runtimeconnectionoptions> ::=  
  [ CONNECT TO <connectionoptions> ] [ ...n]  
  
<connectionoptions> ::=  
    [ -E | { -U login_id [ -P password ] } ]  
  [ -S server_name[\instance_name] ]  
  [ -d database_name ]  
  [ -l login_timeout ]  
  
```  
  
## <a name="command-line-options"></a>命令行选项  
 **-XML**  
 指定将 **ssbdiagnose** 输出生成为格式化 XML。 此输出可重定向到一个文件或其他应用程序。 如果未指定 **-XML** ，则 **ssbdiagnose** 输出的格式为人工读取的文本。  
  
 **-LEVEL** { **ERROR** | **WARNING** | **INFO**}  
 指定要报告的消息的级别。  
  
 **ERROR**：仅报告错误消息。  
  
 **WARNING**：报告错误消息和警告消息。  
  
 **INFO**：报告错误消息、警告消息和信息性消息。  
  
 默认设置为 **WARNING**。  
  
 **-IGNORE** _error_id_  
 指定不在报告中包含具有指定 *error_id* 的错误或消息。 可以多次指定 **-IGNORE** 来禁止显示多个消息 ID。  
  
 **\<baseconnectionoptions>**  
 指定在特定子句中未包含连接选项时 **ssbdiagnose** 所使用的基本连接信息。 特定子句中给定的连接信息将覆盖 **baseconnectionoption** 信息。 各参数分别执行此选项。 例如，如果 **baseconnetionoptions** 中指定了 **-S** 和 **-d**，而 **toconnetionoptions** 中仅指定了 **-d**，则 **ssbdiagnose** 使用 **baseconnetionoptions** 中的 -S 和 **toconnetionoptions** 中的 -d。  
  
 **CONFIGURATION**  
 请求一对 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 服务之间或单个服务的配置错误报告。  
  
 **FROM SERVICE** _service_name_  
 指定启动会话的服务。  
  
 **\<fromconnectionoptions>**  
 指定连接到承载发起方服务的数据库所需的信息。 如果未指定 **fromconnectionoptions** ，则 **ssbdiagnose** 使用 **baseconnectionoptions** 中的连接信息来连接到发起方数据库。 如果指定了 **fromconnectionoptions** ，则它必须包括含有发起方服务的数据库。 如果未指定 **fromconnectionoptions** ，则 **baseconnectionoptions** 必须指定发起方数据库。  
  
 **TO SERVICE** _service_name_[, *broker_id* ]  
 指定作为会话目标的服务。  
  
 *service_name*：指定目标服务的名称。  
  
 *broker_id*：指定标识目标数据库的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] ID。 *broker_id* 是一个 GUID。 可在目标数据库中运行以下查询来查找该 ID：  
  
```  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID();  
```  
  
 **\<toconnectionoptions>**  
 指定连接承载目标服务的数据库所需的信息。 如果未指定 **toconnectionoptions** ，则 **ssbdiagnose** 使用 **baseconnectionoptions** 中的连接信息来连接到目标数据库。  
  
 **MIRROR**  
 指定关联的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 服务驻留在镜像数据库中。 **ssbdiagnose** 验证到该服务的路由是否为镜像路由，其中 MIRROR_ADDRESS 已在 CREATE ROUTE 中指定。  
  
 **\<mirrorconnectionoptions>**  
 指定连接到镜像数据库所需的信息。 如果未指定 **mirrorconnectionoptions** ，则 **ssbdiagnose** 使用 **baseconnectionoptions** 中的连接信息来连接到镜像数据库。  
  
 **ON CONTRACT** _contract_name_  
 请求 **ssbdiagnose** 仅检查使用指定协定的配置。 如果未指定 ON CONTRACT，则 **ssbdiagnose** 会将名为 DEFAULT 的协定作为报告对象。  
  
 **ENCRYPTION** { **ON** | **OFF** | **ANONYMOUS** }  
 请求检查是否为对话正确配置了指定的加密级别：  
  
 **ON**：默认设置。 配置了完全对话安全设置。 已为对话双方部署了证书，存在远程服务绑定，且针对目标服务的 GRANT SEND 语句指定了发起方用户。  
  
 **OFF**：未配置任何对话安全设置。 未部署证书，没有创建任何远程服务绑定，且针对发起方服务的 GRANT SEND 指定了 **公共** 角色。  
  
 **ANONYMOUS**：配置了匿名对话安全设置。 已部署了一个证书，远程服务绑定指定了匿名子句，且针对目标服务的 GRANT SEND 指定了 **公共** 角色。  
  
 **RUNTIME**  
 请求导致 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会话运行时错误的问题的报告。 如果 **-NEW** 和 **-ID** 都未指定，则 **ssbdiagnose** 将监视连接选项中指定的所有数据库中的所有会话。 如果指定了 **-NEW** 或 **-ID** ， **ssbdiagnose** 将生成参数中指定的 ID 列表。  
  
 **ssbdiagnose** 在运行状态下将记录指示运行时错误的所有 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 事件。 它会记录指定 ID 所发生的事件以及系统级事件。 如果遇到运行时错误， **ssbdiagnose** 会对相关配置运行配置报告。  
  
 默认情况下，输出报告中不包含运行时错误，而只包含对配置的分析结果。 使用 **-SHOWEVENTS** 可将运行时错误包含在报告中。  
  
 **-SHOWEVENTS**  
 指定 **ssbdiagnose** 报告 RUNTIME 报告期间出现的 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 事件。 仅报告视为错误情况的事件。 默认情况下， **ssbdiagnose** 只监视错误事件，而不在输出中报告这些事件。  
  
 **-NEW**  
 请求对 **ssbdiagnose** 开始运行后的第一个会话进行运行时监视。  
  
 **-ID**  
 请求对指定会话元素进行运行时监视。 可以多次指定 **-ID** 。  
  
 如果指定一个会话句柄，则仅报告与关联的会话端点相关的事件。 如果指定一个会话 ID，则会报告该会话及其发起方端点和目标端点的所有事件。 如果指定一个会话组 ID，则会报告该会话组中所有会话和端点的所有事件。  
  
 *conversation_handle*  
 标识应用程序中某个会话端点的唯一标识符。 每个会话端点的会话句柄都是唯一的，发起方端点和目标端点具有不同的会话句柄。  
  
 会话句柄由 BEGIN DIALOG  语句的 \@dialog_handle  参数以及 RECEIVE  语句结果集中的 conversation_handle  列返回到应用程序。  
  
 会话句柄在 **sys.transmission_queue** 和 **sys.conversation_endpoints** 目录视图的 **conversation_handle** 列中报告。  
  
 *conversation_group_id*  
 标识会话组的唯一标识符。  
  
 会话组 ID 由 GET CONVERSATION GROUP  语句的 \@conversation_group_id  参数以及 RECEIVE  语句结果集中的 conversation_group_id  列返回到应用程序。  
  
 会话组 ID 在 **sys.conversation_groups** 和 **sys.conversation_endpoints** 目录视图的 **conversation_group_id** 列中报告。  
  
 *conversation_id*  
 标识会话的唯一标识符。 会话的发起方端点和目标端点的会话 ID 是相同的。  
  
 会话 ID 在 **sys.conversation_endpoints** 目录视图的 **conversation_id** 列中报告。  
  
 **-TIMEOUT** _timeout_interval_  
 指定运行 **RUNTIME** 报告的秒数。 如果未指定 **-TIMEOUT** ，则运行时报告的运行时间不限。 **-TIMEOUT** 仅用于 **RUNTIME** 报告，而不用于 **CONFIGURATION** 报告。 如果未指定 -TIMEOUT 或要在超时间隔到期之前结束运行时报告，请按 Ctrl + C 退出 ssbdiagnose   **-** 。 *timeout_interval* 必须是介于 1 和 2,147,483,647 之间的数字。  
  
 **\<runtimeconnectionoptions>**  
 指定数据库的连接信息，该数据库包含与受监视的会话元素关联的服务。 如果所有服务都位于同一数据库中，则只需指定一个 **CONNECT TO** 子句。 如果各服务位于不同的数据库中，必须为每个数据库提供一个 **CONNECT TO** 子句。 如果未指定 **runtimeconnectionoptions** ，则 **ssbdiagnose** 使用 **baseconnectionoptions** 中的连接信息。  
  
 **-E**  
 将当前 Windows 帐户用作登录 ID 来打开指向 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例的 Windows 身份验证连接。 登录名必须是 **sysadmin** 固定服务器角色的成员。  
  
 使用 -E 选项将忽略 SQLCMDUSER 和 SQLCMDPASSWORD 环境变量的用户和密码设置。  
  
 如果 **-E** 和 **-U** 都未指定，则 **ssbdiagnose** 将使用 SQLCMDUSER 环境变量的值。 如果 SQLCMDUSER 也未设置，则 **ssbdiagnose** 将使用 Windows 身份验证。  
  
 如果将 **-E** 选项与 **-U** 选项或 **-P** 选项一起使用，将生成错误消息。  
  
 **-U** _login_id_  
 使用指定的登录 ID 打开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证连接。 登录名必须是 **sysadmin** 固定服务器角色的成员。  
  
 如果 **-E** 和 **-U** 都未指定，则 **ssbdiagnose** 将使用 SQLCMDUSER 环境变量的值。 如果 SQLCMDUSER 也未设置，则 **ssbdiagnose** 会尝试使用 Windows 身份验证模式，基于运行 **ssbdiagnose** 的用户的 Windows 帐户进行连接。  
  
 如果将 **-U** 选项与 **-E** 选项一起使用，将生成错误消息。 如果 -U  选项后跟多个参数，便会生成错误消息并退出程序。  
  
 **-P** _password_  
 指定 **-U** 登录 ID 的密码。 密码是区分大小写的。 如果使用了 **-U** 选项而未使用 **-P** 选项，则 **ssbdiagnose** 将使用 SQLCMDPASSWORD 环境变量的值。 如果 SQLCMDPASSWORD 也未设置，则 **ssbdiagnose** 会提示用户输入密码。  
  
> [!IMPORTANT]  
>  键入 SET SQLCMDPASSWORD 命令时，任何可以看到您的监视器的人均可看到密码。  
  
 如果指定了 **-P** 选项而未指定密码，则 **ssbdiagnose** 将使用默认密码 (NULL)。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]有关详细信息，请参阅[强密码](../../relational-databases/security/strong-passwords.md)。  
  
 通过向控制台输出密码提示，可以显示密码提示，如下所示： `Password:`  
  
 隐藏用户输入。 也就是说，将不会显示任何输入的内容，光标保留原位不动。  
  
 如果将 **-P** 选项与 **-E** 选项一起使用，将生成错误消息。  
  
 如果 **-P** 选项后跟多个参数，将生成错误消息。  
  
 **-S** _server_name_[\\*instance_name*]  
 指定承载要分析的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 服务的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 实例。  
  
 指定要连接到该服务器上 *默认实例的* server_name [!INCLUDE[ssDE](../../includes/ssde-md.md)] 。 指定要连接到该服务器上 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的命名实例的 _server\_name_\\  _instance\_name_。 如果未指定 **-S** ，则 **ssbdiagnose** 将使用 SQLCMDSERVER 环境变量的值。 如果 SQLCMDSERVER 也未设置，则 **ssbdiagnose** 将连接到本地计算机上的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的默认实例。  
  
 **-d** _database_name_  
 指定承载要分析的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 服务的数据库。 如果该数据库不存在，将生成错误消息。 如果未指定 **-d** ，则默认为登录帐户的默认数据库属性中指定的数据库。  
  
 **-l** _login_timeout_  
 指定尝试连接到服务器时，等待了多少秒后超时。如果未指定 **-l** ，则 **ssbdiagnose** 将使用为 SQLCMDLOGINTIMEOUT 环境变量设置的值。 如果 SQLCMDLOGINTIMEOUT 也未设置，则默认的超时值为 30 秒。 登录超时必须是介于 0 和 65534 之间的数字。 如果提供的值不是数值或不在此范围内，则 **ssbdiagnose** 将生成错误消息。 该值为 0 时，则允许无限制等待。  
  
 **-?**  
 显示命令行帮助。  
  
## <a name="remarks"></a>备注  
 使用 **ssbdiagnose** 可以执行下列操作：  
  
-   确认在新配置的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 应用程序中没有配置错误。  
  
-   确认在更改现有 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 应用程序的配置后没有配置错误。  
  
-   确认在将 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 数据库从 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例分离并重新附加到数据库引擎的新实例后没有配置错误。  
  
-   探查当消息不能在服务间成功传输时是否存在配置错误。  
  
-   获取一组 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 会话元素中出现的所有错误的报告。  
  
## <a name="configuration-reporting"></a>配置报告  
 若要正确分析会话所使用的配置，请运行所用选项与该会话所用选项相同的 **ssbdiagnose** 配置报告。 如果为 **ssbdiagnose** 指定的选项级别低于会话所使用的选项级别， **ssbdiagnose** 可能不报告会话所需的条件。 如果为 **ssbdiagnose** 指定的选项级别高于会话所使用的选项级别，则可能会报告会话并不需要的项。 例如，同一数据库中两个服务间的会话可在 ENCPRYPTION OFF 的情况下运行。 如果运行 **ssbdiagnose** 来验证这两个服务间的配置，但使用的是默认的 ENCRYPTION ON 设置，则 **ssbdiagnose** 将报告数据库缺少主密钥。 而主密钥并不是会话所需的。  
  
 **ssbdiagnose** 配置报告每次运行时只能分析一个 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 服务或单对服务。 若要对多对 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 服务生成报告，请生成一个 .cmd 命令文件，在其中多次调用 **ssbdiagnose** 。  
  
## <a name="runtime-reporting"></a>运行时报告  
 指定了 -RUNTIME 时， **ssbdiagnose** 将搜索 **runtimeconnectionoptions** 和 **baseconnectionoptions** 中指定的所有数据库以生成 [!INCLUDE[ssSB](../../includes/sssb-md.md)] ID 的列表。 生成的 ID 的完整列表取决于为 -NEW 和 -ID 指定的内容：  
  
-   如果 **-NEW** 和 **-ID** 都未指定，则该列表将包含连接选项中指定的所有数据库的所有会话。  
  
-   如果指定了 **-NEW** ，则 **ssbdiagnose** 将包含 **ssbdiagnose** 运行后开始的第一个会话的元素。 这些元素包括会话 ID 以及目标会话端点和发起方会话端点的会话句柄。  
  
-   如果指定 **-ID** 时提供了会话句柄，则只在列表中包含该句柄。  
  
-   如果指定 **-ID** 时提供了会话 ID，则该会话 ID 及其两个会话端点的句柄都会添加到列表中。  
  
-   如果指定 **-ID** 时提供了会话组 ID，则该组中的所有会话 ID 和会话句柄都会添加到列表中。  
  
 该列表不包含连接选项未涵盖的数据库中的元素。 例如，假定你使用 **-ID** 指定会话 ID，但只为发起方数据库提供 **runtimeconnectionoptions** 子句而不为目标数据库提供。 **ssbdiagnose** 将不在其 ID 列表中包含目标会话句柄，而只包含会话 ID 和发起方会话句柄。  
  
 **ssbdiagnose** 监视 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] runtimeconnectionoptions **和** baseconnectionoptions **涵盖的数据库中的** 事件。 它会搜索指示运行时列表中的一个或多个 [!INCLUDE[ssSB](../../includes/sssb-md.md)] ID 遇到错误的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 事件。 **ssbdiagnose** 还会搜索未专门与任何会话组关联的系统级 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 错误事件。  
  
 如果 **ssbdiagnose** 找到会话错误，则该实用工具也将通过运行配置报告来尝试报告导致事件的根本原因。 **ssbdiagnose** 使用数据库中的元数据来尝试确定会话所使用的实例、 [!INCLUDE[ssSB](../../includes/sssb-md.md)] ID、数据库、服务和协定。 然后它使用所有可用信息运行配置报告。  
  
 默认情况下， **ssbdiagnose** 不报告错误事件。 它只报告配置检查中找到的基础性问题。 这使报告的信息量减到最少，有助于您将注意力集中在基础配置问题上。 可以指定 **-SHOWEVENTS** 以查看 **ssbdiagnose** 遇到的错误事件。  
  
## <a name="issues-reported-by-ssbdiagnose"></a>ssbdiagnose 报告的问题  
 **ssbdiagnose** 报告三类问题。 在 XML 输出文件中，每类问题都作为 Issue 元素的一个单独类型来报告。 **ssbdiagnose** 报告的三类问题如下：  
  
 **诊断**  
 报告配置问题。 这包括运行 **CONFIGURATION** 报告时或在 **RUNTIME** 报告的配置阶段中发现的问题。 **ssbdiagnose** 对每个配置问题只报告一次。  
  
 **事件**  
 报告 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 事件，该事件指示 **RUNTIME** 报告期间监视的会话所遇到的问题。 **ssbdiagnose** 在每次生成事件时都进行报告。 如果多个会话都遇到相同问题，则会多次报告相关事件。  
  
 问题  
 报告导致 **ssbdiagnose** 无法完成配置分析或无法监视会话的问题。  
  
## <a name="sqlcmd-environment-variables"></a>sqlcmd 环境变量  
 **ssbdiagnose** 实用工具支持 **sqlcmd** 实用工具也使用的 SQLCMDSERVER、SQLCMDUSER、SQLCMDPASSWORD 和 SQLCMDLOGINTIMOUT 环境变量。 设置环境变量可以使用命令提示 SET 命令，也可以使用通过 **sqlcmd** 运行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本中的 **setvar** 命令。 有关如何在 **sqlcmd** 中使用 **setvar** 的详细信息，请参阅 [将 sqlcmd 与脚本变量结合使用](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)。  
  
## <a name="permissions"></a>权限  
 在每个 **connectionoptions** 子句中，通过 **-E** 或 **-U** 指定的登录名必须是 **-S** 所指定实例中 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 本节包含在命令提示符下使用 **ssbdiagnose** 的示例。  
  
### <a name="a-checking-the-configuration-of-two-services-in-the-same-database"></a>A. 检查同一数据库中两个服务的配置  
 下例说明当符合以下条件时如何请求配置报告：  
  
-   发起方服务和目标服务在同一个数据库中。  
  
-   该数据库在 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的默认实例中。  
  
-   该实例在运行 **ssbdiagnose** 的计算机上。  
  
 **ssbdiagnose** 实用工具将报告使用 DEFAULT 协定的配置，因为没有指定 ON CONTRACT。  
  
```  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE /test/initiator TO SERVICE /test/target  
```  
  
### <a name="b-checking-the-configuration-of-two-services-on-separate-computers-that-use-one-login"></a>B. 检查在不同计算机上使用一个登录名的两个服务的配置  
 下例说明在以下情况下如何请求配置报告：发起方服务和目标服务在不同计算机上，但可使用相同的 Windows 身份验证登录名进行访问。  
  
```  
ssbdiagnose -E CONFIGURATION FROM SERVICE /text/initiator -S InitiatorComputer -d InitiatorDatabase TO SERVICE /test/target -S TargetComputer -d TargetDatabase ON CONTRACT TestContract  
```  
  
### <a name="c-checking-the-configuration-of-two-services-on-separate-computers-that-use-separate-logins"></a>C. 检查在不同计算机上使用不同登录名的两个服务的配置  
 下例说明在以下情况下如何请求配置报告：发起方服务和目标服务在不同计算机上，且每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例需要不同的 [!INCLUDE[ssDE](../../includes/ssde-md.md)]身份验证登录名。  
  
```  
ssbdiagnose CONFIGURATION FROM SERVICE /text/initiator   
-S InitiatorComputer -U InitiatorLogin -p !wEx23Dvb   
-d InitiatorDatabase TO SERVICE /test/target -S TargetComputer   
-U TargetLogin -p ER!49jiy -d TargetDatabase ON CONTRACT TestContract  
```  
  
### <a name="d-checking-mirrored-service-configurations-on-separate-computers-with-anonymous-encryption"></a>D. 检查不同计算机上包括匿名加密情况在内的镜像服务配置  
 下例说明在以下情况下如何请求配置报告：发起方服务和目标服务在不同计算机上，且发起方镜像到命名实例。 该报告还检查服务是否配置为使用匿名加密。  
  
```  
ssbdiagnose -E CONFIGURATION FROM SERVICE /text/initiator   
-S InitiatorComputer -d InitiatorDatabase MIRROR   
-S MirrorComputer/MirrorInstance TO SERVICE /test/target   
-S TargetComputer -d TargetDatabase ON CONTRACT TestContract ENCRYPTION ANONYMOUS  
```  
  
### <a name="e-checking-the-configuration-of-two-contracts"></a>E. 检查两个约定的配置  
 下例说明当符合以下条件时如何生成请求配置报告的命令文件：  
  
-   发起方服务和目标服务在同一个数据库中。  
  
-   该数据库在 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的默认实例中。  
  
-   该实例在运行 **ssbdiagnose** 的计算机上。  
  
 **ssbdiagnose** 每次运行时都将报告相同服务间不同协定的配置。  
  
```  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target ON CONTRACT PayRaiseContract  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE /test/initiator   
TO SERVICE /test/target ON CONTRACT PromotionContract  
```  
  
### <a name="f-monitor-the-status-of-a-specific-conversation-on-the-local-computer-with-a-time-out"></a>F. 监视本地计算机上特定会话的状态，并设置超时  
 下例说明如何监视特定会话，其中发起方服务和目标服务在同一数据库中，该数据库在运行 **ssbdiagnose** 的计算机的默认实例中。 超时间隔设置为 20 秒。  
  
```  
ssbdiagnose -E -d TestDatabase RUNTIME -ID D68D77A9-B1CF-41BF-A5CE-279ABCAB140D -TIMEOUT 20  
```  
  
### <a name="g-monitor-the-status-of-a-conversation-that-spans-two-computers"></a>G. 监视跨越两台计算机的会话的状态  
 下例说明如何监视特定会话，其中发起方服务和目标服务在不同计算机上。  
  
```  
ssbdiagnose RUNTIME -ID D68D77A9-B1CF-41BF-A5CE-279ABCAB140D   
-TIMEOUT 10 CONNECT TO -E -S InitiatorComputer/InitiatorInstance   
-d InitiatorDatabase CONNECT TO -E -S TargetComputer/TargetInstance   
-d TargetDatabase  
```  
  
### <a name="h-monitor-the-status-of-a-conversation-in-two-databases-in-the-same-instance"></a>H. 监视同一实例的两个数据库中的会话的状态  
 下例说明如何监视特定会话，其中发起方服务和目标服务在同一 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的不同数据库中。 该示例使用 **baseconnectionoptions** 来指定实例和登录信息，使用两个 CONNECT TO 子句来指定数据库。 指定了 -SHOWEVENTS，因此所有运行时事件都包含在报告输出中。  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME -SHOWEVENTS   
-ID 5094d4a7-e38c-4c37-da37-1d58b1cb8455 -TIMEOUT 10 CONNECT TO   
-d InitiatorDatabase CONNECT TO -d TargetDatabase  
```  
  
### <a name="i-monitor-the-status-of-two-conversations-between-two-databases"></a>I. 监视两个数据库间的两个会话的状态  
 下例说明如何监视两个会话，其中发起方服务和目标服务在同一 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的不同数据库中。 该示例使用 **baseconnectionoptions** 来指定实例和登录信息，使用两个 CONNECT TO 子句来指定数据库。  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME   
-ID 5094d4a7-e38c-4c37-da37-1d58b1cb8455   
-ID 9b293be9-226b-4e22-e169-1d2c2c15be86 -TIMEOUT 10 CONNECT TO   
-d InitiatorDatabase CONNECT TO -d TargetDatabase  
```  
  
### <a name="j-monitor-the-status-of-all-conversations-between-two-databases"></a>J. 监视两个数据库间的所有会话的状态  
 下例说明如何监视同一 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例中两个数据库间的所有会话。 该示例使用 **baseconnectionoptions** 来指定实例和登录信息，使用两个 CONNECT TO 子句来指定数据库。  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME   
-TIMEOUT 10 CONNECT TO -d InitiatorDatabase CONNECT TO   
-d TargetDatabase  
```  
  
### <a name="k-ignore-specific-errors"></a>K. 忽略特定错误  
 下例说明如何忽略测试系统中当前激活配置方式中存在的已知错误（303 和 304）。  
  
```  
ssbdiagnose -IGNORE 303 -IGNORE 304 -E -d TestDatabase   
CONFIGURATION FROM SERVICE /test/initiator TO SERVICE /test/target   
ON CONTRACT TextContract  
```  
  
### <a name="l-redirecting-ssbdiagnose-xml-output"></a>L. 重定向 ssbdiagnose XML 输出  
 下例说明如何请求 **ssbdiagnose** 将其输出生成为重定向到一个文件的 XML 文件。 然后应用程序可打开 TestDiag.xml 文件以分析或报告 **ssbdiagnose** XML 文件。 或者，您还可以在常规 XML 编辑器（如 XML Notepad）中查看该 XML 文件。  
  
```  
ssbdiagnose -XML -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target > c:\MyDiagnostics\TestDiag.xml  
```  
  
### <a name="m-using-an-environment-variable"></a>M. 使用环境变量  
 以下示例首先设置 SQLCMDSERVER 环境变量使其包含服务器名称，然后运行未指定 **-S** 的 **ssbdiagnose**。  
  
```  
SET SQLCMDSERVER=MyComputer  
ssbdiagnose -XML -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [BEGIN DIALOG CONVERSATION (Transact-SQL)](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [CREATE BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE CONTRACT (Transact-SQL)](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md)   
 [CREATE MESSAGE TYPE (Transact-SQL)](../../t-sql/statements/create-message-type-transact-sql.md)   
 [CREATE QUEUE (Transact SQL)](../../t-sql/statements/create-queue-transact-sql.md)   
 [CREATE REMOTE SERVICE BINDING (Transact-SQL)](../../t-sql/statements/create-remote-service-binding-transact-sql.md)   
 [CREATE ROUTE (Transact SQL)](../../t-sql/statements/create-route-transact-sql.md)   
 [CREATE SERVICE (Transact SQL)](../../t-sql/statements/create-service-transact-sql.md)   
 [RECEIVE (Transact-SQL)](../../t-sql/statements/receive-transact-sql.md)   
 [sys.transmission_queue (Transact-SQL)](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md)   
 [sys.conversation_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)   
 [sys.conversation_groups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-groups-transact-sql.md)  
  
