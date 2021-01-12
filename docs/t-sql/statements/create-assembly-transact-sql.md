---
description: CREATE ASSEMBLY (Transact-SQL)
title: CREATE ASSEMBLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ASSEMBLY
- CREATE ASSEMBLY
- CREATE_ASSEMBLY_TSQL
- ASSEMBLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- assemblies [CLR integration], validating
- validating assemblies
- CREATE ASSEMBLY statement
- assemblies [CLR integration], creating
ms.assetid: d8d1d245-c2c3-4325-be52-4fc1122c2079
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 13b639ace60f98ce9ab55cc0280130131f6851ca
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098400"
---
# <a name="create-assembly-transact-sql"></a>CREATE ASSEMBLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  创建包含类元数据和托管代码的托管应用程序模块，将其作为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的对象。 通过引用此模块，可在数据库中创建公共语言运行时 (CLR) 函数、存储过程、触发器、用户定义聚合以及用户定义类型。  
  
> [!WARNING]
>  CLR 在 .NET Framework 中使用代码访问安全性 (CAS)（不可再作为安全边界）。 使用 `PERMISSION_SET = SAFE` 创建的 CLR 程序集可以访问外部系统资源、调用非托管代码以及获取 sysadmin 特权。 从 [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] 开始，引入了名为 `clr strict security` 的 `sp_configure` 选项，以增强 CLR 程序集的安全性。 默认启用 `clr strict security`，并将 `SAFE` 和 `EXTERNAL_ACCESS` 程序集与标记为 `UNSAFE` 的程序集同等对待。 可禁用 `clr strict security` 选项以实现后向兼容性，但不建议这样做。 Microsoft 建议所有程序集都通过证书或非对称密钥进行签名，且该证书或非对称密钥具有已在主数据库中获得 `UNSAFE ASSEMBLY` 权限的相应登录名。 有关详细信息，请参阅 [CLR 严格安全性](../../database-engine/configure-windows/clr-strict-security.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE ASSEMBLY assembly_name  
[ AUTHORIZATION owner_name ]  
FROM { <client_assembly_specifier> | <assembly_bits> [ ,...n ] }  
[ WITH PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE } ]  
[ ; ]  
<client_assembly_specifier> :: =  
        '[\\computer_name\]share_name\[path\]manifest_file_name'  
  | '[local_path\]manifest_file_name'  
  
