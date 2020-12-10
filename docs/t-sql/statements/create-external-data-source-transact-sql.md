---
description: CREATE EXTERNAL DATA SOURCE (Transact-SQL)
title: CREATE EXTERNAL DATA SOURCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/26/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL DATA SOURCE
- CREATE_EXTERNAL_DATA_SOURCE
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, data source
- PolyBase, create data source
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7888360aa4d4bdfa9b9eb782226f718d8f503ff8
ms.sourcegitcommit: 0c0e4ab90655dde3e34ebc08487493e621f25dda
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96443092"
---
# <a name="create-external-data-source-transact-sql"></a>CREATE EXTERNAL DATA SOURCE (Transact-SQL)

使用 SQL Server、SQL 数据库、Azure Synapse Analytics 或分析平台系统（并行数据仓库或 PDW）创建用于查询的外部数据源。

本文提供所选任何 SQL 产品的语法、参数、注解、权限和示例。

有关语法约定的详细信息，请参阅 [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017||=sqlallproducts-allversions"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL 数据库](create-external-data-source-transact-sql.md?view=azuresqldb-current)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-data-source-transact-sql.md?view=azure-sqldw-latest)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-data-source-transact-sql.md?view=aps-pdw-2016-au7)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-server"></a>概述：SQL Server

为 PolyBase 查询创建外部数据源。 外部数据源用于建立连接以及支持以下这些用例：

- 使用 [PolyBase][intro_pb] 执行数据虚拟化和数据加载
- 使用 `BULK INSERT` 或 `OPENROWSET` 大容量加载操作

**适用对象**：自 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 起

## <a name="syntax"></a>语法

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( [ LOCATION = '<prefix>://<path>[:<port>]' ]
    [ [ , ] CONNECTION_OPTIONS = '<name_value_pairs>']
    [ [ , ] CREDENTIAL = <credential_name> ]
    [ [ , ] PUSHDOWN = { ON | OFF } ]
    [ [ , ] TYPE = { HADOOP | BLOB_STORAGE } ]
    [ [ , ] RESOURCE_MANAGER_LOCATION = '<resource_manager>[:<port>]' )
[ ; ]
```

## <a name="arguments"></a>参数

### <a name="data_source_name"></a>data_source_name

指定数据源的用户定义名称。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，该名称在数据库中必须唯一。

### <a name="location--prefixpathport"></a>LOCATION = *`'<prefix>://<path[:port]>'`*

提供连接协议和外部数据源的路径。

| 外部数据源    | 位置前缀 | 位置路径                                         | 产品/服务支持的位置 |
| ----------------------- | --------------- | ----------------------------------------------------- | ---------------------------------------- |
| Cloudera 或 Hortonworks | `hdfs`          | `<Namenode>[:port]`                                   | 自 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 起                       |
| Azure 存储帐户 (V2) | `wasb[s]`       | `<container>@<storage_account>.blob.core.windows.net` | 自 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 起          不支持分层命名空间 |
| [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]              | `sqlserver`     | `<server_name>[\<instance_name>][:port]`              | 自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起                       |
| Oracle                  | `oracle`        | `<server_name>[:port]`                                | 自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起                       |
| Teradata                | `teradata`      | `<server_name>[:port]`                                | 自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起                       |
| MongoDB 或 CosmosDB     | `mongodb`       | `<server_name>[:port]`                                | 自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起                       |
| ODBC                    | `odbc`          | `<server_name>[:port]`                                | 自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起 - 仅限 Windows        |
| 批量操作         | `https`         | `<storage_account>.blob.core.windows.net/<container>` | 自 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 起                        |
| Edge 中心         | `edgehub`         | 不适用 | EdgeHub 始终位于 [Azure SQL Edge](/azure/azure-sql-edge/overview/) 实例的本地。 因此，无需指定路径或端口值。 仅在 Azure SQL Edge 中可用。                      |
| Kafka        | `kafka`         | `<Kafka IP Address>[:port]` | 仅在 Azure SQL Edge 中可用。                      |

位置路径：

- `<`Namenode`>` = Hadoop 群集中 `Namenode` 的计算机名称、名称服务 URI 或 IP 地址。 PolyBase 必须解析 Hadoop 群集使用的任何 DNS 名称。 <!-- For highly available Hadoop configurations, provide the Nameservice ID as the `LOCATION`. -->
- `port` = 外部数据源侦听的端口。 在 Hadoop 中，可以使用 `fs.defaultFS` 配置参数查找该端口。 默认值为 8020。
- `<container>` = 保存数据的存储帐户的容器。 根容器是只读的，数据无法写回容器。
- `<storage_account>` = Azure 资源的存储帐户名称。
- `<server_name>` = 主机名。
- `<instance_name>` = SQL Server 命名实例的名称。 如果在目标实例上运行 SQL Server Browser 服务，则使用此路径。

设置位置时的其他说明和指南：

- 创建对象时，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 不会验证外部数据源是否存在。 要进行验证，请使用外部数据源创建外部表。
- 查询 Hadoop 时，所有表使用相同的外部数据源，以确保查询语义一致。
- 可使用 `sqlserver` 位置前缀将 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 连接到另一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 或 Azure Synapse Analytics。
- 通过 `ODBC` 连接时，请指定 `Driver={<Name of Driver>}`。
- `wasbs` 是可选的，但建议用于访问 Azure 存储帐户，因为将使用安全的 TLS/SSL 连接发送数据。
- 访问 Azure 存储帐户时，不支持 `abfs` 或 `abfss` API。
- 不支持 Azure 存储帐户 (V2) 的“分层命名空间”选项。 请确保禁用此选项。
- 要确保在 Hadoop `Namenode` 故障转移期间成功进行 PolyBase 查询，请考虑针对 Hadoop 群集的 `Namenode` 使用虚拟 IP 地址。 如果不这样做，请执行 [ALTER EXTERNAL DATA SOURCE][alter_eds] 命令以指向新位置。

### <a name="connection_options--key_value_pair"></a>CONNECTION_OPTIONS = key_value_pair

通过 `ODBC` 连接到外部数据源时指定其他选项。

驱动程序的名称是必需的（最基本的要求），但设置其他选项（例如 `APP='<your_application_name>'` 或 `ApplicationIntent= ReadOnly|ReadWrite`）也很有用，可以帮助进行故障排除。

请参阅 `ODBC` 产品文档，获取允许的 [CONNECTION_OPTIONS][connection_options] 列表

### <a name="pushdown--on--off"></a>PUSHDOWN = 打开 | 关闭

说明是否可以将计算下推到外部数据源。 它默认处于打开状态。

在外部数据源级别连接到 SQL Server、Oracle、Teradata、MongoDB 或 ODBC 时，`PUSHDOWN` 受支持。

通过[提示][hint_pb]实现在查询级别启用或禁用下推。

### <a name="credential--credential_name"></a>CREDENTIAL = credential_name

指定用于对外部数据源进行身份验证的数据库范围凭据。

创建凭证时的其他说明和指导：

- 只有在数据得到保护的情况下才需要 `CREDENTIAL`。 允许匿名访问的数据集不需要 `CREDENTIAL`。
- 当 `TYPE` = `BLOB_STORAGE` 时，必须使用 `SHARED ACCESS SIGNATURE` 作为标识创建凭据。 此外，应按如下所示配置 SAS 令牌：
  - 配置为密码时排除前导 `?`
  - 至少对应加载的文件具有读取权限（例如 `srt=o&sp=r`）
  - 使用有效的有效期（所有日期均采用 UTC 时间）。

有关使用具有 `SHARED ACCESS SIGNATURE` 和 `TYPE` = `BLOB_STORAGE` 的 `CREDENTIAL` 的示例，请参阅[创建外部数据源以执行批量操作并将数据从 Azure 存储检索到 SQL 数据库](#i-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-storage)

要创建数据库范围凭据，请参阅 [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]。

### <a name="type---hadoop--blob_storage-"></a>TYPE = [HADOOP | BLOB_STORAGE ]

指定要配置的外部数据源的类型。 此参数并非总是必需的。

- 如果外部数据源为 Cloudera、Hortonworks 或 Azure 存储帐户，请使用 HADOOP。
- 在使用 [BULK INSERT][bulk_insert] 或 [OPENROWSET][openrowset] 从 Azure 存储帐户对 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 执行批量操作时，可使用 BLOB_STORAGE。

> [!IMPORTANT]
> 如果使用任何其他外部数据源，请勿设置 `TYPE`。

有关使用 `TYPE` = `HADOOP` 从 Azure 存储帐户加载数据的示例，请参阅[创建外部数据源以使用 wasb:// 接口访问 Azure 存储中的数据](#e-create-external-data-source-to-access-data-in-azure-storage-using-the-wasb-interface) <!--[Create external data source to reference Azure Storage](#e-create-external-data-source-to-reference-azure-storage).-->

### <a name="resource_manager_location--resourcemanager_uriport"></a>RESOURCE_MANAGER_LOCATION = 'ResourceManager_URI[:port]'

连接到 Hortonworks 或 Cloudera 时配置此可选值。

定义 `RESOURCE_MANAGER_LOCATION` 后，查询优化器将做出基于成本的决策，以提高性能。 MapReduce 作业可用于将计算下推到 Hadoop。 指定 `RESOURCE_MANAGER_LOCATION` 可以显著减少 Hadoop 与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之间传输的数据量，这可以提高查询性能。

如果未指定资源管理器，则会为 PolyBase 查询禁用到 Hadoop 的计算下推。

如果未指定端口，则使用“hadoop 连接”配置的当前设置选择默认值。

| Hadoop 连接 | 默认资源管理器端口 |
| ------------------- | ----------------------------- |
| 1                   | 50300                         |
| 2                   | 50300                         |
| 3                   | 8021                          |
| 4                   | 8032                          |
| 5                   | 8050                          |
| 6                   | 8032                          |
| 7                   | 8050                          |

有关受支持的 Hadoop 版本的完整列表，请参阅 [PolyBase 连接配置 (Transact-SQL)][connectivity_pb]。

> [!IMPORTANT]  
> 创建外部数据源时，不会验证 RESOURCE_MANAGER_LOCATION 值。 每次尝试下推时，输入不正确的值都可能会导致查询执行失败，因为提供的值无法解析。

[创建外部数据源以引用启用了下推功能的 Hadoop](#c-create-external-data-source-to-reference-hadoop-with-push-down-enabled) 中提供了具体示例和详细指南。

## <a name="permissions"></a>权限

需要对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的数据库具有 `CONTROL` 权限。

## <a name="locking"></a>锁定

对 `EXTERNAL DATA SOURCE` 对象采用共享锁。

## <a name="security"></a>安全性

PolyBase 支持大多数外部数据源的基于代理的身份验证。 创建数据库范围凭据以创建代理帐户。

连接到 SQL Server 大数据群集中的存储或数据池时，会将用户的凭据传递到后端系统。 在数据池本身中创建登录名以启用直通身份验证。

目前，不支持类型为 `HADOOP` 的 SAS 令牌。 它仅在使用存储帐户访问密钥时才受支持。 尝试创建类型为 `HADOOP` 的外部数据源和使用 SAS 凭据失败，并显示以下错误：

`Msg 105019, Level 16, State 1 - EXTERNAL TABLE access failed due to internal error: 'Java exception raised on call to HdfsBridge_Connect. Java exception message: Parameters provided to connect to the Azure storage account are not valid.: Error [Parameters provided to connect to the Azure storage account are not valid.] occurred while accessing external file.'`

## <a name="examples-starting-with-sssql15"></a>示例（自 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 起）

> [!IMPORTANT]
> 有关如何安装和启用 PolyBase 的信息，请参阅[在 Windows 上安装 PolyBase](../../relational-databases/polybase/polybase-installation.md)

### <a name="a-create-external-data-source-in-sql-server-2019-to-reference-oracle"></a>A. 在 SQL Server 2019 中创建外部数据源以引用 Oracle

要创建引用 Oracle 的外部数据源，请确保具有数据库范围凭据。 也可以选择针对此数据源启用或禁用计算下推。

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '!MyC0mpl3xP@ssw0rd!' ;

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL OracleProxyAccount
WITH
     IDENTITY = 'oracle_username',
     SECRET = 'oracle_password' ;

CREATE EXTERNAL DATA SOURCE MyOracleServer
WITH
  ( LOCATION = 'oracle://145.145.145.145:1521',
    CREDENTIAL = OracleProxyAccount,
    PUSHDOWN = ON
  ) ;
```

有关其他数据源（如 MongoDB）的更多示例，请参阅[配置 PolyBase 以访问 MongoDB 中的外部数据][mongodb_pb]

### <a name="b-create-external-data-source-to-reference-hadoop"></a>B. 创建外部数据源以引用 Hadoop

若要创建外部数据源以引用 Hortonworks 或 Cloudera Hadoop 群集，请指定 Hadoop `Namenode` 的计算机名称或 IP 地址以及端口。 <!-- Provide the Nameservice ID as the `LOCATION` for highly available configurations. -->

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8050' ,
    TYPE = HADOOP
  ) ;
```

### <a name="c-create-external-data-source-to-reference-hadoop-with-push-down-enabled"></a>C. 创建外部数据源以引用 Hadoop 并启用下推

指定 `RESOURCE_MANAGER_LOCATION` 选项以便为 PolyBase 查询启用到 Hadoop 的下推计算。 启用后，PolyBase 会根据成本作出决策，以确定是否应将查询计算下推到 Hadoop。

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8020' ,
    TYPE = HADOOP ,
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
  ) ;
```

### <a name="d-create-external-data-source-to-reference-kerberos-secured-hadoop"></a>D. 创建外部数据源以引用受 Kerberos 保护的 Hadoop

若要验证 Hadoop 群集是否受 Kerberos 保护，请检查 Hadoop core-site.xml 中的 hadoop.security.authentication 属性值。 若要引用受 Kerberos 保护的 Hadoop 群集，必须指定包含 Kerberos 用户名和密码的数据库范围凭据。 数据库主密钥用于加密数据库范围凭据密钥。

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Kerberos user name and password.
CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
WITH
     IDENTITY = '<hadoop_user_name>',
     SECRET = '<hadoop_password>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8050' ,
    CREDENTIAL = HadoopUser1 ,
    TYPE = HADOOP ,
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
  );
