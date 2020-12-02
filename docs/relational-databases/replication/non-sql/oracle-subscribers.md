---
description: Oracle 订阅服务器
title: Oracle 订阅服务器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- data types [SQL Server replication], non-SQL Server Subscribers
- Oracle Subscribers [SQL Server replication]
- non-SQL Server Subscribers, Oracle
- heterogeneous Subscribers, Oracle
- mapping data types [SQL Server replication]
ms.assetid: 591c0313-82ce-4689-9fc1-73752ff122cf
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3e7b6abc6b235950fef107a870e53d827dee6f5e
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88455595"
---
# <a name="oracle-subscribers"></a>Oracle 订阅服务器
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  从 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]开始， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 就可通过 Oracle 提供的 Oracle OLE DB 访问接口支持到 Oracle 的推送订阅。  
  
## <a name="configuring-an-oracle-subscriber"></a>配置 Oracle 订阅服务器  
 若要配置 Oracle 订阅服务器，请按照以下步骤进行：  
  
1.  在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 分发服务器上安装和配置 Oracle 客户端网络软件和 Oracle OLE DB 访问接口，以便分发服务器可以连接到 Oracle 订阅服务器。 Oracle 客户端网络软件应是可用的最新版本。 Oracle 建议用户安装最新版本的客户端软件。 因此，客户端软件的版本通常比数据库软件更高。 安装此软件的最直接方法是使用 Oracle Client 光盘上的 Oracle Universal Installer。 在 Oracle Universal Installer 中，您需要提供以下信息：  
  
    |信息|说明|  
    |-----------------|-----------------|  
    |Oracle 主目录|这是 Oracle 软件的安装目录的路径。 接受默认路径（C:\oracle\ora90 或类似路径），或输入另一个路径。 有关 Oracle 主目录的详细信息，请参阅本主题后面的“Oracle 主目录注意事项”部分。|  
    |Oracle 主目录名称|Oracle 主目录路径的别名。|  
    |安装类型|在 Oracle 10g 中，选择 **Runtime** 或 **Administrator** 安装选项。|  
  
2.  创建订阅服务器的 TNS 名称。 TNS（透明网络底层）是 Oracle 数据库使用的通信层。 在网络上使用 TNS 服务名称来辨别 Oracle 数据库实例。 TNS 服务名称是在配置与 Oracle 数据库的连接时分配的。 复制使用 TNS 服务名称标识订阅服务器并建立连接。  
  
     完成 Oracle Universal Installer 之后，使用 Net Configuration Assistant 配置网络连接。 必须提供四部分信息以配置网络连接。 在设置数据库和侦听器时，Oracle 数据库管理员将配置网络。如果您没有此信息，应由数据库管理员提供此信息。 必须执行以下操作：  
  
    |操作|说明|  
    |------------|-----------------|  
    |标识数据库|标识数据库的方法有两种。 第一种方法是使用 Oracle 系统标识符 (SID)，该方法在所有 Oracle 版本中均可用。 第二种方法是使用服务名称，该名称从 Oracle 8.0 版本开始才可用。 这两种方法都使用一个在创建数据库时所配置的值，并且要注意，客户端网络配置所使用的命名方法要与管理员在配置数据库的侦听器时使用的命名方法相同。|  
    |标识数据库的网络别名|必须指定网络别名，该别名将用来访问 Oracle 数据库。 实际上，网络别名是指向在创建数据库时所配置的远程 SID 或服务名称的指针。在不同的 Oracle 版本和产品中存在多种引用名称，包括 Net 服务名称和 TNS 别名。 在登录时，SQL*Plus 将提示您输入此别名来作为“Host String”参数。|  
    |选择网络协议|选择要支持的相应协议。 大多数应用程序使用 TCP。|  
    |指定主机信息以标识数据库侦听器|主机是正在运行 Oracle 侦听器的计算机的名称或 DNS 别名，该计算机通常是该数据库所驻留的计算机。 对于某些协议，必须提供其他信息。 例如，如果选择 TCP，则必须提供相应的端口，以便侦听器侦听针对目标数据库的连接请求。 默认 TCP 配置使用 1521 端口。|  
  
