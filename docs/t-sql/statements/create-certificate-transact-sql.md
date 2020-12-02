---
description: CREATE CERTIFICATE (Transact-SQL)
title: CREATE CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/06/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CERTIFICATE
- CREATE_CERTIFICATE_TSQL
- sql13.swb.createcertificate.f1
- CERTIFICATE_TSQL
- CREATE CERTIFICATE
- sql13.swb.certificateproperties.f1
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], certificates
- certificates [SQL Server], adding
- certificates [SQL Server]
- adding certificates
- cryptography [SQL Server], certificates
- CREATE CERTIFICATE statement
ms.assetid: a4274b2b-4cb0-446a-a956-1c8e6587515d
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d4799b962fd1c8b6084443f5d83fa171fe4b0a1
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124145"
---
# <a name="create-certificate-transact-sql"></a>CREATE CERTIFICATE (Transact-SQL)
[!INCLUDE [sql-asdb-asa-pdw](../../includes/applies-to-version/sql-asdb-asa-pdw.md)]

  为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的数据库添加证书。  

 此功能与使用数据层应用程序框架 (DACFx) 的数据库导出不兼容。 必须在导出之前删除所有证书。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE CERTIFICATE certificate_name [ AUTHORIZATION user_name ]   
    { FROM <existing_keys> | <generate_new_keys> }  
    [ ACTIVE FOR BEGIN_DIALOG = { ON | OFF } ]  
  
<existing_keys> ::=   
    ASSEMBLY assembly_name  
    | {   
        [ EXECUTABLE ] FILE = 'path_to_file'  
        [ WITH PRIVATE KEY ( <private_key_options> ) ]   
      }  
    | {   
        BINARY = asn_encoded_certificate  
        [ WITH PRIVATE KEY ( <private_key_options> ) ]  
      }  
<generate_new_keys> ::=   
    [ ENCRYPTION BY PASSWORD = 'password' ]   
    WITH SUBJECT = 'certificate_subject_name'   
    [ , <date_options> [ ,...n ] ]   
  
<private_key_options> ::=  
      {   
        FILE = 'path_to_private_key'  
         [ , DECRYPTION BY PASSWORD = 'password' ]  
         [ , ENCRYPTION BY PASSWORD = 'password' ]    
      }  
    |  
      {   
        BINARY = private_key_bits  
         [ , DECRYPTION BY PASSWORD = 'password' ]  
         [ , ENCRYPTION BY PASSWORD = 'password' ]    
      }  
  
<date_options> ::=  
    START_DATE = 'datetime' | EXPIRY_DATE = 'datetime'  