```

### <a name="e-create-external-data-source-to-access-data-in-azure-storage-using-the-wasb-interface"></a>E. 创建外部数据源以使用 wasb:// 接口访问 Azure 存储中的数据
在本示例中，外部数据源是名为 `logs` 的 Azure V2 存储帐户。 容器名称为 `daily`。 Azure 存储外部数据源仅用于数据传输。 它不支持谓词下推。 通过 `wasb://` 接口访问数据时，不支持分层命名空间。

本示例演示如何创建数据库范围凭据以用于对 Azure V2 存储帐户进行身份验证。 在数据库凭据机密中指定 Azure 存储帐户密钥。 可以在数据库范围凭据标识中指定任何字符串，因为在对 Azure 存储进行身份验证的过程中不会使用它。

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
  IDENTITY = '<my_account>' ,
  SECRET = '<azure_storage_account_key>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyAzureStorage
WITH
  ( LOCATION = 'wasbs://daily@logs.blob.core.windows.net/' ,
    CREDENTIAL = AzureStorageCredential ,
    TYPE = HADOOP
  ) ;
```

### <a name="f-create-external-data-source-to-reference-a-sql-server-named-instance-via-polybase-connectivity-sql-server-2019"></a>F. 创建外部数据源以通过 PolyBase 连接引用 SQL Server 命名实例 ([!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)])

要创建引用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 命名实例的外部数据源，可以使用 CONNECTION_OPTIONS 指定实例名称。 在下面的示例中，`WINSQL2019` 是主机名，而 `SQL2019` 是实例名。

```sql
CREATE EXTERNAL DATA SOURCE SQLServerInstance2
WITH (
  LOCATION = 'sqlserver://WINSQL2019' ,
  CONNECTION_OPTIONS = 'Server=%s\SQL2019' ,
  CREDENTIAL = SQLServerCredentials
) ;
```

或者，可以使用端口连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。

```sql
CREATE EXTERNAL DATA SOURCE SQLServerInstance2
WITH (
  LOCATION = 'sqlserver://WINSQL2019:58137' ,
  CREDENTIAL = SQLServerCredentials
) ;
```

### <a name="g-create-external-data-source-to-reference-kafka"></a>G. 创建外部数据源以引用 Kafka

在本示例中，外部数据源是 IP 地址为 xxx.xxx.xxx.xxx 且在端口 1900 上进行侦听的 Kafak 服务器。 Kafka 外部数据源仅用于数据流式传输，不支持谓词向下推送。

```sql
-- Create an External Data Source for Kafka
CREATE EXTERNAL DATA SOURCE MyKafkaServer WITH (
    LOCATION = 'kafka://xxx.xxx.xxx.xxx:1900'
)
go
```

### <a name="h-create-external-data-source-to-reference-edgehub"></a>H. 创建外部数据源以引用 EdgeHub

在本示例中，外部数据源是在与 Azure SQL Edge 相同的边缘设备上运行的 EdgeHub。 EdgeHub 外部数据源仅用于数据流式传输，不支持谓词向下推送。

```sql
-- Create an External Data Source for Kafka
CREATE EXTERNAL DATA SOURCE MyEdgeHub WITH (
    LOCATION = 'edgehub://'
)
go
```

## <a name="examples-bulk-operations"></a>示例：批量操作

> [!IMPORTANT]
> 为批量操作配置外部数据源时，请勿在 `LOCATION` URL 的末尾放置尾随 /、文件名或共享访问签名参数。

### <a name="i-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-storage"></a>I. 创建外部数据源以用于从 Azure 存储检索数据的批量操作

适用对象：[!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]。
对使用 [BULK INSERT][bulk_insert] 或 [OPENROWSET][openrowset] 的批量操作使用以下数据源。 凭据必须设置 `SHARED ACCESS SIGNATURE` 作为标识、不应在 SAS 令牌中具有前导 `?`、必须对应加载的文件（例如 `srt=o&sp=r`）至少具有读取权限，并且有效期应有效（所有日期均采用 UTC 时间）。 有关共享访问签名的详细信息，请参阅[使用共享访问签名 (SAS)][sas_token]。

```sql
CREATE DATABASE SCOPED CREDENTIAL AccessAzureInvoices
WITH
  IDENTITY = 'SHARED ACCESS SIGNATURE',
  -- Remove ? from the beginning of the SAS token
  SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************' ;