3.  创建快照发布或事务发布，为非[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 订阅服务器启用该发布，然后为订阅服务器创建推送订阅。 有关详细信息，请参阅 [为非 SQL Server 订阅服务器创建订阅](../../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)。  

### <a name="setting-directory-permissions"></a>设置目录权限  
 分发服务器上的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务运行所用的帐户必须具有对 Oracle 客户端网络软件安装目录（以及所有子目录）的读取和执行权限。  
  
### <a name="testing-connectivity-between-the-sql-server-distributor-and-the-oracle-publisher"></a>测试 SQL Server 分发服务器和 Oracle 发布服务器之间的连接  
 在 Net Configuration Assistant 快要结束的时候，可能会出现一个用于测试到 Oracle 订阅服务器的连接的选项。 在测试连接之前，请确保 Oracle 数据库实例已联机，并且 Oracle 侦听器正在运行。 如果测试不成功，请与负责所要连接的数据库的 Oracle DBA 联系。  
  
 成功连接到 Oracle 订阅服务器后，尝试使用为订阅的分发代理配置的同一帐户和密码登录到该数据库：  
  
1.  单击 **“启动”** ，再单击 **“运行”** 。  
  
2.  键入 `cmd` ，然后单击 **“确定”** 。  
  
3.  在命令提示符处，键入：  
  
     `sqlplus <UserSchemaLogin>/<UserSchemaPassword>@<NetServiceName>`  
  
     例如： `sqlplus replication/$tr0ngPasswerd@Oracle90Server`  
  
4.  如果网络配置成功，将成功登录，并显示 `SQL` 提示符。  
  
### <a name="considerations-for-oracle-home"></a>Oracle 主目录注意事项  
 Oracle 支持并行安装应用程序二进制文件，但是，在给定时间复制过程只能使用一组二进制文件。 每组二进制文件都与一个 Oracle 主目录关联。二进制文件位于 %ORACLE_HOME%\bin 目录中。 当复制连接到 Oracle 订阅服务器时，必须确保使用正确的二进制文件组（特别是客户端网络软件的最新版本）。  
  
 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理服务所使用的帐户登录到分发服务器，并设置相应的环境变量。 应将 %ORACLE_HOME% 变量设置为引用在安装客户端网络软件时所指定的安装点。 %PATH% 必须包括 %ORACLE_HOME% \bin 目录作为所遇到的第一个 Oracle 项。 有关设置环境变量的信息，请参阅 Windows 文档。  
  
> [!NOTE]  
>  如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 分发服务器上有不止一个 Oracle 主目录，请确保分发代理使用的是最新的 Oracle OLE DB 访问接口。 在某些情况中，当您在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 分发服务器上更新客户端组件时，Oracle 默认不更新 OLE DB 访问接口。 请卸载旧的 OLE DB 访问接口并安装最新的 OLE DB 访问接口。 有关安装和卸载该访问接口的详细信息，请参阅 Oracle 文档。  
  
## <a name="considerations-for-oracle-subscribers"></a>Oracle 订阅服务器注意事项  
 当复制到 Oracle 订阅服务器时，除了主题 [Non-SQL Server Subscribers](../../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)中介绍的注意事项外，还需考虑以下问题：  
  
-   Oracle 将空字符串和 NULL 值均视为 NULL。 这在将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 列定义为 NOT NULL 并将该列复制到 Oracle 订阅服务器时很重要。 将更改应用到 Oracle 订阅服务器时，为避免失败，必须执行以下操作之一：  
  
    -   确保不要将空字符串作为列值插入到已发布表中。  
  
    -   如果可以接受分发代理历史记录日志中的失败通知并继续处理，则可以对分发代理使用 -SkipErrors 参数。 指定 Oracle 错误代码 1400 (**-SkipErrors1400**)。  
  
    -   修改生成的创建表脚本，从任何可能包含关联空字符串的字符列中删除 NOT NULL 属性，并使用 @creation_script sp_addarticle [的](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)参数将修改后的脚本作为项目的自定义创建脚本提供。  
  
-   Oracle 订阅服务器支持 0x4071 架构选项。 有关架构选项的详细信息，请参阅 [sp_addarticle (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)。  
  
## <a name="mapping-data-types-from-sql-server-to-oracle"></a>将数据类型从 SQL Server 映射到 Oracle  
 下表显示将数据复制到运行 Oracle 的订阅服务器时使用的数据类型映射。  
  
|SQL Server 数据类型|Oracle 数据类型|  
|--------------------------|----------------------|  
|**bigint**|NUMBER(19,0)|  
|**binary(1-2000)**|RAW(1-2000)|  
|**binary(2001-8000)**|BLOB|  
|**bit**|NUMBER(1)|  
|**char(1-2000)**|CHAR(1-2000)|  
|**char(2001-4000)**|VARCHAR2(2001-4000)|  
|**char(4001-8000)**|CLOB|  
|**date**|DATE|  
|**datetime**|DATE|  
|**datetime2(0-7)**|TIMESTAMP(7)（对于 Oracle 9 和 Oracle 10）；VARCHAR(27)（对于 Oracle 8）|  
|**datetimeoffset(0-7)**|TIMESTAMP(7) WITH TIME ZONE（对于 Oracle 9 和 Oracle 10）；VARCHAR(34)（对于 Oracle 8）|  
|**decimal(1-38, 0-38)**|NUMBER(1-38, 0-38)|  
|**float(53)**|FLOAT|  
|**float**|FLOAT|  
|**地理**|BLOB|  
|**geometry**|BLOB|  
|**hierarchyid**|BLOB|  
|**图像**|BLOB|  
|**int**|NUMBER(10,0)|  
|**money**|NUMBER(19,4)|  
|**nchar(1-1000)**|CHAR(1-1000)|  
|**nchar(1001-4000)**|NCLOB|  
|**ntext**|NCLOB|  
|**numeric(1-38, 0-38)**|NUMBER(1-38, 0-38)|  
|**nvarchar(1-1000)**|VARCHAR2(1-2000)|  
|**nvarchar(1001-4000)**|NCLOB|  
|**nvarchar(max)**|NCLOB|  
|**real**|REAL|  
|**smalldatetime**|DATE|  
|**smallint**|NUMBER(5,0)|  
|**smallmoney**|NUMBER(10,4)|  
|**sql_variant**|空值|  
|**sysname**|VARCHAR2(128)|  
|**text**|CLOB|  
|**time(0-7)**|VARCHAR(16)|  
|**timestamp**|RAW(8)|  
|**tinyint**|NUMBER(3,0)|  
|**uniqueidentifier**|CHAR(38)|  
|**varbinary(1-2000)**|RAW(1-2000)|  
|**varbinary(2001-8000)**|BLOB|  
|**varchar(1-4000)**|VARCHAR2(1-4000)|  
|**varchar(4001-8000)**|CLOB|  
|**varbinary(max)**|BLOB|  
|**varchar(max)**|CLOB|  
|**xml**|NCLOB|  
  
## <a name="see-also"></a>另请参阅  
 [Non-SQL Server Subscribers](../../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)   
 [订阅发布](../../../relational-databases/replication/subscribe-to-publications.md)  
  
  
