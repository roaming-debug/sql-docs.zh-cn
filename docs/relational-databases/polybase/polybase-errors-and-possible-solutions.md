---
title: PolyBase 错误和可行解决方案
description: PolyBase 错误和建议解决方案参考。
ms.date: 03/22/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
dev_langs:
- TSQL
- XML
f1_keywords:
- PolyBase, monitoring
- PolyBase, performance monitoring
helpviewer_keywords:
- PolyBase, troubleshooting
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: ''
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-2016'
ms.openlocfilehash: 46f56288382c1b1928e6ad3081a8eea41e23bb5f
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833795"
---
# <a name="polybase-errors-and-possible-solutions"></a>PolyBase 错误和可行解决方案

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

本文包含适用于 PolyBase 的常见错误方案和解决方案。

有关 PolyBase 监视和故障排除的详细信息，请参阅[对 PolyBase 进行监视和故障排除](polybase-troubleshooting.md)。

有关 Windows 和 Linux 中常见的 PolyBase 日志文件位置，请参阅[对 PolyBase 进行监视和故障排除](polybase-troubleshooting.md#log-file-locations)。


## <a name="error-messages-and-possible-solutions"></a>错误消息和可能的解决方案

### <a name="error-100001failed-to-generate-query-plan"></a>错误：“100001;无法生成查询计划”

SQL Server 数据库引擎至少已修补至累积更新 8 (15.0.4073)，而 PolyBase 功能尚未更新到同一内部版本时，系统可能会出现“无法生成查询计划”错误。 将 PolyBase 功能添加到现有 SQL Server 实例时，可能会发生这种情况。 有关详细信息，请参阅 [PolyBase 错误 -“100001;无法生成查询计划”](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-error-100001-failed-to-generate-query-plan/ba-p/2174693)。

在安装 PolyBase 功能之后，务必要将新功能引入到相同版本级别。 根据需要安装服务包 (SP)、累积更新 (CU) 和/或常规分发版本 (GDR)。 要确定 PolyBase 的版本，请参阅[确定 SQL Server 及其组件的版本、版本类别和更新级别](/troubleshoot/sql/general/determine-version-edition-update-level#polybase)。

### <a name="service-account-change"></a>更改服务帐户

示例错误消息：

> 107035;由于 [DOMAIN\user] 不是组 [PdwDataMovementAccess] 成员，Dms 授权失败 <BR>
> 107017;无效 DMS 控制标头:

此错误原因可能是更改了 PolyBase 服务帐户。 若要更改 PolyBase 引擎和 PolyBase 数据移动服务的服务帐户，请卸载并重新安装 PolyBase 功能。


### <a name="data-movement-service-permissions-errors"></a>数据移动服务权限错误

若要详细了解如何排查数据移动服务的权限问题以及如何解决这些问题，请参阅 [PolyBase 服务帐户权限以及缺少这些权限时观察到的常见错误](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-service-account-permissions-and-common-errors-observed/ba-p/2112711)。


### <a name="windows-authentication-failure"></a>Windows 身份验证失败

若要详细了解如何排查与 Windows 身份验证失败相关的权限问题以及如何解决这些问题，请参阅 [PolyBase 服务帐户权限以及缺少这些权限时观察到的常见错误](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-service-account-permissions-and-common-errors-observed/ba-p/2112711)。


### <a name="cannot-execute-the-query-remote-query"></a>无法执行查询“远程查询” 

示例错误消息：

> 消息 7320，级别 16，状态 110，行 14<BR>
> 无法对链接服务器“(null)”的 OLE DB 访问接口“SQLNCLI11”执行查询“远程查询”。 查询已中止-- 在从外部源进行读取时达到最大拒绝阈值(0 行): 拒绝了已处理总计 1 行中的 1 行。
(/nation/sensors.ldjson.txt)列序号: 0，预期数据类型: INT, 问题值: {"id":"S2740036465E2B","time":"2016-02-26T16:59:02.9300000Z","temp":23.3,"hum":0.77,"wind":17,"press":1032,"loc":[-76.90914996169623,38.8929314364726]} (列转换错误)，错误: 将数据类型 NVARCHAR 转换为 INT 时出错。

请注意，可能会出现此错误。 第一个被拒绝文件的名称显示在 SQL Server Management Studio (SSMS) 中，包含有问题的数据类型或值。

可能的原因：  
出现此错误的原因是，每个文件都有不同的架构。 当指向某个目录时，PolyBase 外部表 DDL 会以递归方式读取该目录中的所有文件。 当列或数据类型不匹配时，可能会在 SSMS 中看到此错误。

可行解决方案：  
如果每个表的数据包含一个文件，则使用以外部文件目录为前缀的 LOCATION 部分中的文件名。 如果每个表有多个文件，请将每组文件放入 Azure Blob 存储中的不同目录。 将 LOCATION 指向目录，而不是特定文件。 推荐使用此解决方案。

**示例：**  

```sqlsyntax
Create External Table foo
(col1 int)WITH (LOCATION='/bar/foobar.txt',DATA_SOURCE…); OR
Create External Table foo
(col1 int) WITH (LOCATION = '/bar/', DATA_SOURCE…);
```


### <a name="kerberos-support"></a>Kerberos 支持

SQL Server 配置为访问受支持的 Hadoop 群集。 不会在 Hadoop 群集中强制执行 Kerberos 安全性。

从外部表中选择将返回以下错误：

> 消息 105019，级别 16，状态 1，行 55<BR>
> 由于内部错误，EXTERNAL TABLE 访问失败:“调用 HdfsBridge_Connect 时引发了 Java 异常: 在访问外部文件时出现错误[无法实例化 LoginClass]。”<BR>
> 消息 7320，级别 16，状态 110，行 55<BR>
> 无法对链接服务器“(null)”的 OLE DB 访问接口“SQLNCLI11”执行查询“远程查询”。 由于内部错误，EXTERNAL TABLE 访问失败:“调用 HdfsBridge_Connect 时引发了 Java 异常: 在访问外部文件时出现错误[无法实例化 LoginClass]。”

查询 DWEngine 服务器日志时出现以下错误:

> [线程:16432] [EngineInstrumentation:EngineQueryErrorEvent] (错误, 高):<BR>
> 由于内部错误，EXTERNAL TABLE 访问失败:“调用 HdfsBridge_Connect 时引发了 Java 异常: 访问外部文件时出现错误 [com.microsoft.polybase.client.KerberosSecureLogin]。”
Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: 由于内部错误，EXTERNAL TABLE 访问失败:“调用 HdfsBridge_Connect 时引发了 Java 异常: 访问外部文件时出现错误 [com.microsoft.polybase.client.KerberosSecureLogin]。” ---> Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException: 调用 HdfsBridge_Connect 时引发了 Java 异常: 访问外部文件时出现错误 [com.microsoft.polybase.client.KerberosSecureLogin]。

可能的原因：  
Hadoop 群集未启用 Kerberos，但默认位于 Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf 下的 core-site.xml、yarn-site.xml 或 hdfs-site.xml 启用了 Kerberos 安全性。 在 Linux 中，这些文件默认位于 /var/opt/mssql/binn/polybase/hadoop/conf/。

可行解决方案：  
注释掉上述文件中的 Kerberos 安全信息。

有关 PolyBase 和 Kerberos 疑难解答的详细信息，请参阅[排查 PolyBase Kerberos 连接问题](polybase-troubleshoot-connectivity.md)。

### <a name="internal-query-processor-error"></a>内部查询处理器错误

查询外部表返回以下错误：

> 消息 8680，级别 17，状态 5，行 118<BR>
> 内部查询处理器错误:查询处理器在处理远程查询操作过程中遇到意外错误。

DWEngine 服务器日志包含以下消息：

> [线程:5216] [ControlNodeMessenger:ErrorEvent] (错误, 高): ***** DMS 系统已断开连接节点:<BR>
> [线程:5216] [ControlNodeMessenger:ErrorEvent] (错误, 高): ***** DMS 系统已断开连接节点:<BR>
> [线程:5216] [ControlNodeMessenger:ErrorEvent] (错误, 高): ***** DMS 系统已断开连接节点:<BR>

可能的原因：  
此错误的原因可能是在配置 PolyBase 后 SQL Server 未重启。

可行解决方案：  
请重新启动 SQL Server。 请检查 DWEngine 服务器日志，确认重启后 DMS 未断开。


### <a name="user-needed-for-hdfs-access"></a>HDFS 访问所需的用户

**应用场景：**  
SQL Server 连接到不安全的 Hadoop 群集（未启用 Kerberos）。 PolyBase 配置为将计算推送到 Hadoop 群集。

示例查询：  

```sql
select count(*) from foo WITH (FORCE EXTERNALPUSHDOWN);
```

返回与以下内容类似的错误消息： 

> 消息 105019，级别 16，状态 1，行 1<BR>
> 由于内部错误，EXTERNAL TABLE 访问失败:“调用 JobSubmitter_PollJobStatus 时引发了 Java 异常: 访问外部表时出现错误 [java.net.ConnectException: 由于连接异常，从 big1506sql2016/172.16.1.4 到 0.0.0.0:10020 的调用过程失败: java.net ConnectException: 连接被拒绝: 没有更多信息; 有关更多详细信息，请参阅: http://wiki.apache.org/hadoop/ConnectionRefused ]。”<BR>
> 链接服务器“(null)”的 OLE DB 访问接口“SQLNCLI11”返回消息“未指定的错误”。<BR>
> 消息 7421，级别 16，状态 2，行 1<BR>
> 无法从链接服务器“(null)”的 OLE DB 访问接口“SQLNCLI11”提取行集。 .<BR>

Hadoop Yarn 日志错误：
> 作业设置失败 : org.apache.hadoop.security.AccessControlException: 权限被拒绝: user=pdw_user, access=WRITE, inode="/user":hdfs:hdfs:drwxr-xr-x at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkFsPermission(FSPermissionChecker.java:265) at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:251) at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:232) org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:176) at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkPermission(FSNamesystem.java:5525)