<assembly_bits> :: =  
{ varbinary_literal | varbinary_expression }  
```  
  
## <a name="arguments"></a>参数  
 assembly_name  
 程序集的名称。 此名称必须在数据库中唯一，并且是有效的[标识符](../../relational-databases/databases/database-identifiers.md)。  
  
 AUTHORIZATION owner_name  
 指定作为程序集所有者的用户或角色的名称。 owner_name 必须是当前用户所属的角色的名称，或当前用户必须具有对 owner_name 的 IMPERSONATE 权限。 如果未指定，则所有权授予当前用户。  
  
 \<client_assembly_specifier>  
指定正在上载的程序集所在的本地路径或网络位置，以及与程序集对应的清单文件名。  \<client_assembly_specifier> 可表示为固定字符串或其值等于固定字符串的、带有变量的表达式。 CREATE ASSEMBLY 不支持加载多模块程序集。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还将在同一位置查找此程序集的所有相关程序集，并使用与根级别程序集相同的所有者将其上载。 如果没有找到这些相关程序集且它们尚未加载到当前数据库中，则 CREATE ASSEMBLY 失败。 如果相关程序集已加载到当前数据库中，则这些程序集的所有者必须与新创建的程序集的所有者相同。

> [!IMPORTANT]
> Azure SQL 数据库不支持通过文件创建程序集。
  
 如果模拟登录用户，则无法指定 \<client_assembly_specifier>。  
  
 \<assembly_bits>  
 组成程序集和依赖程序集的二进制值的列表。 列表中的第一个值将视为根级程序集。 与相关程序集对应的值可以按任意顺序提供。 所有与根程序集的依赖项不相对应的值都将忽略。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
 varbinary_literal  
 是 varbinary 文本。  
  
 *varbinary_expression*  
 varbinary 类型的表达式。  
  
 PERMISSION_SET { **SAFE** | EXTERNAL_ACCESS | UNSAFE }  
> [!IMPORTANT]
>  `PERMISSION_SET` 选项受 `clr strict security` 选项的影响，首次警告中给予了相关说明。 如果启用 `clr strict security`，所有程序集被视为 `UNSAFE`。
 
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 访问程序集时向程序集授予的一组代码访问权限。 如果未指定，则将 SAFE 用作默认值。  
  
 我们推荐使用 SAFE。 SAFE 是最具限制性的权限集。 由具有 SAFE 权限的程序集所执行的代码将无法访问外部系统资源，例如文件、网络、环境变量或注册表。  
  
 EXTERNAL_ACCESS 使程序集可以访问某些外部系统资源，例如文件、网络、环境变量以及注册表。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
 UNSAFE 可使程序集不受限制地访问资源，无论是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例内部还是外部的资源都可以访问。 从 UNSAFE 程序集内运行的代码可调用未托管代码。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
> [!IMPORTANT]  
>  对于执行计算和数据管理任务而无需访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例外部资源的程序集，SAFE 是推荐的权限设置。  
>   
>  对于访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例外部资源的程序集，我们推荐使用 EXTERNAL_ACCESS。 EXTERNAL_ACCESS 程序集包含 SAFE 程序集的可靠性和可伸缩性保护，但从安全角度而言，它与 UNSAFE 程序集类似。 原因是在默认情况下，EXTERNAL_ACCESS 程序集中的代码以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户身份运行并访问此帐户的外部资源，除非此代码显式模拟调用方。 因此，创建 EXTERNAL_ACCESS 程序集的权限应只授予以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户身份运行代码的可信登录。 有关模拟的详细信息，请参阅 [CLR 集成安全性](../../relational-databases/clr-integration/security/clr-integration-security.md)。  
>   
>  指定 UNSAFE 将使程序集中的代码在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程空间中完全自由地执行操作，但这些操作可能危及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的可靠性。 UNSAFE 程序集也可能破坏 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或公共语言运行时的安全系统。 UNSAFE 权限只应授予高度可信的程序集。 只有 sysadmin 固定服务器角色的成员才能创建并更改 UNSAFE 程序集。  
  
 有关程序集的权限集的详细信息，请参阅[设计程序集](../../relational-databases/clr-integration/assemblies-designing.md)。  
  
## <a name="remarks"></a>注解  
 CREATE ASSEMBLY 将上载以前由托管代码编写为 .dll 文件的程序集，以便在 SQL Server 实例中使用。  
 
启用时，`CREATE ASSEMBLY` 和 `ALTER ASSEMBLY` 语句中的 `PERMISSION_SET` 选项在运行时将被忽略，但元数据中将保留 `PERMISSION_SET` 选项。 忽略选项可最大程度减少中断现有代码语句。
 
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不允许使用相同的名称、区域性和公钥来注册程序集的不同版本。  
  
当尝试访问 \<client_assembly_specifier> 中指定的程序集时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 模拟当前 Windows 登录的安全上下文。 如果 \<client_assembly_specifier> 指定了网络位置（UNC 路径），则由于委托限制，当前登录名的模拟将不应用于网络位置。 在这种情况下，将使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户的安全上下文进行访问。 有关详细信息，请参阅[凭据（数据库引擎）](../../relational-databases/security/authentication-access/credentials-database-engine.md)。
  
 除了 assembly_name 指定的根程序集外，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还试图上传正在上传的根程序集所引用的所有程序集。 如果前面的 CREATE ASSEMBLY 语句已将被引用的程序集上载到数据库中，则不再上载此程序集，但它仍可用于根程序集。 如果以前未上载相关程序集，但 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法在源目录中找到它的清单文件，则 CREATE ASSEMBLY 将返回错误。  
  
 如果根程序集引用的所有相关程序集尚未在数据库中并且与根程序集一起隐式加载，则它们与根级别程序集具有相同的权限设置。 如果必须使用不同于根级别程序集的权限设置创建相关程序集，则它们必须在具有相应权限设置的根级别程序集之前显式上载。  
  
## <a name="assembly-validation"></a>程序集验证  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将对 CREATE ASSEMBLY 语句上载的程序集二进制文件执行检查，以确保符合以下要求：  
  
-   程序集二进制文件具有格式正确的有效元数据和代码段，并且代码段包含有效的 Microsoft 中间语言 (MSIL) 指令。  
  
-   所引用的一组系统程序集是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中以下支持的程序集之一：Microsoft.Visualbasic.dll、Mscorlib.dll、System.Data.dll、System.dll、System.Xml.dll、Microsoft.Visualc.dll、Custommarshallers.dll、System.Security.dll、System.Web.Services.dll、System.Data.SqlXml.dll、System.Core.dll 和 System.Xml.Linq.dll。 还可引用其他系统程序集，但这些程序集必须在数据库中显式注册。  
  
-   对于使用 SAFE 或 EXTERNAL ACCESS 权限集创建的程序集：  
  
    -   程序集代码应是类型安全的。 通过对程序集运行公共语言运行时验证工具可建立类型安全。  
  
    -   程序集的类中不应包含任何静态数据成员，除非这些成员标记为只读。  
  
    -   程序集中的类不能包含终结器方法。  
  
    -   程序集的类或方法只能使用允许的代码属性进行注释。 有关详细信息，请参阅 [CLR 例程的自定义属性](../../relational-databases/clr-integration/database-objects/clr-integration-custom-attributes-for-clr-routines.md)。  
  
 除了执行 CREATE ASSEMBL 时进行的上述检查外，在执行程序集中代码时还应进行其他检查：  
  
-   如果程序集的权限集不包括此权限，则调用某些需要特定代码访问权限的 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] API 可能失败。  
  
-   对于 SAFE 和 EXTERNAL_ACCESS 程序集，对使用某些 HostProtectionAttribute 注释的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] API 的任何调用尝试都将失败。  
  
 有关详细信息，请参阅[设计程序集](../../relational-databases/clr-integration/assemblies-designing.md)。  
  
## <a name="permissions"></a>权限  
 需要 CREATE ASSEMBLY 权限。  
  
 如果指定 PERMISSION_SET = EXTERNAL_ACCESS，则需要对服务器的 EXTERNAL ACCESS ASSEMBLY 权限。 如果指定 PERMISSION_SET = UNSAFE，则需要对服务器的 UNSAFE ASSEMBLY 权限。  
  
 如果程序集已经存在于数据库中，则用户必须是将上载的程序集所引用的所有程序集的所有者。 若要使用文件路径上传程序集，则当前用户必须是经过 Windows 身份验证的登录名或 sysadmin 固定服务器角色的成员。 执行 CREATE ASSEMBLY 的用户的 Windows 登录名必须对此语句中加载的共享和文件具有读取权限。  

### <a name="permissions-with-clr-strict-security"></a>CLR 严格安全性的权限    
启用 `CLR strict security` 时，创建 CLR 程序集需要以下权限：

- 用户必须具有 `CREATE ASSEMBLY` 权限  
- 并且，还必须满足以下条件之一：  
  - 使用具有相应登录名（该登录名对应于服务器上的 `UNSAFE ASSEMBLY` 权限）的证书或非对称密钥对程序集进行签名。 建议对程序集进行签名。  
  - 数据库的 `TRUSTWORTHY` 属性设置为 `ON`，且数据库由在服务器上具有 `UNSAFE ASSEMBLY` 权限的登录名所有。 不建议使用此选项。  
  
 有关程序集的权限集的详细信息，请参阅[设计程序集](../../relational-databases/clr-integration/assemblies-designing.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="example-a-creating-an-assembly-from-a-dll"></a>示例 A：从 dll 创建程序集  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 以下示例假定：[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]示例安装在本地计算机的默认位置，且 HelloWorld.csproj 示例应用程序已编写。 有关详细信息，请参阅 [Hello World 示例](/previous-versions/sql/sql-server-2016/ff878250(v=sql.130))。  
  
```sql  
CREATE ASSEMBLY HelloWorld   
FROM '<system_drive>:\Program Files\Microsoft SQL Server\100\Samples\HelloWorld\CS\HelloWorld\bin\debug\HelloWorld.dll'  
WITH PERMISSION_SET = SAFE;  
```  

> [!IMPORTANT]
> Azure SQL 数据库不支持通过文件创建程序集。
  
### <a name="example-b-creating-an-assembly-from-assembly-bits"></a>示例 B：从程序集位创建程序集  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 将示例位（不完整或无效）替换为程序集位。  
  
```sql  
CREATE ASSEMBLY HelloWorld  
    FROM 0x4D5A900000000000  
WITH PERMISSION_SET = SAFE;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER ASSEMBLY (Transact-SQL)](../../t-sql/statements/alter-assembly-transact-sql.md)   
 [DROP ASSEMBLY (Transact-SQL)](../../t-sql/statements/drop-assembly-transact-sql.md)   
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)   
 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md)   
 [CREATE AGGREGATE (Transact-SQL)](../../t-sql/statements/create-aggregate-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [公共语言运行时 (CLR) 集成的使用方案和示例](/previous-versions/sql/sql-server-2016/ms131078(v=sql.130))  
  