CREATE EXTERNAL DATA SOURCE MyAzureInvoices
WITH
  ( LOCATION = 'https://newinvoices.blob.core.windows.net/week3' ,
    CREDENTIAL = AccessAzureInvoices ,
    TYPE = BLOB_STORAGE
  ) ;
```

若要查看正在使用的示例，请参阅 [BULK INSERT][bulk_insert_example]。

## <a name="see-also"></a>另请参阅

- [ALTER EXTERNAL DATA SOURCE (Transact-SQL)][alter_eds]
- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]
- [CREATE EXTERNAL FILE FORMAT (Transact-SQL)][create_eff]
- [CREATE EXTERNAL TABLE (Transact-SQL)][create_etb]
- [sys.external_data_sources (Transact-SQL)][cat_eds]
- [使用共享访问签名 (SAS)][sas_token]

<!-- links to external pages -->
<!-- SQL Docs -->
[bulk_insert]: ./bulk-insert-transact-sql.md
[bulk_insert_example]: ./bulk-insert-transact-sql.md#f-importing-data-from-a-file-in-azure-blob-storage
[openrowset]: ../functions/openrowset-transact-sql.md

[create_dsc]: ./create-database-scoped-credential-transact-sql.md
[create_eff]: ./create-external-file-format-transact-sql.md
[create_etb]: ./create-external-table-transact-sql.md
[create_etb_as_sel]: ./create-external-table-as-select-transact-sql.md?view=azure-sqldw-latest
[create_tbl_as_sel]: ./create-table-as-select-azure-sql-data-warehouse.md?view=azure-sqldw-latest

[alter_eds]: ./alter-external-data-source-transact-sql.md

[cat_eds]: ../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md
<!-- PolyBase docs -->
[intro_pb]: ../../relational-databases/polybase/polybase-guide.md
[mongodb_pb]: ../../relational-databases/polybase/polybase-configure-mongodb.md
[connectivity_pb]:https://docs.microsoft.com/sql/database-engine/configure-windows/polybase-connectivity-configuration-transact-sql
[connection_options]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md
[hint_pb]: ../../relational-databases/polybase/polybase-pushdown-computation.md#force-pushdown

<!-- Azure Docs -->
[sas_token]: /azure/storage/storage-dotnet-shared-access-signature-part-1

::: moniker-end
::: moniker range="=azuresqldb-current||=sqlallproducts-allversions"

:::row:::
    :::column:::
        [SQL Server](create-external-data-source-transact-sql.md?view=sql-server-2017)
    :::column-end:::
    :::column:::
        \* SQL 数据库 \*&nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-data-source-transact-sql.md?view=azure-sqldw-latest)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-data-source-transact-sql.md?view=aps-pdw-2016-au7)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-sql-database"></a>概述：Azure SQL Database

为弹性查询创建外部数据源。 外部数据源用于建立连接以及支持以下这些用例：

- 使用 `BULK INSERT` 或 `OPENROWSET` 大容量加载操作
- 使用[弹性查询][remote_eq]通过 SQL 数据库查询远程 SQL 数据库或 Azure Synapse 实例
- 使用[弹性查询][sharded_eq]查询分片的 Azure SQL 数据库

## <a name="syntax"></a>语法

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( [ LOCATION = '<prefix>://<path>[:<port>]' ]
    [ [ , ] CREDENTIAL = <credential_name> ]
    [ [ , ] TYPE = { BLOB_STORAGE | RDBMS | SHARD_MAP_MANAGER } ]
    [ [ , ] DATABASE_NAME = '<database_name>' ]
    [ [ , ] SHARD_MAP_NAME = '<shard_map_manager>' ] )
[ ; ]
```