```  
  
> [!Note]
> [!INCLUDE [Synapse preview note](../../includes/synapse-preview-note.md)]
   
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE CERTIFICATE certificate_name   
    { <generate_new_keys> | FROM <existing_keys> }  
    [ ; ]  
  
<generate_new_keys> ::=   
    WITH SUBJECT = 'certificate_subject_name'   
    [ , <date_options> [ ,...n ] ]   
  
<existing_keys> ::=   
    {   
      FILE ='path_to_file'  
      WITH PRIVATE KEY   
         (   
           FILE = 'path_to_private_key'  
           , DECRYPTION BY PASSWORD ='password'   
         )  
    }  
  
<date_options> ::=  
    START_DATE ='datetime' | EXPIRY_DATE ='datetime'  
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 certificate_name  
 数据库中证书的名称。  
  
 AUTHORIZATION user_name  
 拥有该证书的用户的名称。  
  
 ASSEMBLY assembly_name  
 指定已经加载到数据库中的已签名的程序集。  
  
 [ EXECUTABLE ] FILE = 'path_to_file'  
 指定包含证书的 DER 编码文件的完整路径（包括文件名）。 如果使用 EXECUTABLE 选项，则文件为已使用证书签名的 DLL。 path_to_file 可以是本地路径，也可以是网络位置的 UNC 路径。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户的安全上下文中访问该文件。 该帐户必须具有所需的文件系统权限。  

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 不支持通过文件或使用私钥文件创建证书。
  
 BINARY = asn_encoded_certificate  
 指定为二进制常量的 ASN 编码证书字节数。  
 **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 WITH PRIVATE KEY  
 指定将证书的私钥加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中。 从程序集创建证书时，此子句无效。 若要加载从程序集创建的证书的私钥，请使用 [ALTER CERTIFICATE](../../t-sql/statements/alter-certificate-transact-sql.md)。  
  
 FILE ='path_to_private_key'  
 指定私钥的完整路径（包括文件名）。 path_to_private_key 可以是本地路径，也可以是网络位置的 UNC 路径。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户的安全上下文中访问该文件。 该帐户必须具有所需的文件系统权限。  
  
> [!IMPORTANT]  
> 此选项在包含的数据库或 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中不可用。  
  
 BINARY = private_key_bits  
 **适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。  
  
 指定为二进制常量的专有键位。 这些位可采用加密形式。 如果加密，则用户必须提供解密密码。 不会对此密码执行密码策略检查。 私钥位应该采用 PVK 文件格式。  
  
 DECRYPTION BY PASSWORD = 'key_password'  
 指定对从文件中检索的私钥进行解密所需的密码。 如果私钥受空密码的保护，则该子句为可选项。 建议不要将私钥保存到无密码保护的文件中。 如果需要密码，但是未指定密码，则该语句将失败。  
  
 ENCRYPTION BY PASSWORD = 'password'  
 指定用于加密私钥的密码。 只有在需要使用密码对证书进行加密时，才使用该选项。 如果省略该子句，则使用数据库主密钥对私钥进行加密。 password 必须符合运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计算机的 Windows 密码策略要求。 有关详细信息，请参阅 [Password Policy](../../relational-databases/security/password-policy.md)。  
  
 SUBJECT = 'certificate_subject_name'  
 根据 X.509 标准中的定义，术语 subject 是指证书的元数据中的字段。 主题的长度应不超过 64 个字符，并且在 Linux 上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中强制执行此限制。 对于 Windows 上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，主题的长度最多是 128 个字符。 将主题存储到目录中时，如果主题的长度超过 128 个字节，则主题会被截断，但是包含证书的二进制大型对象 (BLOB) 将保留完整的主题名称。  
  
 START_DATE = 'datetime'  
 证书生效的日期。 如果未指定，则将 START_DATE 设置为当前日期。 START_DATE 采用 UTC 时间，并且可以通过可转换为日期和时间的任何格式指定。  
  
 EXPIRY_DATE = 'datetime'  
 证书过期的日期。 如果未指定，则将 EXPIRY_DATE 设置为 START_DATE 一年之后的日期。 EXPIRY_DATE 采用 UTC 时间，并且可以通过可转换为日期和时间的任何格式指定。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Broker 会检查过期日期。 使用证书进行加密的备份还将检查到期日期，且不会允许使用过期证书创建新备份，但会允许使用过期证书进行还原。 但是，在将证书用于数据库加密或 Always Encrypted 时，不会强制应用到期日期。  
  
 ACTIVE FOR BEGIN_DIALOG = { ON | OFF }  
 使证书可用于 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 对话会话的发起方。 默认值为 ON。  
  
## <a name="remarks"></a>备注  
 证书是一个数据库级的安全对象，它遵循 X.509 标准并支持 X.509 V1 字段。 `CREATE CERTIFICATE`可以从文件、二进制常量或程序集中加载证书。 该语句也可生成密钥对并创建自我签名的证书。  
  
 私钥必须 \<= 2500 个字节，并且为加密格式。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成的私钥长度在 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及以前版本中是 1024 位，从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始是 2048 位。 从外部源导入的私钥的最小长度为 384 位，最大长度为 4,096 位。 导入的私钥的长度必须是 64 位的整数倍。 用于 TDE 的证书限于专用密钥大小 3456 比特。  
  
 存储证书的整个序列号，但只有前 16 个字节出现在 sys.certificates 目录视图中。  
  
 存储证书的整个证书颁发者字段，但只有前 884 个字节出现在 sys.certificates 目录视图中。  
  
 私钥必须与 certificate_name 指定的公钥相对应。  
  
 当您通过容器创建证书时，可选择是否加载私钥。 但是当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成自我签名的证书时，始终会创建私钥。 默认情况下，私钥使用数据库主密钥进行加密。 如果数据库主密钥不存在并且未指定密码，则该语句将失败。  
  
 如果用数据库主密钥对私钥进行加密，则不需要 `ENCRYPTION BY PASSWORD` 选项。 只有在使用密码对私钥进行加密时，才使用该选项。 如果未指定密码，则使用数据库主密钥对证书的私钥进行加密。 如果数据库主密钥无法打开，则省略该子句会导致错误。  
  
 如果使用数据库主密钥对私钥进行加密，则不一定必须指定解密密码。  
  
> [!NOTE]  
> 内置的加密和签名功能不会检查证书的过期日期。 使用这些功能的用户必须决定何时检查证书的过期日期。  
  
 可以使用 [CERTENCODED (Transact-SQL)](../../t-sql/functions/certencoded-transact-sql.md) 和 [CERTPRIVATEKEY (Transact-SQL)](../../t-sql/functions/certprivatekey-transact-sql.md) 函数创建证书的二进制说明。 有关使用 CERTPRIVATEKEY 和 CERTENCODED 将证书复制到其他数据库中的示例，请参阅文章 [CERTENCODED (Transact-SQL)](../../t-sql/functions/certencoded-transact-sql.md) 中的示例 B。  

已在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 中弃用 MD2、MD4、MD5、SHA 和 SHA1 算法。 在 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 之前，需使用 SHA1 创建自签名证书。 从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始，可使用 SHA2_256 创建自签名证书。

## <a name="permissions"></a>权限  
 需要对数据库拥有 `CREATE CERTIFICATE` 权限。 只有 Windows 登录名、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和应用程序角色才能拥有证书。 其他组和角色不能拥有证书。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-self-signed-certificate"></a>A. 创建自我签名的证书  
 下面的示例创建名为 `Shipping04` 的证书。 该证书的私钥是使用一个密码来保护的。  
  
```sql  
CREATE CERTIFICATE Shipping04   
   ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
   WITH SUBJECT = 'Sammamish Shipping Records',   
   EXPIRY_DATE = '20201031';  