可能的原因：  
禁用 Kerberos 后，PolyBase 将使用 pdw_user 作为访问 HDFS 和提交 MapReduce 作业的用户。

可行解决方案：  
在 Hadoop 上创建 pdw_user，并向其授予对 mapreduce 处理过程中使用的目录足够的权限。 此外，请确保 pdw_user 是 /user/pdw_user HDFS 目录的所有者。

下面的示例演示如何创建主目录并为 pdw_user 分配权限：

```console
sudo -u hdfs hadoop fs -mkdir /user/pdw_user
sudo -u hdfs hadoop fs -chown pdw_user /user/pdw_user
```

在此之后，请确保 pdw_user 对 /user/pdw_user 目录具有读取、写入和执行权限。 请确保 /tmp 目录具有 777 权限。

有关 PolyBase 和 Kerberos 疑难解答的详细信息，请参阅[排查 PolyBase Kerberos 连接问题](polybase-troubleshoot-connectivity.md)。

### <a name="java-memory-error-due-to-utf-8"></a>因 UTF-8 导致的 Java 内存错误

**应用场景：**  
SQL Server PolyBase 是通过 Hadoop 群集或 Azure Blob 存储进行设置的。 任何 Select 查询都将失败，并出现以下错误：

> 消息 106000，级别 16，状态 1，行 1<BR>
> Java 堆空间<BR>