## <a name="arguments"></a>参数

### <a name="data_source_name"></a>data_source_name

指定数据源的用户定义名称。 该名称在 SQL 数据库中必须是唯一的。

### <a name="location--prefixpathport"></a>LOCATION = *`'<prefix>://<path[:port]>'`*

提供连接协议和外部数据源的路径。

| 外部数据源   | 位置前缀 | 位置路径                                         |
| ---------------------- | --------------- | ----------------------------------------------------- |
| 批量操作        | `https`         | `<storage_account>.blob.core.windows.net/<container>` |
| 弹性查询（分片）  | 不是必需    | `<shard_map_server_name>.database.windows.net`        |
| 弹性查询（远程） | 不是必需    | `<remote_server_name>.database.windows.net`           |

位置路径：

- `<shard_map_server_name>` = Azure 中托管分片映射管理器的逻辑服务器名称。 `DATABASE_NAME` 参数提供用于托管分片映射的数据库，`SHARD_MAP_NAME` 用于分片映射本身。
- `<remote_server_name>` = 弹性查询的目标逻辑服务器名称。 使用 `DATABASE_NAME` 参数指定数据库名称。

设置位置时的其他说明和指南：

- 创建对象时，[!INCLUDE[ssde_md](../../includes/ssde_md.md)] 不会验证外部数据源是否存在。 要进行验证，请使用外部数据源创建外部表。

### <a name="credential--credential_name"></a>CREDENTIAL = credential_name

指定用于对外部数据源进行身份验证的数据库范围凭据。

创建凭证时的其他说明和指导：

- 若要将数据从 Azure 存储加载到 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]，请使用共享访问签名（SAS 令牌）。
- 只有在数据得到保护的情况下才需要 `CREDENTIAL`。 允许匿名访问的数据集不需要 `CREDENTIAL`。
- 当 `TYPE` = `BLOB_STORAGE` 时，必须使用 `SHARED ACCESS SIGNATURE` 作为标识创建凭据。 此外，应按如下所示配置 SAS 令牌：
  - 配置为密码时排除前导 `?`
  - 至少对应加载的文件具有读取权限（例如 `srt=o&sp=r`）
  - 使用有效的有效期（所有日期均采用 UTC 时间）。

