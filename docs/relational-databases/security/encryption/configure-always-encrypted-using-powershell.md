---
title: 使用 PowerShell 配置 Always Encrypted | Microsoft Docs
description: 了解如何导入和使用 SqlServer PowerShell 模块，该模块提供用于在 Azure SQL 数据库和 SQL Server 中配置 Always Encrypted 的 cmdlet。
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 12f2bde5-e100-41fa-b474-2d2332fc7650
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c5768357f83252c4d71ac1aeff064364a63367b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345404"
---
# <a name="configure-always-encrypted-using-powershell"></a>使用 PowerShell 配置 Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

SqlServer PowerShell 模块提供用于在 [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] 或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中配置 [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) 的 cmdlet。

## <a name="security-considerations-when-using-powershell-to-configure-always-encrypted"></a>使用 PowerShell 配置 Always Encrypted 时的安全注意事项

由于 Always Encrypted 的主要目的是确保加密敏感数据的安全（即使数据库系统遭到入侵），因此在 SQL Server 计算机上执行处理密钥或敏感数据的 PowerShell 脚本可减少或抵消该功能带来的益处。 有关其他安全相关的建议，请参阅 [Security Considerations for Key Management](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)（密钥管理的安全注意事项）。

你可以在分隔角色或不分隔角色的情况下使用 PowerShell 管理 Always Encrypted 密钥，从而控制可访问密钥存储中实际加密密钥的人员和可访问该数据库的人员。

 有关其他建议，请参阅 [密钥管理安全注意事项](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)。

## <a name="prerequisites"></a>先决条件

在一台安全计算机（并非托管 SQL Server 实例的计算机）上安装 [SqlServer module](/powershell/sqlserver/sqlserver/vlatest/sqlserver) （SqlServer 模块）。 可通过 PowerShell 库直接安装该模块。  请参阅[下载说明](../../../powershell/download-sql-server-ps-module.md)了解详细信息。


## <a name="importing-the-sqlserver-module"></a><a name="importsqlservermodule"></a> 导入 SqlServer 模块 

加载 SqlServer 模块：

1.  使用 **Set-ExecutionPolicy** cmdlet 设置相应的脚本执行策略。
2.  使用 **Import-Module** cmdlet 导入 SqlServer 模块。

此示例将加载 SqlServer 模块。

```
# Import the SQL Server Module.  
Import-Module "SqlServer" 
```

## <a name="connecting-to-a-database"></a><a name="connectingtodatabase"></a> 连接到数据库

一些 Always Encrypted cmdlet 适用于数据库中的数据或元数据，并要求你首先应连接到数据库。 使用 SqlServer 模块配置 Always Encrypted 时，建议使用两种方法来连接到数据库： 
1. 使用 Get-SqlDatabase cmdlet 进行连接。
2. 使用 SQL Server PowerShell 提供程序进行连接。