可能的原因：  
非法输入可能导致 java 内存不足错误。 文件可能不是 UTF-8 格式。 DMS 尝试将整个文件作为一行进行读取，因为它无法解码行分隔符，并会出现 Java 堆空间错误。

可行解决方案：  
将该文件转换为 UTF-8 格式，因为 PolyBase 当前要求文本分隔文件使用 UTF-8 格式。

### <a name="hadoop-connectivity-configuration"></a>Hadoop 连接配置

配置 SQL Server PolyBase 以连接到 Azure Blob 存储时，将在 SQL Server 中返回以下错误消息：

> 消息 105019，级别 16，状态 1，行 74<BR>
> 由于内部错误，EXTERNAL TABLE 访问失败:“调用 HdfsBridge_Connect 时引发了 Java 异常: 在访问外部文件时出现错误[架构没有 FileSystem: wasbs]。”<BR>

可能的原因：  
Hadoop 连接未设置为访问 Azure Blob 存储的配置值。

可行解决方案：  
将 Hadoop 连接设置为一个值（最好是 7），该值支持 Azure Blob 存储，然后重启 SQL Server。 有关连接值和支持的类型的列表，请参阅 [PolyBase 连接配置](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md#arguments)。


### <a name="create-table-as-select-error"></a>Create Table As Select 错误

**应用场景：**  
尝试使用 PolyBase 和 CREATE EXTERNAL TABLE AS SELECT (CETAS) 语法将数据从 SQL Server 导出到 Azure blob 存储或 Hadoop 文件系统时失败，并出现以下错误消息：

> 消息 156，级别 15，状态 1，行 177<BR>
> 关键字“WITH”附近有语法错误。<BR>
> 消息 319，级别 15，状态 1，行 177<BR>
> 关键字 'with' 附近有语法错误。 如果此语句是公用表表达式、xmlnamespaces 子句或者更改跟踪上下文子句，那么前一个语句必须以分号结尾。<BR>

可能的原因：  
通过 PolyBase 将数据导出到 Hadoop 或 Azure Blob 存储时，只会导出数据，而不会导出 CREATE EXTERNAL TABLE 命令中定义的列名称（元数据）。

可行解决方案：  
首先创建外部表，然后使用 INSERT INTO SELECT 选择导出到外部位置。 有关代码示例，请参阅 [PolyBase 查询方案](polybase-queries.md#export-data)。


### <a name="create-external-table-from-azure-blob-storage-fails"></a>从 Azure Blob 存储创建外部表失败

**应用场景：**  
SQL DW 设置为从 Azure Blob 存储导入数据。 创建外部表失败，并出现以下消息。

> 消息 105019，级别 16，状态 1，行 34<BR>
> 由于内部错误，EXTERNAL TABLE 访问失败:“调用 HdfsBridge_IsDirExist 时引发了 Java 异常。 Java 异常消息: com.microsoft.azure.storage.StorageException: 服务器未能对请求进行身份验证。 请确保授权标头的值格式正确(包括签名): 访问外部文件时出现错误 [StorageException: 服务器未能对请求进行身份验证。 请确保授权标头的值格式正确(包括签名)。]”<BR>

可能的原因：  
使用了错误的 Azure 存储密钥来创建数据库范围凭据。

可行解决方案：  
删除所有相关对象（例如数据源、文件格式），然后删除数据库范围凭据，并使用正确的存储密钥重新创建凭据。


### <a name="kerberos-configuration-capitalization"></a>Kerberos 配置大小写

**应用场景：**  
为 SQL Server 设置启用了 Kerberos 的 Cloudera 群集。 在所有配置更改后，SQL Server 已重新启动。 PolyBase 引擎和 PolyBase 数据移动服务在重启后运行。 返回以下错误消息：

未配置作业跟踪程序的数据源:  
> org.apache.hadoop.fs.FileSystem: Provider org.apache.hadoop.fs.viewfs.ViewFileSystem 不能实例化

配置了作业跟踪器位置的数据源:  
> 访问外部文件时出现错误[无法获取 Kerberos 领域]

可能的原因：  
“hadoop.security.authentication”属性的值指示 Coresite.xml 中的 kerberos。

可行解决方案：  
Coresite.xml 的“hadoop.security.authentication”属性值应为 KERBEROS (全部大写)。 

有关 PolyBase 和 Kerberos 疑难解答的详细信息，请参阅[排查 PolyBase Kerberos 连接问题](polybase-troubleshoot-connectivity.md)。

### <a name="mapred-sitexml-missing-needed-values"></a>Mapred-site.xml 缺少所需的值

**应用场景：**  
为 SQL Server 或 APS 设置支持的 HDP 群集。 对不需要下推工作的查询使用“强制下推”提示时，查询将失败并出现以下错误消息：

> 消息 7320，级别 16，状态 110，行 35<BR>
> 无法对链接服务器“(null)”的 OLE DB 访问接口“SQLNCLI11”执行查询“远程查询”。 由于出现内部错误，EXTERNAL TABLE 访问失败:“调用 JobSubmitter_PollJobStatus 时引发了 Java 异常: 访问外部文件时出现错误 [org.apache.hadoop.ipc.RemoteException(java.lang.NullPointerException): java.lang.NullPointerException<BR>
> at org.apache.hadoop.mapreduce.v2.hs.HistoryClientService$HSClientProtocolHandler.getTaskAttemptCompletionEvents(HistoryClientService.java:277)<BR>
> at org.apache.hadoop.mapreduce.v2.api.impl.pb.service.MRClientProtocolPBServiceImpl.getTaskAttemptCompletionEvents(MRClientProtocolPBServiceImpl.java:173)<BR>
> at org.apache.hadoop.yarn.proto.MRClientProtocol$MRClientProtocolService$2.callBlockingMethod(MRClientProtocol.java:283)<BR>
> at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:619)<BR>
> at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:962)<BR>
> at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2127)<BR>
> at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2123)<BR>
> at java.security.AccessController.doPrivileged(本机方法)<BR>
> at javax.security.auth.Subject.doAs(Subject.java:415)<BR>
> at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1671)<BR>
> at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2121)<BR>
> ]。”<BR>