有关使用具有 `SHARED ACCESS SIGNATURE` 和 `TYPE` = `BLOB_STORAGE` 的 `CREDENTIAL` 的示例，请参阅[创建外部数据源以执行批量操作并将数据从 Azure 存储检索到 SQL 数据库](#c-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-storage)

要创建数据库范围凭据，请参阅 [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]。

### <a name="type---blob_storage--rdbms--shard_map_manager"></a>TYPE = [ BLOB_STORAGE | RDBMS | SHARD_MAP_MANAGER]

指定要配置的外部数据源的类型。 此参数并非总是必需的。

- 使用 `RDBMS` 通过 SQL 数据库中的弹性查询进行跨数据库查询。
- 连接到分片的 SQL 数据库时，请使用 `SHARD_MAP_MANAGER` 创建外部数据源。
- 使用 [BULK INSERT][bulk_insert] 或 [OPENROWSET][openrowset] 执行批量操作时，请使用 `BLOB_STORAGE`。

> [!IMPORTANT]
> 如果使用任何其他外部数据源，请勿设置 `TYPE`。

### <a name="database_name--database_name"></a>DATABASE_NAME = database_name

当 `TYPE` 设置为 `RDBMS` 或 `SHARD_MAP_MANAGER` 时，配置此参数。

| TYPE              | DATABASE_NAME 的值                                       |
| ----------------- | ------------------------------------------------------------ |
| RDBMS             | 使用 `LOCATION` 提供的服务器上的远程数据库的名称 |
| SHARD_MAP_MANAGER | 作为分片映射管理器运行的数据库的名称      |

有关如何创建 `TYPE` = `RDBMS` 的外部数据源的示例，请参阅[创建 RDBMS 外部数据源](#b-create-an-rdbms-external-data-source)

### <a name="shard_map_name--shard_map_name"></a>SHARD_MAP_NAME = shard_map_name

将 `TYPE` 参数设置为 `SHARD_MAP_MANAGER` 时使用，仅用于设置分片映射的名称。

有关如何创建 `TYPE` = `SHARD_MAP_MANAGER` 的外部数据源的示例，请参阅[创建分片映射管理器外部数据源](#a-create-a-shard-map-manager-external-data-source)

## <a name="permissions"></a>权限

需要对 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中的数据库具有 `CONTROL` 权限。

## <a name="locking"></a>锁定

对 `EXTERNAL DATA SOURCE` 对象采用共享锁。

## <a name="examples"></a>示例：

### <a name="a-create-a-shard-map-manager-external-data-source"></a>A. 创建分片映射管理器外部数据源

若要创建外部数据源以引用 SHARD_MAP_MANAGER，请指定托管 SQL 数据库中的分片映射管理器或虚拟机上的 SQL Server 数据库的 SQL 数据库服务器名称。

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
WITH
  IDENTITY = '<username>',
  SECRET = '<password>' ;

CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc
WITH
  ( TYPE = SHARD_MAP_MANAGER ,
    LOCATION = '<server_name>.database.windows.net' ,
    DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb' ,
    CREDENTIAL = ElasticDBQueryCred ,
    SHARD_MAP_NAME = 'CustomerIDShardMap'
  ) ;
```

有关分步教程，请参阅[跨扩展云数据库进行报告（预览）][sharded_eq_tutorial]。

### <a name="b-create-an-rdbms-external-data-source"></a>B. 创建 RDBMS 外部数据源

若要创建外部数据源以引用 RDBMS，请指定 SQL 数据库中的远程数据库的 SQL 数据库服务器名称。

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

CREATE DATABASE SCOPED CREDENTIAL SQL_Credential
WITH
  IDENTITY = '<username>' ,
  SECRET = '<password>' ;

CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc
WITH
  ( TYPE = RDBMS ,
    LOCATION = '<server_name>.database.windows.net' ,
    DATABASE_NAME = 'Customers' ,
    CREDENTIAL = SQL_Credential
  ) ;
```

有关 RDBMS 的分步教程，请参阅[跨数据库查询（纵向分区）入门（预览）][remote_eq_tutorial]。

## <a name="examples-bulk-operations"></a>示例：批量操作

> [!IMPORTANT]
> 为批量操作配置外部数据源时，请勿在 `LOCATION` URL 的末尾放置尾随 /、文件名或共享访问签名参数。

### <a name="c-create-an-external-data-source-for-bulk-operations-retrieving-data-from-azure-storage"></a>C. 创建外部数据源以用于从 Azure 存储检索数据的批量操作

对使用 [BULK INSERT][bulk_insert] 或 [OPENROWSET][openrowset] 的批量操作使用以下数据源。 凭据必须设置 `SHARED ACCESS SIGNATURE` 作为标识、不应在 SAS 令牌中具有前导 `?`、必须对应加载的文件（例如 `srt=o&sp=r`）至少具有读取权限，并且有效期应有效（所有日期均采用 UTC 时间）。 有关共享访问签名的详细信息，请参阅[使用共享访问签名 (SAS)][sas_token]。

```sql
CREATE DATABASE SCOPED CREDENTIAL AccessAzureInvoices
WITH
  IDENTITY = 'SHARED ACCESS SIGNATURE',
  -- Remove ? from the beginning of the SAS token
  SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************' ;

CREATE EXTERNAL DATA SOURCE MyAzureInvoices
WITH
  ( LOCATION = 'https://newinvoices.blob.core.windows.net/week3' ,
    CREDENTIAL = AccessAzureInvoices ,
    TYPE = BLOB_STORAGE
  ) ;
```

若要查看这一使用中的示例，请参阅 [BULK INSERT][bulk_insert_example]。

## <a name="see-also"></a>另请参阅

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]
- [CREATE EXTERNAL TABLE (Transact-SQL)][create_etb]
- [sys.external_data_sources (Transact-SQL)][cat_eds]
- [使用共享访问签名 (SAS)][sas_token]
- [弹性查询简介][intro_eq]

<!-- links to external pages -->
<!-- SQL Docs -->
[bulk_insert]: ./bulk-insert-transact-sql.md
[bulk_insert_example]: ./bulk-insert-transact-sql.md#f-importing-data-from-a-file-in-azure-blob-storage
[openrowset]: ../functions/openrowset-transact-sql.md
[create_dsc]: ./create-database-scoped-credential-transact-sql.md
[create_etb]: https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source
[alter_eds]: ./alter-external-data-source-transact-sql.md
[cat_eds]: ../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md
<!-- PolyBase docs -->
[intro_pb]: ../../relational-databases/polybase/polybase-guide.md
[mongodb_pb]: ../../relational-databases/polybase/polybase-configure-mongodb.md
[connectivity_pb]:https://docs.microsoft.com/sql/database-engine/configure-windows/polybase-connectivity-configuration-transact-sql
[connection_options]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md
[hint_pb]: ../../relational-databases/polybase/polybase-pushdown-computation.md#force-pushdown
<!-- Elastic Query Docs -->
[intro_eq]: /azure/azure-sql/database/elastic-query-overview
[remote_eq]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[remote_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[sharded_eq]: /azure/azure-sql/database/elastic-query-getting-started
[sharded_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started

<!-- Azure Docs -->
[azure_ad]: /azure/data-lake-store/data-lake-store-authenticate-using-active-directory
[sas_token]: /azure/storage/storage-dotnet-shared-access-signature-part-1

::: moniker-end
::: moniker range="=azure-sqldw-latest||=sqlallproducts-allversions"

:::row:::
    :::column:::
        [SQL Server](create-external-data-source-transact-sql.md?view=sql-server-2017)
    :::column-end:::
    :::column:::
        [SQL 数据库](create-external-data-source-transact-sql.md?view=azuresqldb-current)
    :::column-end:::
    :::column:::
        _Azure Synapse<br />Analytics_&nbsp;\*\*
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-data-source-transact-sql.md?view=aps-pdw-2016-au7)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-synapse-analytics"></a>概述：Azure Synapse Analytics

为 PolyBase 创建外部数据源。 外部数据源用于建立连接以及支持以下主要用例：使用 [PolyBase][intro_pb] 执行数据虚拟化和数据加载

> [!IMPORTANT]  
> 若要使用[弹性查询][remote_eq]通过 Azure SQL 数据库创建外部数据源，以查询 SQL Analytics 资源，请参阅 [SQL 数据库](create-external-data-source-transact-sql.md?view=azuresqldb-current)。

## <a name="syntax"></a>语法

### [[!INCLUDE[sss-dedicated-pool-md.md](../../includes/sss-dedicated-pool-md.md)]](#tab/dedicated)
```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( [ LOCATION = '<prefix>://<path>[:<port>]' ]
    [ [ , ] CREDENTIAL = <credential_name> ]
    [ [ , ] TYPE = HADOOP ]
[ ; ]
```
### [[!INCLUDE[sssod-md.md](../../includes/sssod-md.md)]](#tab/serverless)
```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>  
WITH
(    LOCATION = '<prefix>://<path>[:<port>]'
) 
[;]
```
---

## <a name="arguments"></a>参数

### <a name="data_source_name"></a>data_source_name

指定数据源的用户定义名称。 该名称在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 的 [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] 中必须唯一。

### <a name="location--prefixpathport"></a>LOCATION = *`'<prefix>://<path[:port]>'`*

提供连接协议和外部数据源的路径。

| 外部数据源        | 位置前缀 | 位置路径                                         |
| --------------------------- | --------------- | ----------------------------------------------------- |
| Azure Data Lake Store Gen 1 | `adl`           | `<storage_account>.azuredatalake.net`                 |
| Azure Data Lake Store Gen 2 | `abfs[s]`       | `<container>@<storage_account>.dfs.core.windows.net`  |
| Azure V2 存储帐户    | `wasb[s]`       | `<container>@<storage_account>.blob.core.windows.net` |

位置路径：

- `<container>` = 保存数据的存储帐户的容器。 根容器是只读的，数据无法写回容器。
- `<storage_account>` = Azure 资源的存储帐户名称。

设置位置时的其他说明和指南：

- 默认选项是在预配 Azure Data Lake Storage Gen 2 时使用 `enable secure SSL connections`。 如果此选项已启用，必须在选择安全 TSL/SSL 连接时使用 `abfss`。 请注意，`abfss` 也适用于不安全的 TLS/SSL 连接。
- 创建对象时，Azure Synapse 不会验证外部数据源是否存在。 . 要进行验证，请使用外部数据源创建外部表。
- 查询 Hadoop 时，所有表使用相同的外部数据源，以确保查询语义一致。
- 建议使用 `wasbs`，因为将使用安全的 TLS 连接发送数据
- 使用 wasb:// 接口通过 PolyBase 访问数据时，Azure V2 存储帐户不支持分层命名空间。

### <a name="credential--credential_name"></a>CREDENTIAL = credential_name

指定用于对外部数据源进行身份验证的数据库范围凭据。

创建凭证时的其他说明和指导：

- 若要从 Azure 存储或 Azure Data Lake Store (ADLS) Gen 2 将数据加载到 Azure Synapse Analytics，请使用 Azure 存储密钥。
- 只有在数据得到保护的情况下才需要 `CREDENTIAL`。 允许匿名访问的数据集不需要 `CREDENTIAL`。

要创建数据库范围凭据，请参阅 [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]。

### <a name="type--hadoop"></a>TYPE = HADOOP

指定要配置的外部数据源的类型。 此参数并非总是必需的。

- 如果外部数据源为 Azure 存储、ADLS Gen 1 或 ADLS Gen 2，请使用 HADOOP。

有关使用 `TYPE` = `HADOOP` 从 Azure 存储加载数据的示例，请参阅[创建外部数据源以使用服务主体引用 Azure Data Lake Store Gen 1 或 Azure Data Lake Store Gen 2](#b-create-external-data-source-to-reference-azure-data-lake-store-gen-1-or-2-using-a-service-principal)。

## <a name="permissions"></a>权限

需要对数据库拥有 `CONTROL` 权限。

## <a name="locking"></a>锁定

对 `EXTERNAL DATA SOURCE` 对象采用共享锁。

## <a name="security"></a>安全性

PolyBase 支持大多数外部数据源的基于代理的身份验证。 创建数据库范围凭据以创建代理帐户。

连接到 SQL Server 大数据群集中的存储或数据池时，会将用户的凭据传递到后端系统。 在数据池本身中创建登录名以启用直通身份验证。

目前，不支持类型为 `HADOOP` 的 SAS 令牌。 它仅在使用存储帐户访问密钥时才受支持。 尝试创建类型为 `HADOOP` 的外部数据源和使用 SAS 凭据失败，并显示以下错误：

`Msg 105019, Level 16, State 1 - EXTERNAL TABLE access failed due to internal error: 'Java exception raised on call to HdfsBridge_Connect. Java exception message: Parameters provided to connect to the Azure storage account are not valid.: Error [Parameters provided to connect to the Azure storage account are not valid.] occurred while accessing external file.'`

## <a name="examples"></a>示例：

### <a name="a-create-external-data-source-to-access-data-in-azure-storage-using-the-wasb-interface"></a>A. 创建外部数据源以使用 wasb:// 接口访问 Azure 存储中的数据
在本示例中，外部数据源是名为 `logs` 的 Azure V2 存储帐户。 容器名称为 `daily`。 Azure 存储外部数据源仅用于数据传输。 它不支持谓词下推。 通过 `wasb://` 接口访问数据时，不支持分层命名空间。

本示例演示如何创建数据库范围凭据以用于对 Azure 存储进行身份验证。 在数据库凭据机密中指定 Azure 存储帐户密钥。 可以在数据库范围凭据标识中指定任何字符串，因为在对 Azure 存储进行身份验证的过程中不会使用它。

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
  IDENTITY = '<my_account>',
  SECRET = '<azure_storage_account_key>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyAzureStorage
WITH
  ( LOCATION = 'wasbs://daily@logs.blob.core.windows.net/' ,
    CREDENTIAL = AzureStorageCredential ,
    TYPE = HADOOP
  ) ;
```

### <a name="b-create-external-data-source-to-reference-azure-data-lake-store-gen-1-or-2-using-a-service-principal"></a>B. 创建外部数据源以使用服务主体引用 Azure Data Lake Store Gen 1 或 Azure Data Lake Store Gen 2

Azure Data Lake Store 连接可基于 ADLS URI 和 Azure Active Directory 应用程序的服务主体。 可以在[使用 Active Directory 域服务进行 Data Lake Store 身份验证][azure_ad]中找到有关创建此应用程序的文档。

```sql
-- If you do not have a Master Key on your DW you will need to create one.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

-- These values come from your Azure Active Directory Application used to authenticate to ADLS
CREATE DATABASE SCOPED CREDENTIAL ADLS_credential
WITH
  -- IDENTITY = '<clientID>@<OAuth2.0TokenEndPoint>' ,
  IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token' ,
  -- SECRET = '<KEY>'
  SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI=' 
;

-- For Gen 1 - Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen 1 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step
CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH
  ( LOCATION = 'adl://newyorktaxidataset.azuredatalakestore.net' ,
    CREDENTIAL = ADLS_credential ,
    TYPE = HADOOP
  ) ;

-- For Gen 2 - Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen 2 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step
CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH
  -- Please note the abfss endpoint when your account has secure transfer enabled
  ( LOCATION = 'abfss://data@newyorktaxidataset.dfs.core.windows.net' , 
    CREDENTIAL = ADLS_credential ,
    TYPE = HADOOP
  ) ;
```

### <a name="c-create-external-data-source-to-reference-azure-data-lake-store-gen-2-using-the-storage-account-key"></a>C. 创建外部数据源，以使用存储帐户密钥引用 Azure Data Lake Store Gen 2

```sql
-- If you do not have a Master Key on your DW you will need to create one.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

CREATE DATABASE SCOPED CREDENTIAL ADLS_credential
WITH
-- IDENTITY = '<storage_account_name>' ,
  IDENTITY = 'newyorktaxidata' ,
-- SECRET = '<storage_account_key>'
  SECRET = 'yz5N4+bxSb89McdiysJAzo+9hgEHcJRJuXbF/uC3mhbezES/oe00vXnZEl14U0lN3vxrFKsphKov16C0w6aiTQ=='
;

-- Note this example uses a Gen 2 secured endpoint (abfss)
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( LOCATION = 'abfss://2013@newyorktaxidataset.dfs.core.windows.net' ,
    CREDENTIAL = ADLS_credential ,
    TYPE = HADOOP
  ) ;
```

### <a name="d-create-external-data-source-to-reference-polybase-connectivity-to-azure-data-lake-store-gen-2-using-abfs"></a>D. 创建外部数据源以使用 abfs:// 引用与 Azure Data Lake Store Gen 2 的 Polybase 连接

连接到具有[托管标识](/azure/active-directory/managed-identities-azure-resources/overview
)机制的 Azure Data Lake Store Gen2 帐户时，无需指定密码。

```sql
-- If you do not have a Master Key on your DW you will need to create one
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>' ;

--Create database scoped credential with **IDENTITY = 'Managed Service Identity'**

CREATE DATABASE SCOPED CREDENTIAL msi_cred 
WITH IDENTITY = 'Managed Service Identity' ;

--Create external data source with abfss:// scheme for connecting to your Azure Data Lake Store Gen2 account

CREATE EXTERNAL DATA SOURCE ext_datasource_with_abfss 
WITH 
  ( TYPE = HADOOP , 
    LOCATION = 'abfss://myfile@mystorageaccount.dfs.core.windows.net' , 
    CREDENTIAL = msi_cred
  ) ;
```

## <a name="see-also"></a>另请参阅

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]
- [CREATE EXTERNAL FILE FORMAT (Transact-SQL)][create_eff]
- [CREATE EXTERNAL TABLE (Transact-SQL)][create_etb]
- [CREATE EXTERNAL TABLE AS SELECT (Azure Synapse Analytics)][create_etb_as_sel]
- [CREATE EXTERNAL TABLE AS SELECT (Azure Synapse Analytics)][create_tbl_as_sel]
- [sys.external_data_sources (Transact-SQL)][cat_eds]
- [使用共享访问签名 (SAS)][sas_token]

<!-- links to external pages -->
<!-- SQL Docs -->
[bulk_insert]: ./bulk-insert-transact-sql.md
[bulk_insert_example]: ./bulk-insert-transact-sql.md#f-importing-data-from-a-file-in-azure-blob-storage
[openrowset]: ../functions/openrowset-transact-sql.md

[create_dsc]: ./create-database-scoped-credential-transact-sql.md
[create_eff]: ./create-external-file-format-transact-sql.md
[create_etb]: https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source
[create_etb_as_sel]: ./create-external-table-as-select-transact-sql.md?view=azure-sqldw-latest
[create_tbl_as_sel]: ./create-table-as-select-azure-sql-data-warehouse.md?view=azure-sqldw-latest

[alter_eds]: ./alter-external-data-source-transact-sql.md

[cat_eds]: ../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md
<!-- PolyBase docs -->
[intro_pb]: ../../relational-databases/polybase/polybase-guide.md
[mongodb_pb]: ../../relational-databases/polybase/polybase-configure-mongodb.md
[connectivity_pb]:https://docs.microsoft.com/sql/database-engine/configure-windows/polybase-connectivity-configuration-transact-sql
[connection_options]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md
[hint_pb]: ../../relational-databases/polybase/polybase-pushdown-computation.md#force-pushdown
<!-- Elastic Query Docs -->
[intro_eq]: /azure/azure-sql/database/elastic-query-overview
[remote_eq]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[remote_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[sharded_eq]: /azure/azure-sql/database/elastic-query-getting-started
[sharded_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started

<!-- Azure Docs -->
[azure_ad]: /azure/data-lake-store/data-lake-store-authenticate-using-active-directory
[sas_token]: /azure/storage/storage-dotnet-shared-access-signature-part-1

::: moniker-end
::: moniker range=">=aps-pdw-2016||=sqlallproducts-allversions"

:::row:::
    :::column:::
        [SQL Server](create-external-data-source-transact-sql.md?view=sql-server-2017)
    :::column-end:::
    :::column:::
        [SQL 数据库](create-external-data-source-transact-sql.md?view=azuresqldb-current)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-data-source-transact-sql.md?view=azure-sqldw-latest)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-analytics-platform-system"></a>概述：分析平台系统

为 PolyBase 查询创建外部数据源。 外部数据源用于建立连接以及支持以下用例：使用 [PolyBase][intro_pb] 执行数据虚拟化和数据加载

## <a name="syntax"></a>语法

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
  ( [ LOCATION = '<prefix>://<path>[:<port>]' ]
    [ [ , ] CREDENTIAL = <credential_name> ]
    [ [ , ] TYPE = HADOOP ]
    [ [ , ] RESOURCE_MANAGER_LOCATION = '<resource_manager>[:<port>]' )
[ ; ]
```

## <a name="arguments"></a>参数

### <a name="data_source_name"></a>data_source_name

指定数据源的用户定义名称。 该名称在 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 的服务器中必须唯一。

### <a name="location--prefixpathport"></a>LOCATION = *`'<prefix>://<path[:port]>'`*

提供连接协议和外部数据源的路径。

| 外部数据源    | 位置前缀 | 位置路径                                         |
| ----------------------- | --------------- | ----------------------------------------------------- |
| Cloudera 或 Hortonworks | `hdfs`          | `<Namenode>[:port]`                                   |
| Azure 存储帐户   | `wasb[s]`       | `<container>@<storage_account>.blob.core.windows.net` |

位置路径：

- `<Namenode>` = Hadoop 群集中 `Namenode` 的计算机名称、名称服务 URI 或 IP 地址。 PolyBase 必须解析 Hadoop 群集使用的任何 DNS 名称。 <!-- For highly available Hadoop configurations, provide the Nameservice ID as the `LOCATION`. -->
- `port` = 外部数据源侦听的端口。 在 Hadoop 中，可以使用 `fs.defaultFS` 配置参数查找该端口。 默认值为 8020。
- `<container>` = 保存数据的存储帐户的容器。 根容器是只读的，数据无法写回容器。
- `<storage_account>` = Azure 资源的存储帐户名称。

设置位置时的其他说明和指南：

- 创建对象时，PDW 引擎不会验证外部数据源是否存在。 要进行验证，请使用外部数据源创建外部表。
- 查询 Hadoop 时，所有表使用相同的外部数据源，以确保查询语义一致。
- 建议使用 `wasbs`，因为将使用安全的 TLS 连接发送数据。
- 通过 wasb:// 与 Azure 存储帐户配合使用时，不支持分层命名空间。
- 要确保在 Hadoop `Namenode` 故障转移期间成功进行 PolyBase 查询，请考虑针对 Hadoop 群集的 `Namenode` 使用虚拟 IP 地址。 如果不这样做，请执行 [ALTER EXTERNAL DATA SOURCE][alter_eds] 命令以指向新位置。

### <a name="credential--credential_name"></a>CREDENTIAL = credential_name

指定用于对外部数据源进行身份验证的数据库范围凭据。

创建凭证时的其他说明和指导：

- 若要从 Azure 存储将数据加载到 Azure Synapse 或 PDW，请使用 Azure 存储密钥。
- 只有在数据得到保护的情况下才需要 `CREDENTIAL`。 允许匿名访问的数据集不需要 `CREDENTIAL`。

### <a name="type---hadoop-"></a>TYPE = [ HADOOP ]

指定要配置的外部数据源的类型。 此参数并非总是必需的。

- 如果外部数据源为 Cloudera、Hortonworks 或 Azure 存储，请使用 HADOOP。

有关使用 `TYPE` = `HADOOP` 从 Azure 存储加载数据的示例，请参阅[创建外部数据源以引用 Hadoop](#a-create-external-data-source-to-reference-hadoop)。

### <a name="resource_manager_location--resourcemanager_uriport"></a>RESOURCE_MANAGER_LOCATION = 'ResourceManager_URI[:port]'

连接到 Hortonworks 或 Cloudera 时配置此可选值。

定义 `RESOURCE_MANAGER_LOCATION` 后，查询优化器将根据成本做出决策以提高性能。 MapReduce 作业可用于将计算下推到 Hadoop。 指定 `RESOURCE_MANAGER_LOCATION` 可以显着减少 Hadoop 和 SQL 之间传输的数据量，从而提高查询性能。

如果未指定资源管理器，则会为 PolyBase 查询禁用到 Hadoop 的计算下推。

如果未指定端口，则使用“hadoop 连接”配置的当前设置选择默认值。

| Hadoop 连接 | 默认资源管理器端口 |
| ------------------- | ----------------------------- |
| 1                   | 50300                         |
| 2                   | 50300                         |
| 3                   | 8021                          |
| 4                   | 8032                          |
| 5                   | 8050                          |
| 6                   | 8032                          |
| 7                   | 8050                          |

有关受支持的 Hadoop 版本的完整列表，请参阅 [PolyBase 连接配置 (Transact-SQL)][connectivity_pb]。

> [!IMPORTANT]
> 创建外部数据源时，不会验证 `RESOURCE_MANAGER_LOCATION` 值。 每次尝试下推时，输入不正确的值都可能会导致查询执行失败，因为提供的值无法解析。

[创建外部数据源以引用启用了下推功能的 Hadoop](#b-create-external-data-source-to-reference-hadoop-with-push-down-enabled) 中提供了具体示例和详细指南。

## <a name="permissions"></a>权限

需要对 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中的数据库具有 `CONTROL` 权限。

> [!NOTE]
> 在以前版本的 PDW 中，创建外部数据源需要 `ALTER ANY EXTERNAL DATA SOURCE` 权限。

## <a name="locking"></a>锁定

对 `EXTERNAL DATA SOURCE` 对象采用共享锁。

## <a name="security"></a>安全性

PolyBase 支持大多数外部数据源的基于代理的身份验证。 创建数据库范围凭据以创建代理帐户。

目前，不支持类型为 `HADOOP` 的 SAS 令牌。 它仅在使用存储帐户访问密钥时才受支持。 尝试创建类型为 `HADOOP` 的外部数据源和使用 SAS 凭据失败，并显示以下错误：

`Msg 105019, Level 16, State 1 - EXTERNAL TABLE access failed due to internal error: 'Java exception raised on call to HdfsBridge_Connect. Java exception message: Parameters provided to connect to the Azure storage account are not valid.: Error [Parameters provided to connect to the Azure storage account are not valid.] occurred while accessing external file.'`

## <a name="examples"></a>示例：

### <a name="a-create-external-data-source-to-reference-hadoop"></a>A. 创建外部数据源以引用 Hadoop

若要创建外部数据源以引用 Hortonworks 或 Cloudera Hadoop 群集，请指定 Hadoop `Namenode` 的计算机名称或 IP 地址以及端口。 <!-- Provide the Nameservice ID as the `LOCATION` for highly available configurations. -->

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8050' ,
    TYPE = HADOOP
  ) ;
```

### <a name="b-create-external-data-source-to-reference-hadoop-with-push-down-enabled"></a>B. 创建外部数据源以引用 Hadoop 并启用下推

指定 `RESOURCE_MANAGER_LOCATION` 选项以便为 PolyBase 查询启用到 Hadoop 的下推计算。 启用后，PolyBase 会根据成本作出决策，以确定是否应将查询计算下推到 Hadoop。

```sql
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8020'
    TYPE = HADOOP
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
) ;
```

### <a name="c-create-external-data-source-to-reference-kerberos-secured-hadoop"></a>C. 创建外部数据源以引用受 Kerberos 保护的 Hadoop

若要验证 Hadoop 群集是否受 Kerberos 保护，请检查 Hadoop core-site.xml 中的 hadoop.security.authentication 属性值。 若要引用受 Kerberos 保护的 Hadoop 群集，必须指定包含 Kerberos 用户名和密码的数据库范围凭据。 数据库主密钥用于加密数据库范围凭据密钥。

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Kerberos user name and password.
CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
WITH
  IDENTITY = '<hadoop_user_name>' ,
  SECRET = '<hadoop_password>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyHadoopCluster
WITH
  ( LOCATION = 'hdfs://10.10.10.10:8050' ,
    CREDENTIAL = HadoopUser1 ,
    TYPE = HADOOP ,
    RESOURCE_MANAGER_LOCATION = '10.10.10.10:8050'
  ) ;
```

### <a name="d-create-external-data-source-to-access-data-in-azure-storage-using-the-wasb-interface"></a>D. 创建外部数据源以使用 wasb:// 接口访问 Azure 存储中的数据

在本示例中，外部数据源是名为 `logs` 的 Azure V2 存储帐户。 容器名称为 `daily`。 Azure 存储外部数据源仅用于数据传输。 它不支持谓词下推。 通过 `wasb://` 接口访问数据时，不支持分层命名空间。

此示例演示如何创建数据库范围凭据以用于对 Azure 存储进行身份验证。 在数据库凭据机密中指定 Azure 存储帐户密钥。 可以在数据库范围凭据标识中指定任何字符串，因为在对 Azure 存储进行身份验证的过程中不会使用它。

```sql
-- Create a database master key if one does not already exist, using your own password. This key is used to encrypt the credential secret in next step.
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo' ;

-- Create a database scoped credential with Azure storage account key as the secret.
CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
  IDENTITY = '<my_account>' ,
  SECRET = '<azure_storage_account_key>' ;

-- Create an external data source with CREDENTIAL option.
CREATE EXTERNAL DATA SOURCE MyAzureStorage
WITH
  ( LOCATION = 'wasbs://daily@logs.blob.core.windows.net/'
    CREDENTIAL = AzureStorageCredential
    TYPE = HADOOP
  ) ;
```


## <a name="see-also"></a>另请参阅

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)][create_dsc]
- [CREATE EXTERNAL FILE FORMAT (Transact-SQL)][create_eff]
- [CREATE EXTERNAL TABLE (Transact-SQL)][create_etb]
- [sys.external_data_sources (Transact-SQL)][cat_eds]
- [使用共享访问签名 (SAS)][sas_token]

<!-- links to external pages -->
<!-- SQL Docs -->
[bulk_insert]: ./bulk-insert-transact-sql.md
[bulk_insert_example]: ./bulk-insert-transact-sql.md#f-importing-data-from-a-file-in-azure-blob-storage
[openrowset]: ../functions/openrowset-transact-sql.md

[create_dsc]: ./create-database-scoped-credential-transact-sql.md
[create_eff]: ./create-external-file-format-transact-sql.md
[create_etb]: https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source
[create_etb_as_sel]: ./create-external-table-as-select-transact-sql.md?view=azure-sqldw-latest
[create_tbl_as_sel]: ./create-table-as-select-azure-sql-data-warehouse.md?view=azure-sqldw-latest

[alter_eds]: ./alter-external-data-source-transact-sql.md

[cat_eds]: ../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md
<!-- PolyBase docs -->
[intro_pb]: ../../relational-databases/polybase/polybase-guide.md
[mongodb_pb]: ../../relational-databases/polybase/polybase-configure-mongodb.md
[connectivity_pb]:https://docs.microsoft.com/sql/database-engine/configure-windows/polybase-connectivity-configuration-transact-sql
[connection_options]: ../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md
[hint_pb]: ../../relational-databases/polybase/polybase-pushdown-computation.md#force-pushdown
<!-- Elastic Query Docs -->
[intro_eq]: /azure/azure-sql/database/elastic-query-overview
[remote_eq]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[remote_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started-vertical
[sharded_eq]: /azure/azure-sql/database/elastic-query-getting-started
[sharded_eq_tutorial]: /azure/azure-sql/database/elastic-query-getting-started

<!-- Azure Docs -->
[azure_ad]: /azure/data-lake-store/data-lake-store-authenticate-using-active-directory
[sas_token]: /azure/storage/storage-dotnet-shared-access-signature-part-1

::: moniker-end
