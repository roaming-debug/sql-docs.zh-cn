---
description: ALTER ASSEMBLY (Transact-SQL)
title: ALTER ASSEMBLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_ASSEMBLY_TSQL
- ALTER ASSEMBLY
dev_langs:
- TSQL
helpviewer_keywords:
- assemblies [CLR integration], modifying
- refreshing assemblies
- assemblies [CLR integration], versioning
- assemblies [CLR integration], adding files
- modifying assemblies
- adding files
- ALTER ASSEMBLY statement
ms.assetid: 87bca678-4e79-40e1-bb8b-bd5ed8f34853
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ba3d1139d0ceca041185f7b2724ad240cc8cc346
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "100348569"
---
# <a name="alter-assembly-transact-sql"></a>ALTER ASSEMBLY (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  通过修改程序集的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目录属性更改程序集。 ALTER ASSEMBLY 将它刷新为保存其实现的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 模块的最新副本，并添加或删除与之关联的文件。 可以使用 [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md) 创建程序集。  

> [!WARNING]
>  CLR 在 .NET Framework 中使用代码访问安全性 (CAS)（不可再作为安全边界）。 使用 `PERMISSION_SET = SAFE` 创建的 CLR 程序集可以访问外部系统资源、调用非托管代码以及获取 sysadmin 特权。 从 [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] 开始，引入了名为 `clr strict security` 的 `sp_configure` 选项，以增强 CLR 程序集的安全性。 默认启用 `clr strict security`，并将 `SAFE` 和 `EXTERNAL_ACCESS` 程序集与标记为 `UNSAFE` 的程序集同等对待。 可禁用 `clr strict security` 选项以实现后向兼容性，但不建议这样做。 Microsoft 建议所有程序集都通过证书或非对称密钥进行签名，且该证书或非对称密钥具有已在主数据库中获得 `UNSAFE ASSEMBLY` 权限的相应登录名。 有关详细信息，请参阅 [CLR 严格安全性](../../database-engine/configure-windows/clr-strict-security.md)。  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER ASSEMBLY assembly_name  
    [ FROM <client_assembly_specifier> | <assembly_bits> ]  
    [ WITH <assembly_option> [ ,...n ] ]  
    [ DROP FILE { file_name [ ,...n ] | ALL } ]  
    [ ADD FILE FROM   
    {   
        client_file_specifier [ AS file_name ]   
      | file_bits AS file_name   
    } [,...n ]   
    ] [ ; ]  
<client_assembly_specifier> :: =  
    '\\computer_name\share-name\[path\]manifest_file_name'  
  | '[local_path\]manifest_file_name'  
  
<assembly_bits> :: =  
    { varbinary_literal | varbinary_expression }  
  
<assembly_option> :: =  
    PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE }   
  | VISIBILITY = { ON | OFF }  
  | UNCHECKED DATA  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 assembly_name  
 要修改的程序集的名称。 数据库中必须已存在 assembly_name。  
  
 FROM \<client_assembly_specifier> | \<assembly_bits>  
 将程序集更新到保存其实现的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 模块的最新副本。 仅当没有与指定程序集关联的文件时才能使用此选项。  
  
 \<client_assembly_specifier>：指定要刷新的程序集所在的网络位置或本地位置。 网络位置包括计算机名称、共享名称和该共享中的路径。 manifest_file_name 指定包含程序集清单的文件的名称。  

> [!IMPORTANT]
> Azure SQL 数据库不支持引用文件。
  
 \<assembly_bits> 是该程序集的二进制值。  
  
 必须为需要更新的任何相关程序集发出单独的 ALTER ASSEMBLY 语句。  
  
 PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE }   
> [!IMPORTANT]
>  `PERMISSION_SET` 选项受 `clr strict security` 选项的影响，首次警告中给予了相关说明。 如果启用 `clr strict security`，所有程序集被视为 `UNSAFE`。  
>  指定程序集的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 代码访问权限集属性。 有关此属性的详细信息，请参阅 [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md)。  
> 
> [!NOTE]
>  EXTERNAL_ACCESS 和 UNSAFE 选项在包含的数据库中不可用。  
  
 VISIBILITY = { ON | OFF }  
 指示在创建公共语言运行时 (CLR) 函数、存储过程、触发器、用户定义的类型以及针对它的用户定义的聚合函数时，该程序集是否可见。 如果设置为 OFF，则程序集只能由其他程序集调用。 如果存在已针对该程序集创建的现有 CLR 数据库对象，则无法更改程序集的可见性。 默认情况下，由 assembly_name 引用的任何程序集都上传为不可见。  
  
 UNCHECKED DATA  
 默认情况下，如果 ALTER ASSEMBLY 必须验证各个表行的一致性，则它将失败。 该选项使您可以通过使用 DBCC CHECKTABLE 将检查推迟到以后的某个时间进行。 如果指定该选项，则即使数据库中存在包含下列项的表，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 仍将执行 ALTER ASSEMBLY 语句：  
  