可能的原因：  
Mapred-site.xml 缺少某些检查中间结果和最终结果所需的值。

可行解决方案：  
添加以下属性并关联正确的值，就像 SQL Server 上的 mapred-site.xml 文件中 Ambari 上显示的那样。 

```xml
<property>
<name>yarn.app.mapreduce.am.staging-dir</name>
<value>/user</value>
</property>
<property>
<name>mapreduce.jobhistory.done-dir</name>
<value>/mr-history/done</value>
</property>
<property>
<name>mapreduce.jobhistory.intermediate-done-dir</name>
<value>/mr-history/tmp</value>
</property>
```

### <a name="configuring-access-by-hostname"></a>配置主机名的访问权限 

**应用场景：**  
SQL Server 设置为访问支持的 Hadoop 群集。 创建外部表将返回以下错误之一：

> 无法对链接服务器“(null)”的 OLE DB 访问接口“SQLNCLI11”执行查询“远程查询”。 110802;由于出现内部 DMS 错误，此操作失败。 详细信息: 异常: Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.DmsSqlNativeException, Message: SqlNativeBufferReader.Run, error in OdbcExecuteQuery: SqlState: 42000, NativeError: 8680, '错误调用: SQLExecDirect(this->GetHstmt(), (SQLWCHAR *)statementText, SQL_NTS), SQL 返回代码: -1 | SQL 错误信息: SrvrMsgState: 26, SrvrSeverity: 17,  错误 <1>: ErrorMsg: [Microsoft][ODBC Driver 13 for SQL Server][SQL Server]内部查询处理器错误: 查询处理器在处理远程查询阶段遇到意外错误。 | 错误调用: pReadConn->ExecuteQuery(statementText, bufferFormat) | 状态: FFFF, 数目: 24, 活动连接: 8', 连接字符串: Driver={pdwodbc};APP=RCSmall-DmsNativeReader:WAD1D16HD2001\mpdwsvc (3600)-ODBC-PoolId1433;Trusted_Connection=yes;AutoTranslate=no;Server=\\.\pipe\sql\query<BR>

