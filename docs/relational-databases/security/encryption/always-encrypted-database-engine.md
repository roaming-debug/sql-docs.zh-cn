---
title: Always Encrypted | Microsoft Docs
description: Always Encrypted 概述，使用 Always Encrypted，可在 SQL Server 和 Azure SQL 数据库中实现透明客户端加密和机密计算
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], Always Encrypted
- Always Encrypted
- TCE Always Encrypted
- Always Encrypted, about
- SQL13.SWB.COLUMNMASTERKEY.CLEANUP.F1
ms.assetid: 54757c91-615b-468f-814b-87e5376a960f
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1f097d500c1d1b0a035f2bb0e737214d65803414
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477718"
---
# <a name="always-encrypted"></a>Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

  ![Always Encrypted](../../../relational-databases/security/encryption/media/always-encrypted.png "Always Encrypted")  
  
 Always Encrypted 功能旨在保护 [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] 或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中存储的敏感数据，如信用卡号或身份证号（例如美国社会安全号码）。 始终加密允许客户端对客户端应用程序内的敏感数据进行加密，并且永远不向 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] （[!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]）显示加密密钥。 因此，Always Encrypted 分隔了拥有数据且有权查看它的人员与管理数据但没有访问权限的人员。 Always Encrypted 确保本地数据库管理员、云数据库操作员或其他高特权但未经授权的用户无法访问加密的数据，使客户能够放心地将敏感数据存储在不受其直接控制的区域。 这样，组织便可以将数据存储在 Azure 中，并将本地数据库的管理权限委托给第三方，或者降低其自身 DBA 员工的安全核查要求。

 Always Encrypted 通过启用 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 来提供机密计算功能，可处理针对加密数据的某些查询，同时保持数据的机密性，并提供上述安全优势。 在 [!INCLUDE[ssSQL15](../../../includes/sssql15-md.md)]、[!INCLUDE[sssSQLv14](../../../includes/sssqlv14-md.md)] 和 [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)]中，Always Encrypted 通过确定性加密支持相等比较。 请参阅[选择确定性加密或随机加密](#selecting--deterministic-or-randomized-encryption)。 

  > [!NOTE] 
  > 在 [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] 中，安全 Enclave 通过模式匹配、其他比较运算符和就地加密，大幅提升了 Always Encrypted 的机密计算能力。 请参阅[具有安全 Enclave 的 Always Encrypted](always-encrypted-enclaves.md)。

 始终加密使向加密对应用程序透明。 安装在客户端计算机上的启用始终加密的驱动程序通过在客户端应用程序中对敏感数据进行加密和解密来实现此目标。 该驱动程序先对敏感列中的数据进行加密，然后再将该数据传递到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]，并且自动重写查询以便保留应用程序的语义。 同样，该驱动程序透明地解密存储在查询结果中包含的加密数据库列中的数据。  
  
 从 [!INCLUDE[ssSQL15](../../../includes/sssql15-md.md)] 开始的所有 [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] 版本和 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 的所有服务层均提供 Always Encrypted。 （[!INCLUDE[ssSQL15_md](../../../includes/sssql15-md.md)] SP1 之前，仅 Enterprise Edition 具有始终加密功能。）有关包含始终加密的第 9 频道演示内容，请参阅 [Keeping Sensitive Data Secure with Always Encrypted](https://channel9.msdn.com/events/DataDriven/SQLServer2016/AlwaysEncrypted)（使用始终加密来保护敏感数据）。  

  
## <a name="typical-scenarios"></a>典型方案  
  
### <a name="client-and-data-on-premises"></a>客户端和数据都在本地  
 客户在营业地本地运行客户端应用程序和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 客户想要委托外部供应商来管理 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 为了保护 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中存储的敏感数据，客户使用始终加密来确保数据库管理员与应用程序管理员的职责分离。 客户在客户端应用程序可以访问的受信任密钥存储中存储 Always Encrypted 密钥的纯文本值。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理员无权访问密钥，因此，无法解密 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中存储的敏感数据。  
  
### <a name="client-on-premises-with-data-in-azure"></a>客户端在本地，数据在 Azure 中  
 客户在营业地运行本地客户端应用程序。 应用程序操作 Azure 中托管的数据库中存储的敏感数据（Microsoft Azure 上虚拟机中运行的[!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ）。 客户使用始终加密功能并在本地托管的受信任密钥存储中存储始终加密密钥，以确保 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 云管理员无权访问敏感数据。  
  
### <a name="client-and-data-in-azure"></a>客户端和数据都在 Azure 中  
 客户在 Microsoft Azure 中托管了一个客户端应用程序（例如辅助角色或 web 角色），该应用程序对在 Azure 中托管的数据库中（在 Microsoft Azure 的虚拟机上运行的 SQL 数据库或 SQL Server）存储的敏感数据进行操作。 由于数据和密钥对托管客户端层的平台的云管理员是公开的，因此 Always Encrypted 不提供针对云管理员的完全数据隔离，但是客户仍将从降低安全攻击范围中受益（数据库中的数据始终是加密的）。  
 
## <a name="how-it-works"></a>工作方式

可以为包含敏感数据的单个数据库列配置“始终加密”功能。 设置列的加密信息时，将指定用于保护列中的数据的加密算法和加密密钥的信息。 “始终加密”功能使用两种类型的密钥：列加密密钥和列主密钥。 列加密密钥用于对加密列中的数据进行加密。 列主密钥是对一个或多个列加密密钥进行加密的密钥保护密钥。 

数据库引擎在数据库元数据中存储每个列的加密配置。 但是请注意，数据库引擎不会存储或使用纯文本的任一类型密钥。 它只存储列加密密钥的加密值和存储在外部受信任密钥存储中（如 Azure 密钥保管库、客户端计算机上的 Windows 证书存储或硬件安全模块）的列主密钥的位置信息。

若要访问以纯文本形式存储在加密列中的数据，应用程序必须使用启用“始终加密”功能的客户端驱动程序。 当应用程序发出参数化查询时，驱动程序以用户透明的方式与数据库引擎协作来确定哪些参数由于指向加密列而应该被加密。 对于每个需要加密的参数，驱动程序将获取有关列的列加密密钥的加密算法和加密值、参数目标以及相应的列主密钥的位置信息。

接下来，驱动程序将联系包含列主密钥的密钥存储，以便进行解密加密列的加密密钥值，然后使用纯文本列加密密钥来加密参数。 产生的纯文本列加密密钥已缓存，以便在以后使用相同的列加密密钥时减少对密钥存储的往返访问次数。 驱动程序将指向加密列的参数的纯文本值替换为它们的加密值，并将查询发送到服务器以进行处理。

服务器计算结果集，对于结果集中包含的任何加密列，驱动程序将附加列的加密元数据，包括有关加密算法和相应密钥的信息。 驱动程序首先尝试在本地缓存中查找纯文本列加密密钥，如果无法在缓存中找到该密钥，则只访问列主密钥。 接下来，驱动程序解密结果并将纯文本值返回给应用程序。

 客户端驱动程序使用列主密钥存储提供程序与包含列主密钥的密钥存储进行交互，列主密钥存储提供程序是一个客户端软件组件，用于封装包含列主密钥的密钥存储。 常见类型的密钥存储的提供程序在 Microsoft 的客户端驱动程序库中提供，或作为单独的软件下载。 你也可以实现自己的提供程序。 Always Encrypted 功能（包括内置的列主密钥存储提供程序）因驱动程序库及其版本而异。 

有关如何使用具有特定客户端驱动程序的 Always Encrypted 开发应用程序的详细信息，请参阅[使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)。

## <a name="remarks"></a>备注

通过客户端驱动程序进行加密和解密。 这意味着使用“Always Encrypted”时，仅在服务器端发生的某些操作将不起作用。 示例包括通过 UPDATE、BULK INSERT(T-SQL)、SELECT INTO、INSERT..SELECT 将数据从一列复制到另一列。 

以下 UPDATE 示例尝试将数据从加密列移动到未加密列，而不将结果集返回客户端： 

```sql
update dbo.Patients set testssn = SSN
```

如果 SSN 是使用 Always Encrypted 加密的列，则上述更新语句将失败，并显示类似于以下内容的错误：

```
Msg 206, Level 16, State 2, Line 89
Operand type clash: char(11) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_1', column_encryption_key_database_name = 'ssn') collation_name = 'Latin1_General_BIN2' is incompatible with char
```

若要成功更新列，请执行以下操作：

1. 使用 SELECT 从 SSN 列中选择数据，并将其作为结果集存储在应用程序中。 这样可让应用程序（客户端驱动程序）解密列  。
2. 使用 INSERT 将结果集中的数据插入 SQL Server。 

 >[!IMPORTANT]
 > 在此方案中，数据在发送回服务器时不会加密，因为目标列是不接受加密数据的常规 varchar。 
  
## <a name="selecting-deterministic-or-randomized-encryption"></a><a name="selecting--deterministic-or-randomized-encryption"></a>选择确定性加密或随机加密  
 数据库引擎始终不会对加密列中存储的纯文本数据进行操作，但仍支持对已加密数据的某些查询，具体取决于列的加密类型。 始终加密支持两种类型的加密：随机加密和确定性加密。  
  
- 确定性加密始终对任何给定的纯文本值生成相同的加密值。 使用确定性加密允许对加密列进行点查找、等值联结、分组和建立索引。 但也可能允许未经授权的用户通过检查加密列中的模式来猜测有关加密值的信息，尤其是存在一个规模较小的可能加密值集合时，如 True/False 或 North/South/East/West 等区域。 确定性加密必须使用具有字符列的 binary2 排序顺序的列排序规则。

- 随机加密使用更不可预测的方式来加密数据。 随机加密更加安全，但不支持对加密列进行搜索、分组、索引和联接。

针对要用作搜索参数或分组参数的列使用确定性加密。 例如，政府 ID 号。 针对不会与其他记录分组在一起且不会用来联接表的数据（例如机密的调查注解）使用随机加密。
有关 Always Encrypted 加密算法的详细信息，请参阅 [Always Encrypted 加密](../../../relational-databases/security/encryption/always-encrypted-cryptography.md)。

## <a name="configuring-always-encrypted"></a>配置“始终加密”功能

 在数据库中“始终加密”功能的初始设置包括生成“始终加密”密钥、创建密钥元数据、配置所选数据库列的加密属性和/或加密可能已存在于需要加密的列中的数据。 请注意，Transact-SQL 不支持其中一些任务，需要使用客户端工具。 由于始终不会向服务器公开纯文本形式的 Always Encrypted 密钥和受保护敏感数据，因此数据库引擎不会参与密钥预配，也不会执行数据加密或解密操作。 可以使用 SQL Server Management Studio 或 PowerShell 来完成此类任务。 

|任务|SSMS|PowerShell|T-SQL|
|:---|:---|:---|:---
|预配列主密钥、列加密密钥、加密列加密密钥及其相应的列主密钥。|是|是|否|
|在数据库中创建密钥元数据。|是|是|是|
|创建具有加密列的新表|是|是|是|
|对选定的数据库列中的现有数据进行加密|是|是|否|

> [!NOTE]
> [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] 中引入的[具有安全 Enclave 的 Always Encrypted](always-encrypted-enclaves.md) 支持使用 Transact-SQL 加密现有数据。 这样也无需将数据移动到数据源之外来进行加密操作。

> [!NOTE]
> 请确保在安全环境中，在非托管数据库的计算机上运行密钥预配或数据加密工具。 否则，敏感数据或密钥可能会泄漏给服务器环境，这将减少使用“始终加密”功能的好处。  

有关配置“始终加密”功能的详细信息，请参阅：
- [使用 SSMS 配置“始终加密”功能](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [使用 PowerShell 配置“始终加密”功能](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)

## <a name="getting-started-with-always-encrypted"></a>始终加密入门

使用 [始终加密向导](../../../relational-databases/security/encryption/always-encrypted-wizard.md) 可以快速开始使用“始终加密”功能。 该向导将预配所需的密钥，并配置所选列的加密设置。 如果要为其设置加密的列已包含一些数据，则该向导将对数据进行加密。 以下示例演示了加密列的过程。

> [!NOTE]  
>  有关该向导用法的视频，请参阅 [《Getting Started with Always Encrypted with SSMS》](https://channel9.msdn.com/Shows/Data-Exposed/Getting-Started-with-Always-Encrypted-with-SSMS)（SSMS 中的始终加密入门）。

1.  连接到现有数据库，该数据库中的表具有你想要使用 Management Studio 的 **对象资源管理器** 加密的列，或创建一个新的数据库，创建具有要加密的列的一个或多个表，并连接到该数据库。
2.  右键单击数据库，将光标指向“任务”，然后单击“加密列”打开“Always Encrypted 向导”    。
3.  查看“简介”页，然后单击“下一步”。  
4.  在“列选择”页上展开表，然后选择要加密的列。 
5.  对于选择的要加密的每个列，将“加密类型”设置为“确定性”或“随机”。   
6.  对于选择进行加密的每个列，选择“加密密钥”。  如果以前没有为此数据库创建任何加密密钥，请选择自动生成的新密钥的默认选项，然后单击“下一步”  。
7.  在“主密钥配置”  页上，选择一个位置来存储新密钥，并选择主密钥源，然后单击“下一步”  。
8.  在“验证”页上，选择是要立即运行脚本还是创建 PowerShell 脚本，然后单击“下一步”。  
9.  在“摘要”页上，查看选择的选项，然后单击“完成”   。 完成后关闭向导。

  
## <a name="feature-details"></a>功能详细信息  
  
-   查询可以针对使用确定性加密进行加密的列执行相等性比较，但不能执行其他操作（例如大于/小于、使用 LIKE 运算符或算术运算的模式匹配）。  
  
-   对使用随机加密进行加密的列执行查询无法对上述任何列执行操作。 不支持对使用随机加密进行加密的列编制索引。  
 > [!NOTE] 
 > [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] 中引入的[具有安全 Enclave 的 Always Encrypted](always-encrypted-enclaves.md)，通过启用模式匹配、比较运算符和对使用随机加密的列编制索引来解决上述限制。

-   列加密密钥最多可以包含两个不同的加密值，每个值是使用不同的列主密钥加密的。 这样便于轮换列主密钥。  
  
-   确定性加密要求列有一个 [*binary2* 排序规则](../../../relational-databases/collations/collation-and-unicode-support.md)。  

-   更改已加密对象的定义之后，需执行 [sp_refresh_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md)，以更新该对象的 Always Encrypted 元数据。
  
具有以下特征的列不支持 Always Encrypted。 例如，如果列满足以下任何条件，则 `ENCRYPTED WITH` 子句不能用于 `CREATE TABLE/ALTER TABLE` 中的列：  
  
-   使用以下数据类型之一的列：`xml`、`timestamp`/`rowversion`、`image`、`ntext`、`text`、`sql_variant`、`hierarchyid`、`geography`、`geometry`、别名、用户定义的类型。  
- `FILESTREAM` 列  
- 具有 `IDENTITY` 属性的列。  
- 具有 `ROWGUIDCOL` 属性的列。  
- 采用非 bin2 排序规则的字符串（`varchar`、`char` 等）列。  
- 使用随机加密时用作聚集索引和非聚集索引的键的列（支持确定性加密）。
- 全文检索中包含的列（Always Encrypted 不支持[全文搜索](../../../relational-databases/search/full-text-search.md)）。  
- 计算列。
- 计算列引用的列（当表达式针对 Always Encrypted 执行不受支持的操作时）。  
- 稀疏列集。  
- 使用随机加密时统计信息引用的列（支持确定性加密）。  
- 使用别名类型的列。  
- 分区列。  
- 包含默认约束的列。  
- 使用随机加密时 unique 约束引用的列（支持确定性加密）。  
- 使用随机加密时的主键列（支持确定性加密）。  
- 使用随机加密或确定性加密时引用外键约束中的列（如果被引用和引用列使用不同的键或算法）。  
- Check 约束引用的列。  
- 使用更改数据捕获捕获/跟踪的列。  
- 具有更改跟踪的表中的主键列。  
- 屏蔽的列（使用动态数据掩码）。  
- Stretch Database 表中的列。 （无法为延伸启用其列已使用始终加密加密的表。）  
- 外部 (PolyBase) 表中的列（注意：支持在同一查询中使用外部表和列已加密的表）。  
- 不支持针对加密列的表值参数。  

不能对加密的列使用以下子句：

- `FOR XML`
- `FOR JSON PATH`

以下功能对加密的列不起作用：

- 事务复制或合并复制
- 分布式查询（链接服务器、`OPENROWSET`(T-SQL)、`OPENDATASOURCE`(T-SQL)）

工具要求

- 建议使用 SQL Server Management Studio 18 或更高版本来运行查询，以对从加密列中检索到的结果进行解密，或者插入、更新或筛选加密列。 
- 需要 `sqlcmd` 13.1 或更高版本，可从[下载中心](https://go.microsoft.com/fwlink/?LinkID=825643)进行下载。

  
## <a name="database-permissions"></a>数据库权限  
 “始终加密”功能需要有四个权限：  
  
*  `ALTER ANY COLUMN MASTER KEY` （创建和删除列主密钥所需的权限。）  
  
*  `ALTER ANY COLUMN ENCRYPTION KEY` （创建和删除列加密密钥所需的权限。） 
  
*  `VIEW ANY COLUMN MASTER KEY DEFINITION` （访问和读取列主密钥的元数据以管理密钥或查询加密列所需的权限。）  
  
*  `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` （访问和读取列加密密钥的元数据以管理密钥或查询加密列所需的权限。）  
  
 下表总结了常见的操作所需的权限。  
  
|场景|<code>ALTER ANY COLUMN MASTER KEY</code>|<code>ALTER ANY COLUMN ENCRYPTION KEY</code>|<code>VIEW ANY COLUMN MASTER KEY DEFINITION</code>|<code>VIEW ANY COLUMN ENCRYPTION KEY DEFINITION</code>|  
|--------------|-----------------------------------|---------------------------------------|---------------------------------------------|-------------------------------------------------|  
|密钥管理（创建/更改/检查数据库中的密钥元数据）|X|X|X|X|  
|查询加密列|||X|X|  
  
 **重要说明：**  
  
-   这些权限适用于使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)]、 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] （对话框和向导）或 PowerShell 的操作。  
  
-   选择加密列时需要这两个 VIEW 权限，即使用户没有权限来解密这些列  。  
  
-   在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中，向固定的数据库角色 *默认授予这两个* VIEW `public` 权限。 数据库管理员可以选择撤消（或拒绝） *角色的* VIEW `public` 权限，并将该权限授予特定的角色或用户来实现更严格的控制。  
  
-   在 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]中，默认情况下不会向 `public` 固定数据库角色授予 VIEW 权限  。 这可以使某些现有的旧工具（使用旧版本的 DacFx）能够正常工作。 因此，若要使用加密列（即使不对其进行解密），数据库管理员必须显式授予这两个 *VIEW* 权限。  

  
## <a name="example"></a>示例  
 以下 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 将创建列主密钥元数据、列加密密钥元数据和一个包含加密列的表。 有关如何创建元数据中引用的密钥的信息，请参阅：
- [使用 SSMS 配置“始终加密”功能](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [使用 PowerShell 配置“始终加密”功能](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)

  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = 'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
   );  
---------------------------------------------  
CREATE COLUMN ENCRYPTION KEY MyCEK   
WITH VALUES  
(  
    COLUMN_MASTER_KEY = MyCMK,   
    ALGORITHM = 'RSA_OAEP',   
    ENCRYPTED_VALUE = 0x01700000016C006F00630061006C006D0061006300680069006E0065002F006D0079002F003200660061006600640038003100320031003400340034006500620031006100320065003000360039003300340038006100350064003400300032003300380065006600620063006300610031006300284FC4316518CF3328A6D9304F65DD2CE387B79D95D077B4156E9ED8683FC0E09FA848275C685373228762B02DF2522AFF6D661782607B4A2275F2F922A5324B392C9D498E4ECFC61B79F0553EE8FB2E5A8635C4DBC0224D5A7F1B136C182DCDE32A00451F1A7AC6B4492067FD0FAC7D3D6F4AB7FC0E86614455DBB2AB37013E0A5B8B5089B180CA36D8B06CDB15E95A7D06E25AACB645D42C85B0B7EA2962BD3080B9A7CDB805C6279FE7DD6941E7EA4C2139E0D4101D8D7891076E70D433A214E82D9030CF1F40C503103075DEEB3D64537D15D244F503C2750CF940B71967F51095BFA51A85D2F764C78704CAB6F015EA87753355367C5C9F66E465C0C66BADEDFDF76FB7E5C21A0D89A2FCCA8595471F8918B1387E055FA0B816E74201CD5C50129D29C015895CD073925B6EA87CAF4A4FAF018C06A3856F5DFB724F42807543F777D82B809232B465D983E6F19DFB572BEA7B61C50154605452A891190FB5A0C4E464862CF5EFAD5E7D91F7D65AA1A78F688E69A1EB098AB42E95C674E234173CD7E0925541AD5AE7CED9A3D12FDFE6EB8EA4F8AAD2629D4F5A18BA3DDCC9CF7F352A892D4BEBDC4A1303F9C683DACD51A237E34B045EBE579A381E26B40DCFBF49EFFA6F65D17F37C6DBA54AA99A65D5573D4EB5BA038E024910A4D36B79A1D4E3C70349DADFF08FD8B4DEE77FDB57F01CB276ED5E676F1EC973154F86  
);  
---------------------------------------------  
CREATE TABLE Customers (  
    CustName nvarchar(60)   
        COLLATE  Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,  
        ENCRYPTION_TYPE = RANDOMIZED,  
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'),   
    SSN varchar(11)   
        COLLATE  Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,  
        ENCRYPTION_TYPE = DETERMINISTIC ,  
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'),   
    Age int NULL  
);  
GO  
  
```  
## <a name="see-also"></a>另请参阅  
- [使用 SSMS 配置“始终加密”功能](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)   
- [使用 PowerShell 配置“始终加密”功能](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)   
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md) 
- [使用 Always Encrypted 向导配置列加密](always-encrypted-wizard.md)
- [Always Encrypted 加密](../../../relational-databases/security/encryption/always-encrypted-cryptography.md)   
- [创建列主密钥 (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)   
- [创建列加密密钥 (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)   
- [CREATE TABLE (Transact-SQL)](../../../t-sql/statements/create-table-transact-sql.md)   
- [column_definition (Transact-SQL)](../../../t-sql/statements/alter-table-column-definition-transact-sql.md)   
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)  
- [sys.column_encryption_key_values (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)   
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)   
- [sys.columns (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
- [sp_refresh_parameter_encryption (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md)   
  
  