-   持久化计算列，这些列通过 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数或方法直接或间接引用程序集中的方法。  
  
-   直接或间接引用程序集中方法的 CHECK 约束。  
  
-   属于 CLR 用户定义类型且依赖于程序集的列，并且该类型实现的是 UserDefined（非 Native）序列化格式 。  
  
-   CLR 用户定义类型的列，这些列引用通过使用 WITH SCHEMABINDING 创建的视图。  
  
 如果存在 CHECK 约束，则它们将被禁用并标记为不可信。 任何包含依赖于程序集的列的表都被标记为包含未检查数据，直到对这些表进行了显式检查为止。  
  
 只有 db_owner 和 db_ddlowner 固定数据库角色的成员才能指定该选项 。  
  
 需要 ALTER ANY SCHEMA 权限来指定此选项。  
  
 有关详细信息，请参阅[实现程序集](../../relational-databases/clr-integration/assemblies-implementing.md)。  
  
 [ DROP FILE { file_name[ ,...n] | ALL } ]  
 从数据库中删除与程序集关联的文件名，或与该程序集关联的所有文件。 如果与下面的 ADD FILE 一起使用，则 DROP FILE 首先执行。 这样可以用相同的文件名替换文件。  
  
> [!NOTE]  
>  这种方法不适用于包含的数据库或 Azure SQL 数据库。  
  
 [ ADD FILE FROM { client_file_specifier [ AS file_name] | file_bitsAS file_name}     
 将与程序集关联的文件（如源代码、调试文件或其他相关信息）上传到服务器中并使其在 sys.assembly_files 目录视图中可见。 client_file_specifier 指定上传文件的源位置。 可以改用 file_bits 来指定构成该文件的二进制值的列表。 file_name 指定将文件存储到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中时所采用的名称。 如果指定 file_bits，则必须指定 file_name，如果指定 client_file_specifier，则可以选择指定 file_name  。 如果未指定 file_name，则 client_file_specifier 的 file_name 部分可用作 file_name  。  
  
> [!NOTE]  
>  这种方法不适用于包含的数据库或 Azure SQL 数据库。  
  
## <a name="remarks"></a>备注  
 ALTER ASSEMBLY 不中断当前正在运行的会话，这些会话正在运行所修改的程序集中的代码。 当前会话通过使用程序集的未更改位完成执行。  
  
 如果指定 FROM 子句，则 ALTER ASSEMBLY 相对于所提供的模块的最新副本更新程序集。 由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中可能存在已针对程序集定义的 CLR 函数、存储过程、触发器、数据类型和用户定义的聚合函数，因此 ALTER ASSEMBLY 语句将它们重新绑定到该程序集的最新实现。 若要完成实现此重新绑定，映射到 CLR 函数、存储过程和触发器的方法必须仍存在于具有相同签名的已修改程序集中。 实现 CLR 用户定义的类型和用户定义的聚合函数的类必须仍满足作为用户定义的类型或聚合的要求。  
  
> [!CAUTION]  
>  未指定 WITH UNCHECKED DATA 时，如果新的程序集版本对表、索引或其他持久性站点中的现有数据造成影响，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将尝试阻止 ALTER ASSEMBLY 执行。 但是，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不保证在更新 CLR 程序集时，计算列、索引、索引视图或表达式与基本例程和类型一致。 执行 ALTER ASSEMBLY 时要小心，确保表达式的结果与程序集中基于该表达式的值之间不存在不匹配。  
  
 ALTER ASSEMBLY 可更改程序集版本。 程序集的区域性和公钥标记保持不变。  
  
 ALTER ASSEMBLY 语句无法用于更改以下各项：  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中引用此程序集的 CLR 函数、聚合函数、存储过程和触发器的签名。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 无法将 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库对象与程序集的新版本重新绑定，则 ALTER ASSEMBLY 将失败。  
  
-   使用其他程序集调用的该程序集中的方法签名。  
  
-   与此程序集依赖的程序集列表，在此程序集的 DependentList 属性中引用。  
  
-   方法的可索引性，除非没有直接或间接与该方法相关的索引或持久化计算列。  
  
-   CLR 表值函数的 FillRow 方法名属性。  
  
-   用户定义聚合的 Accumulate 和 Terminate 方法签名 。  
  
-   系统程序集。  
  
-   程序集所有权。 改为使用 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)。  
  
 此外，对于实现用户定义类型的程序集，ALTER ASSEMBLY 只能用于进行下列更改：  
  