> [线程:30544] [AbstractReaderWorker:ErrorEvent] (错误, 高): QueryId QID2433 PlanId 6c3a4551-e54c-4c06-a5ed-a8733edac691 StepId 7:<BR>
> 无法获取块: BP-1726738607-192.168.225.121-1443123675290:blk_1159687047_86196509 file=/user/hive/warehouse/u_data/000000_0<BR>
> Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: 无法获取块: BP-1726738607-192.168.225.121-1443123675290:blk_1159687047_86196509 file=/user/hive/warehouse/u_data/000000_0<BR>
> at Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsBridgeReadAccess.Read(MemoryBuffer buffer, Boolean& isDone)<BR>
> at Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.DataReader.ExternalMoveBufferReader.Read()<BR>
> at Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.ExternalMoveReaderWorker.ReadAndSendData()<BR>
> at Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.ExternalMoveReaderWorker.Execute(Object status)<BR>

可能的原因：  
当 Hadoop 群集设置为只能通过主机名而不是 IP 地址访问群集外的数据节点的配置时，就会出现此错误消息。

可行解决方案：  
将以下项添加到客户端 (SQL Server) 上的 hdfs-site.xml 文件中。 此配置将强制名称节点返回具有主机名而不是内部 IP 地址的数据节点的 URI。

```xml
<property>
<name>dfs.client.use.datanode.hostname</name>
<value>true</value>
</property>
```