GO  
```  
  
### <a name="b-creating-a-certificate-from-a-file"></a>B. 通过文件创建证书  
 以下示例在数据库中创建证书，并从文件加载密钥对。  
  
```sql  
CREATE CERTIFICATE Shipping11   
    FROM FILE = 'c:\Shipping\Certs\Shipping11.cer'   
    WITH PRIVATE KEY (FILE = 'c:\Shipping\Certs\Shipping11.pvk',   
    DECRYPTION BY PASSWORD = 'sldkflk34et6gs%53#v00');  
GO   
```  

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 不支持通过文件创建证书。
   
### <a name="c-creating-a-certificate-from-a-signed-executable-file"></a>C. 通过已签名的可执行文件创建证书  
  
```sql  
CREATE CERTIFICATE Shipping19   
    FROM EXECUTABLE FILE = 'c:\Shipping\Certs\Shipping19.dll';  
GO  
```  
  
 或者，您还可以先通过 `dll` 文件创建程序集，然后通过程序集创建证书。  
  
```sql  
CREATE ASSEMBLY Shipping19   
    FROM ' c:\Shipping\Certs\Shipping19.dll'   
    WITH PERMISSION_SET = SAFE;  
GO  
CREATE CERTIFICATE Shipping19 FROM ASSEMBLY Shipping19;  
GO  
```  

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 不支持通过文件创建证书。

> [!IMPORTANT]
> 从 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 开始，[CLR 严格安全性](../../database-engine/configure-windows/clr-strict-security.md)服务器配置选项可防止在未先设置程序集安全性的情况下加载程序集。 加载证书，从证书创建登录名，向该登录名授予 `UNSAFE ASSEMBLY`，然后加载程序集。

### <a name="d-creating-a-self-signed-certificate"></a>D. 创建自我签名的证书  
 下面的示例在不指定加密密码的情况下创建名为 `Shipping04` 的证书。 此示例可与 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 一起使用。
  
```sql  
CREATE CERTIFICATE Shipping04   
   WITH SUBJECT = 'Sammamish Shipping Records';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER CERTIFICATE (Transact-SQL)](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE (Transact-SQL)](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [CERTENCODED (Transact-SQL)](../../t-sql/functions/certencoded-transact-sql.md)   
 [CERTPRIVATEKEY (Transact-SQL)](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID (Transact-SQL)](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY (Transact-SQL)](../../t-sql/functions/certproperty-transact-sql.md)  
  
  