-   在不更改签名或属性的情况下，修改用户定义类型类的公共方法。  
  
-   添加新的公共方法。  
  
-   以任何方式修改私有方法。  
  
 不能使用 ALTER ASSEMBLY 更改本机序列化用户定义类型（包含数据成员或基类）内包含的字段。 不支持所有其他更改。  
  
 如果未指定 ADD FILE FROM，则 ALTER ASSEMBLY 将删除任何与程序集关联的文件。  
  
 如果执行了没有 UNCHECKED 数据子句的 ALTER ASSEMBLY，则执行检查以验证新数据集版本是否影响表中的现有数据。 根据需要检查的数据量，这可能影响性能。  
  
## <a name="permissions"></a>权限  
 需要对程序集具有 ALTER 权限。 其他要求如下：  
  
-   若要更改其现有权限集为 EXTERNAL_ACCESS 的程序集，则需要对服务器的 EXTERNAL ACCESS ASSEMBLY 权限。  
  
-   若要更改其现有权限集为 UNSAFE 的程序集，则需要对服务器的 UNSAFE ASSEMBLY 权限。  
  
-   若要将程序集的权限集更改为 EXTERNAL_ACCESS，则需要对服务器的 EXTERNAL ACCESS ASSEMBLY 权限。  
  
-   若要将程序集的权限集更改为 UNSAFE，则需要对服务器的 UNSAFE ASSEMBLY 权限。  
  
-   指定 WITH UNCHECKED DATA 需要 ALTER ANY SCHEMA 权限。  


### <a name="permissions-with-clr-strict-security"></a>CLR 严格安全性的权限    
启用 `CLR strict security` 时，更改 CLR 程序集需要以下权限：

- 用户必须具有 `ALTER ASSEMBLY` 权限  
- 并且，还必须满足以下条件之一：  
  - 使用具有相应登录名（该登录名对应于服务器上的 `UNSAFE ASSEMBLY` 权限）的证书或非对称密钥对程序集进行签名。 建议对程序集进行签名。  
  - 数据库的 `TRUSTWORTHY` 属性设置为 `ON`，且数据库由在服务器上具有 `UNSAFE ASSEMBLY` 权限的登录名所有。 不建议使用此选项。  
  
  
 有关程序集的权限集的详细信息，请参阅[设计程序集](../../relational-databases/clr-integration/assemblies-designing.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-refreshing-an-assembly"></a>A. 刷新程序集  
 以下示例将程序集 `ComplexNumber` 更新到保存其实现的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 模块的最新副本。  
  
> [!NOTE]  
>  可以通过运行 UserDefinedDataType 示例脚本创建程序集 `ComplexNumber`。 有关详细信息，请参阅[用户定义类型](/previous-versions/sql/sql-server-2016/ms131078(v=sql.130))。  
  
 ```sql
 ALTER ASSEMBLY ComplexNumber 
 FROM 'C:\Program Files\Microsoft SQL Server\130\Tools\Samples\1033\Engine\Programmability\CLR\UserDefinedDataType\CS\ComplexNumber\obj\Debug\ComplexNumber.dll' 
  ```

> [!IMPORTANT]
> Azure SQL 数据库不支持引用文件。

### <a name="b-adding-a-file-to-associate-with-an-assembly"></a>B. 添加一个文件以与程序集关联  
 以下示例上载源代码文件 `Class1.cs`，使之与程序集 `MyClass` 关联。 该示例假设已在数据库中创建了程序集 `MyClass`。  
  
```sql  
ALTER ASSEMBLY MyClass   
ADD FILE FROM 'C:\MyClassProject\Class1.cs';  
```  

> [!IMPORTANT]
> Azure SQL 数据库不支持引用文件。

### <a name="c-changing-the-permissions-of-an-assembly"></a>C. 更改程序集的权限  
 以下示例将程序集 `ComplexNumber` 的权限集由 SAFE 更改为 `EXTERNAL ACCESS`。  
  
```sql  
ALTER ASSEMBLY ComplexNumber WITH PERMISSION_SET = EXTERNAL_ACCESS;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md)   
 [DROP ASSEMBLY (Transact-SQL)](../../t-sql/statements/drop-assembly-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