### <a name="folder-organization-forces-excess-memory-overhead"></a>文件夹组织强制额外的内存开销

**应用场景：**  
SQL Server 在一个有大量文件的目录上运行 PolyBase 查询（以递归方式在目录路径下有 30,000 多个文件），并返回以下错误消息之一：

> 消息 105019，级别 16，状态 1，行 1<BR>
> 由于内部错误，EXTERNAL TABLE 访问失败:“调用 HdfsBridge_GetFileNameByIndex 时引发了 Java 异常。 Java 异常消息: 超过了 GC 开销限制: 访问外部文件时出现错误[超出了 GC 开销限制]。”<BR>

> 消息 105019，级别 16，状态 1，行 1<BR>
> 由于内部错误，EXTERNAL TABLE 访问失败:“调用 HdfsBridge_GetDirectoryFiles 时引发了 Java 异常。 Java 异常消息: Java 堆空间: 访问外部文件时出现错误 [Java 堆空间]。

可能的原因：  
在处理路径时，PolyBase 将枚举该路径下的所有文件，并且会产生与用于表示这些文件的数据结构相关联的固定内存开销。 对于大量文件，此开销会变得很明显，最终会消耗可用于 JVM 的所有内存。

可行解决方案：  
重新排列多个目录中的数据，以便每个目录包含一个文件子集，然后将该查询分解成多个查询，这些查询一次操作原始路径的一部分，并将表具体化为 SQL Server 表（在加入它们之前）。

示例：假设你的外部表数据位于以下位置：Orders/file1.txt,...,file30K.txt。

更改布局，使数据按 Orders/*yyyy*/*mm*/*dd*/file1.txt 中的传统文件分区结构布局。
将外部表指向更低端的目录路径（如月 (mm) 或日 (dd)）并将文件分块导入到 SQL Server 表中，然后将它们添加为一个表的一部分。
即使你一开始就有正确的目录结构，也可以按照步骤 #2 来处理多个文件，而不会耗尽 JVM 内存。


### <a name="unexpected-characters-in-configuration-files"></a>配置文件中出现意外字符

**应用场景：**  
为 SQL Server 或 APS 设置 Hadoop 群集，这涉及到修改 yarn-site.xml、hdfs-site.xml 和其他配置文件。 观察到以下 SQL Server 错误消息：

> 消息 105019，级别 16，状态 1，行 1<BR>
> Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: 由于内部错误，EXTERNAL TABLE 访问失败:“调用 HdfsBridge_Connect 时引发了 Java 异常。 Java 异常消息:com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: Invalid byte 1 of 1-byte UTF-8 sequence.: 访问外部表时出现错误 [com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: Invalid byte 1 of 1-byte UTF-8 sequence.]。” ---><BR>

可能的原因：  
如果已从网站或聊天窗口将文本复制并粘贴到配置文件中，则可能出现此错误。 配置文件中可能有不需要的/不可打印的字符。

可行解决方案：  
在不同文本编辑器（无非记事本）中打开文件，并查找这些字符并删除它们。 重新启动必要的服务。 


## <a name="see-also"></a>另请参阅

[PolyBase 的监视和故障排除](polybase-troubleshooting.md)  
[PolyBase Kerberos 连接疑难解答](polybase-troubleshoot-connectivity.md)  

