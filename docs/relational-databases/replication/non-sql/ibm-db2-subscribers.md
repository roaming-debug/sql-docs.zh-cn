---
description: IBM DB2 Subscribers
title: IBM DB2 订阅服务器 | Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- non-SQL Server Subscribers, IBM DB2
- data types [SQL Server replication], non-SQL Server Subscribers
- IBM DB2 Subscribers
- mapping data types [SQL Server replication]
- heterogeneous Subscribers, IBM DB2
ms.assetid: a1a27b1e-45dd-4d7d-b6c0-2b608ed175f6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e044fa0e368893f0aaa1fe8175b306c2fc04c24b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88327063"
---
# <a name="ibm-db2-subscribers"></a>IBM DB2 Subscribers
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 支持通过 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Host Integration Server 附带的 OLE DB 访问接口对 IBM DB2/AS 400、DB2/MVS 和 DB2/通用数据库进行的推送订阅。  
  
## <a name="configuring-an-ibm-db2-subscriber"></a>配置 IBM DB2 订阅服务器  
 若要配置 IBM DB2 订阅服务器，请按下列步骤进行操作：  
  
1.  在分发服务器上安装最新版本的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] OLE DB Provider for DB2：  
  
    -   如果使用的是 [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] Enterprise Edition，请在 [SQL Server 下载](https://go.microsoft.com/fwlink/?LinkId=149256)网页的 **Related Downloads**（相关下载）部分中单击指向最新版本的 Microsoft SQL Server 功能包的链接。 在 **Microsoft SQL Server Feature Pack**（Microsoft SQL Server 功能包）网页中，搜索 **Microsoft OLE DB Provider for DB2**。  
  
    -   如果使用的是 [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] Standard Edition，则安装最新版本的 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Host [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] (HIS) 服务器，其中包含了该访问接口。  
  
     除了安装访问接口外，我们还建议你安装数据访问工具（默认情况下，它随 [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] Enterprise Edition 的下载包一起安装），下一步将使用该工具。 有关安装和使用数据访问工具的详细信息，请参阅访问接口文档或 HIS 文档。  
  
2.  为订阅服务器创建连接字符串。 可以在任何文本编辑器中创建连接字符串，但是建议您使用数据访问工具来创建。 在数据访问工具中创建字符串：  
  
    1.  依次单击 **“开始”**、 **“程序”**、 **“Microsoft OLE DB Provider for DB2”**、 **“数据访问工具”**。  
  
    2.  在 **“数据访问工具”** 中，按照步骤提供 DB2 服务器的相关信息。 当使用此工具完成操作后，就创建了一个包含相关连接字符串的通用数据链接 (UDL)（复制实际使用的不是 UDL，而是连接字符串）。  
  
    3.  访问连接字符串：右键单击数据访问工具中的 UDL 并选择 **“显示连接字符串”**。  
  
     连接字符串类似于（使用换行符是为了方便阅读）：  
  
    ```  
    Provider=DB2OLEDB;Initial Catalog=MY_SUBSCRIBER_DB;Network Transport Library=TCP;Host CCSID=1252;  
    PC Code Page=1252;Network Address=MY_SUBSCRIBER;Network Port=50000;Package Collection=MY_PKGCOL;  
    Default Schema=MY_SCHEMA;Process Binary as Character=False;Derive Parameters=False;Units of Work=RUW;DBMS Platform=DB2/NT;  
    Persist Security Info=False;Connection Pooling=True;  
    ```  
  
     字符串中的大多数选项都特定于正在配置的 DB2 服务器，但 `Process Binary as Character` 和 `Derive Parameters` 选项应始终设置为 `False`。 需要为 `Initial Catalog` 选项指定值以标识订阅数据库。 创建订阅时将在新建订阅向导中输入该连接字符串。  
  
3.  创建快照发布或事务发布，为非[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 订阅服务器启用该发布，然后为订阅服务器创建推送订阅。 有关详细信息，请参阅 [为非 SQL Server 订阅服务器创建订阅](../../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)。  
  
4.  （可选）为一个或多个项目指定自定义创建脚本。 在发布表时，会为该表创建一个 `CREATE TABLE` 脚本。 对于非[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 订阅服务器，该脚本采用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 方言创建，然后由分发代理翻译成较通用的 SQL 方言后应用于订阅服务器。 若要指定自定义创建脚本，请修改现有的 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本或创建使用 DB2 SQL 方言的完整脚本；如果创建 DB2 脚本，请使用 **bypass_translation** 指令，这样分发代理无需转换脚本即可将其应用于订阅服务器。  
  
     修改脚本的原因很多，但最常见的原因是要更改数据类型映射。 有关详细信息，请参阅本主题的“数据类型映射注意事项”部分。 如果修改 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本，应只限于对数据类型映射进行更改（而且脚本不应包含任何注释）。 如果需要进行重大更改，请创建 DB2 脚本。  
  
     **修改项目脚本并将其作为自定义创建脚本提供**  
  
    1.  为发布生成快照后，定位到该发布的快照文件夹。  
  
    2.  找到与项目同名的 `.sch` 文件，例如 `MyArticle.sch`。  
  
    3.  使用记事本或其他文本编辑器打开此文件。  
  
    4.  修改此文件并将其保存到其他目录。  
  
    5.  执行 `sp_changearticle`，为 *creation_script* 属性指定文件路径和文件名。 有关详细信息，请参阅 [sp_changearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)。  
  
     **创建项目脚本并将其作为自定义创建脚本提供**  
  
    1.  使用 DB2 SQL 方言创建项目脚本。 请确保该文件的第一行为 **bypass_translation**，并且该行不包含任何其他内容。  
  
    2.  执行 sp_changearticle，为 *creation_script* 属性指定文件路径和文件名。  
  
## <a name="considerations-for-ibm-db2-subscribers"></a>IBM DB2 订阅服务器的注意事项  
 除了 [Non-SQL Server Subscribers](../../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)主题中所述注意事项外，在向 DB2 订阅服务器复制时，请考虑以下问题：  
  
-   每个复制的表的数据和索引都分配到一个 DB2 表空间。 DB2 表空间的页大小决定表空间中表的最大列数和最大行大小。 应确保与复制的表关联的表空间与表的复制列数和最大行大小适当。  
  
-   如果表中有一个或多个主键列的数据类型为 DECIMAL(32-38, 0-38) 或 NUMERIC(32-38, 0-38)，请勿使用事务复制向 DB2 订阅服务器发布表。 事务复制使用主键标识行，这可能会导致失败，因为这些数据类型在订阅服务器上均映射为 VARCHAR(41)。 可以使用快照复制发布包含使用这些数据类型的主键的表。  
  
-   如果要在订阅服务器上预创建表，而不是让复制创建这些表，请使用“仅支持复制”选项。 有关详细信息，请参阅 [初始化事务订阅（不使用快照）](../../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)中手动初始化订阅。  
  
-   与 DB2 相比，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 允许使用更长的表名称和列名称：  
  
    -   如果发布数据库中有表的名称长度超过订阅服务器上 DB2 版本所支持的长度，请为 destination_table 项目属性指定一个备用名称。 有关创建发布时设置属性的详细信息，请参阅[创建发布](../../../relational-databases/replication/publish/create-a-publication.md)和[定义项目](../../../relational-databases/replication/publish/define-an-article.md)。  
  
    -   不能指定备用列名称。 必须确保已发布的表所包含的列名称长度均不超过订阅服务器上 DB2 版本所支持的长度。  
  
## <a name="mapping-data-types-from-sql-server-to-ibm-db2"></a>将 SQL Server 中的数据类型映射到 IBM DB2  
 下表显示了将数据复制到运行 IBM DB2 的订阅服务器上时使用的数据类型映射。  
  
|SQL Server 数据类型|IBM DB2 数据类型|  
|--------------------------|-----------------------|  
|**bigint**|DECIMAL(19,0)|  
|**binary(1-254)**|CHAR(1-254) FOR BIT DATA|  
|**binary(255-8000)**|VARCHAR(255-8000) FOR BIT DATA|  
|**bit**|SMALLINT|  
|**char(1-254)**|CHAR(1-254)|  
|**char(255-8000)**|VARCHAR(255-8000)|  
|**date**|DATE|  
|**datetime**|TIMESTAMP|  
|**datetime2(0-7)**|VARCHAR(27)|  
|**datetimeoffset(0-7)**|VARCHAR(34)|  
|**decimal(1-31, 0-31)**|DECIMAL(1-31, 0-31)|  
|**decimal(32-38, 0-38)**|VARCHAR(41)|  
|**float(53)**|DOUBLE|  
|**float**|FLOAT|  
|**地理**|IMAGE|  
|**geometry**|IMAGE|  
|**hierarchyid**|IMAGE|  
|**图像**|VARCHAR(0) FOR BIT DATA*|  
|**into**|INT|  
|**money**|DECIMAL(19,4)|  
|**nchar(1-4000)**|VARCHAR(1-4000)|  
|**ntext**|VARCHAR(0)*|  
|**numeric(1-31, 0-31)**|DECIMAL(1-31,0-31)|  
|**numeric(32-38, 0-38)**|VARCHAR(41)|  
|**nvarchar(1-4000)**|VARCHAR(1-4000)|  
|**nvarchar(max)**|VARCHAR(0)*|  
|**real**|REAL|  
|**smalldatetime**|TIMESTAMP|  
|**smallint**|SMALLINT|  
|**smallmoney**|DECIMAL(10,4)|  
|**sql_variant**|空值|  
|**sysname**|VARCHAR(128)|  
|**text**|VARCHAR(0)*|  
|**time(0-7)**|VARCHAR(16)|  
|**timestamp**|CHAR(8) FOR BIT DATA|  
|**tinyint**|SMALLINT|  
|**uniqueidentifier**|CHAR(38)|  
|**varbinary(1-8000)**|VARCHAR(1-8000) FOR BIT DATA|  
|**varchar(1-8000)**|VARCHAR(1-8000)|  
|**varbinary(max)**|VARCHAR(0) FOR BIT DATA*|  
|**varchar(max)**|VARCHAR(0)*|  
|**xml**|VARCHAR(0)*|  
  
* 有关映射到 VARCHAR(0) 的详细信息，请参阅下一部分。  
  
### <a name="data-type-mapping-considerations"></a>数据类型映射注意事项  
 复制到 DB2 订阅服务器时，请考虑下列数据类型映射问题：  
  
-   在将  char、varchar、binary 和 varbinary 分别映射到 DB2 CHAR、VARCHAR、CHAR FOR BIT DATA 和 VARCHAR FOR BIT DATA 时，复制会将 DB2 数据类型的长度设置为与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 类型的长度相同[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。  
  
     这样，只要 DB2 页大小约束足够大，能容纳最大行大小，就可以在订阅服务器上成功创建已生成的表。 请确保用于访问 DB2 数据库的登录帐户具有访问表空间的权限，这些表空间具有足够大小可以存放向 DB2 复制的表。  
  
-   DB2 可以支持 32 KB 大的 VARCHAR 列，因此可以将某些 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 大型对象列适当映射到 DB2 VARCHAR 列。 但是，复制用于 DB2 的 OLE DB 访问接口不支持将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 大型对象映射到 DB2 大型对象。 因此，在生成的创建脚本中，将  text、varchar(max)、ntext 和 nvarchar(max) 列映射到 VARCHAR(0)[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 在将脚本应用于订阅服务器之前，必须将长度值 0 更改为一个适当的值。 如果不更改数据类型长度，则当试图在 DB2 订阅服务器上创建表时，DB2 会产生错误 604（错误 604 表示数据类型的精度或长度属性无效）。  
  
     请根据您对要复制的源表的了解来确定是否适于将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 大型对象映射到可变长度的 DB2 项，并在自定义创建脚本中指定适当的最大长度。 有关指定自定义创建脚本的信息，请参阅本主题中“配置 IBM DB2 订阅服务器”部分的步骤 5。  
  
    > [!NOTE]  
    >  DB2 类型的指定长度（与其他列长度组合时）不能超过由为表数据分配的 DB2 表空间限定的最大行大小。  
  
     如果对于某个大型对象列没有适当的映射，请考虑对项目使用列筛选，以避免复制该列。 有关详细信息，请参阅[筛选已发布数据](../../../relational-databases/replication/publish/filter-published-data.md)。  
  
-   在将  nchar 和 nvarchar 复制到 DB2 的 CHAR 和 VARCHAR 时，复制为 DB2 类型使用的说明符长度与为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 类型相同[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 但是，数据类型长度对于生成的 DB2 表而言可能太小。  
  
     在某些 DB2 环境中， char 数据项并不限于单字节字符，对于 CHAR 或 VARCHAR 项的长度，必须考虑到这一点[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 如果需要，还必须考虑到“移入”  字符和“移出”  字符。 如果要复制包含 **nchar** 和 **nvarchar** 列的表，您可能需要在自定义创建脚本中为数据类型指定更大的最大长度。 有关指定自定义创建脚本的信息，请参阅本主题中“配置 IBM DB2 订阅服务器”部分的步骤 5。  
  
## <a name="see-also"></a>另请参阅  
 [Non-SQL Server Subscribers](../../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)   
 [订阅发布](../../../relational-databases/replication/subscribe-to-publications.md)  
  
  