[!INCLUDE[freshInclude](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

### <a name="using-get-sqldatabase"></a>使用 Get-SqlDatabase
通过 Get-SqlDatabase cmdlet，可以连接到 SQL Server 或 Azure SQL 数据库中的数据库。 它会返回一个数据库对象，随后你可以使用连接数据库的 cmdlet 的 InputObject 参数传递此对象。 

### <a name="using-sql-server-powershell"></a>使用 SQL Server PowerShell

```
# Import the SqlServer module
Import-Module "SqlServer"  

# Connect to your database
# Set the valid server name, database name and authentication keywords in the connection string
$serverName = "<Azure SQL server name>.database.windows.net"
$databaseName = "<database name>"
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Authentication = Active Directory Integrated"
$database = Get-SqlDatabase -ConnectionString $connStr

# List column master keys for the specified database.
Get-SqlColumnMasterKey -InputObject $database
```

或者，你可使用管道传送：


```
$database | Get-SqlColumnMasterKey
```

### <a name="using-sql-server-powershell-provider"></a>使用 SQL Server PowerShell 提供程序
[SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)用类似于文件系统路径的路径公开 SQL Server 对象的层次结构。 通过 SQL Server PowerShell，你可以使用与你通常用于导航文件系统路径的命令相似的 Windows PowerShell 别名来导航路径。 在你导航到目标实例和数据库后，后续 cmdlet 将以该数据库为目标，如下面的示例所示。 

> [!NOTE]
> 连接数据库的此方法仅适用于 SQL Server（Azure SQL 数据库中不支持此方法）。

```
# Import the SqlServer module.
Import-Module "SqlServer"
# Navigate to the database in the remote instance.
cd SQLSERVER:\SQL\servercomputer\DEFAULT\Databases\yourdatabase
# List column master keys in the above database.
Get-SqlColumnMasterKey
```

 
或者，你可使用泛型 **Path** 参数来指定数据库路径，而不是导航到该数据库。


```
# Import the SqlServer module.
Import-Module "SqlServer" 
# List column master keys for the specified database.
Get-SqlColumnMasterKey -Path SQLSERVER:\SQL\servercomputer\DEFAULT\Databases\yourdatabase
```
 
## <a name="always-encrypted-tasks-using-powershell"></a>使用 PowerShell 的 Always Encrypted 任务

- [使用 PowerShell 预配 Always Encrypted 密钥](configure-always-encrypted-keys-using-powershell.md)
- [使用 PowerShell 轮换 Always Encrypted 密钥](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)
- [通过 PowerShell 对使用 Always Encrypted 的列进行加密、重新加密或解密](configure-column-encryption-using-powershell.md)


##  <a name="always-encrypted-cmdlet-reference"></a><a name="aecmdletreference"></a> Always Encrypted Cmdlet 参考

以下 PowerShell cmdlet 可供 Always Encrypted 使用：

|CMDLET |说明
|:---|:---
|**[Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)** |执行 Azure 身份验证，获取身份验证令牌。
|**[Add-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/add-sqlcolumnencryptionkeyvalue)** |为数据库中的现有列加密密钥对象添加新的加密值。
|**[Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)** |完成列主密钥的轮换
|**[Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey)**   |返回数据库中定义的所有列加密密钥对象，或返回一个具有指定名称的列加密密钥对象。
|**[Get-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnmasterkey)**   |返回数据库中定义的列主密钥对象，或返回一个具有指定名称的列主密钥对象。
|**[Invoke-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/invoke-sqlcolumnmasterkeyrotation)** |启动列主密钥的轮换。
|**[New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)** |创建一个 SqlColumnMasterKeySettings 对象，该对象描述存储在 Azure 密钥保管库中的非对称密钥。
|**[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)** |创建一个 SqlColumnMasterKeySettings 对象，该对象描述存储在支持下一代加密技术 (CNG) API 的密钥存储中的非对称密钥。
|**[New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)**   |在数据库中创建列加密密钥对象。
|**[New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)**   |生成列加密密钥的加密值。
|**[New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings)** |创建一个 SqlColumnEncryptionSettings 对象，该对象封装有关单个列的加密的信息（包括 CEK 和加密类型）。
|**[New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)**   |在数据库中创建列主密钥对象。
|**[New-SqlColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkeysettings)**|使用指定的提供程序和密钥路径为列主密钥创建 SqlColumnMasterKeySettings 对象。
|**[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)** |创建一个 SqlColumnMasterKeySettings 对象，该对象描述存储在带有加密服务提供程序 (CSP)（支持加密 API (CAPI)）的密钥存储中的非对称密钥。
|**[Remove-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkey)** |从数据库删除列加密密钥对象。
|**[Remove-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkeyvalue)**   |从数据库的现有列加密对象中删除加密值。
|**[Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)** |从数据库删除列主密钥对象。
|**[Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)** |加密、解密或重新加密数据库中的指定列。



## <a name="see-also"></a>另请参阅

- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted 密钥管理概述](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [使用 SQL Server Management Studio 配置 Always Encrypted](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)