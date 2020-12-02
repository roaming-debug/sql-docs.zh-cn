---
description: 通过 PowerShell 配置使用 Always Encrypted 的列加密
title: 通过 PowerShell 配置使用 Always Encrypted 的列加密 | Microsoft Docs
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 074c012b-cf14-4230-bf0d-55e23d24f9c8
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7014f349998781dcd890e84a77deb4732fe028c9
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127723"
---
# <a name="configure-column-encryption-using-always-encrypted-with-powershell"></a>通过 PowerShell 配置使用 Always Encrypted 的列加密
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

本文提供使用 [Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption) cmdlet（在 *SqlServer* PowerShell 模块中）来设置数据库列的目标 Always Encrypted 配置的步骤。 **Set-SqlColumnEncryption** cmdlet 修改目标数据库的架构和存储在选定列中的数据。 可对存储在列中的数据进行加密、重新加密或解密，具体取决于列和当前加密配置的指定目标加密设置。

::: moniker range=">=sql-server-ver15||=sqlallproducts-allversions"

> [!NOTE]
> 如果使用的是 [!INCLUDE [sssqlv15-md](../../../includes/sssqlv15-md.md)]，并且 SQL Server 实例配置了安全 Enclave，则无需将数据移出数据库，即可就地运行加密操作。 请参阅[使用具有安全 enclave 的 Always Encrypted 就地配置列加密](always-encrypted-enclaves-configure-encryption.md)。 请注意，PowerShell 不支持就地加密。

::: moniker-end
若要深入了解 SqlServer PowerShell 模块中的 Always Encrypted 支持，请参阅[使用 PowerShell 配置 Always Encrypted](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)。

## <a name="prerequisites"></a>先决条件

若要设置目标加密配置，需要确保：
- 数据库中配置了一个列加密密钥（如果要加密或重新加密列）。 有关详细信息，请参阅[使用 PowerShell 配置 Always Encrypted 密钥](../../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md)。
- 可从运行 PowerShell cmdlet 的计算机上访问要加密、重新加密或解密的每一列的列主密钥。 

## <a name="performance-and-availability-considerations"></a>性能和可用性注意事项

为了为数据库应用指定的目标加密设置， **Set-SqlColumnEncryption** cmdlet 将以透明方式从包含目标表的列中下载所有数据，将数据上传回一组临时表（具有目标加密设置），最后将原始表替换为新版本的表。 用于 SQL Server 的基础 .NET Framework 数据提供程序将加密或/和解密下载或/和上传的数据，具体取决于目标列的当前加密配置以及目标列的指定目标加密设置。 移动数据的操作可能需要较长时间，具体取决于受影响的表中数据的大小和网络带宽。

**Set-SqlColumnEncryption** cmdlet 支持两种设置目标加密配置的方法：联机和脱机。

使用脱机方法时，目标表（以及任何与目标表相关的表，例如，目标表与其具有外键关系的任何表）不可用于在操作期间写入事务。 使用脱机方法时，外键约束（**CHECK** 或 **NOCHECK**）的语义会始终保留。

使用联机方法时（需要 21.x 版或更高版本的 SqlServer PowerShell 模块），对数据进行复制和加密、解密或重新加密的操作是以增量方式执行的。 应用程序可以在数据移动操作中将数据写入到目标表以及从中读取数据，除了最后一次迭代，其持续时间受到 MaxDownTimeInSeconds 参数（可自行定义）的限制。 为了检测和处理在复制数据时应用程序可能会做的更改，cmdlet 会在目标数据库中启用[更改跟踪](../../track-changes/enable-and-disable-change-tracking-sql-server.md)。 正因如此，与脱机方法相比，联机方法有可能在服务器端上消耗的资源会更多。 使用联机方法的操作耗时也可能会更长，尤其是对数据库运行大量写入工作负荷时。 联机方法可用于一次加密一个表，该表必须具有一个主键。 默认情况下，外键约束在使用 **NOCHECK** 选项的情况下重新创建以最大限度地降低对应用程序的影响。 你可以通过指定 **KeepCheckForeignKeyConstraints** 选项强制保留外键约束的语义。 

下面是在脱机和联机方法之间进行选择的指导原则：

使用脱机方法：
- 最小化操作的持续时间。 
- 在多个表中同时加密/解密/重新加密列。
- 如果目标表没有主键。

使用联机方法：
- 最小化数据库对应用程序造成的停机时间/不可用性。

## <a name="security-considerations"></a>安全注意事项

用于配置数据库列加密的 **Set-SqlColumnEncryption** cmdlet 可处理 Always Encrypted 密钥和存储在数据库列中的数据。 因此，请务必在一台安全的计算机上运行此 cmdlet。 如果你的数据库在 SQL Server 中，请不要在托管 SQL Server 实例的计算机上执行 cmdlet，而是在另一台计算机上执行。 由于 Always Encrypted 的主要目的是确保加密的敏感数据的安全（即使数据库系统遭到入侵），因此执行用于处理 SQL Server 计算机上密钥和/或敏感数据的 PowerShell 脚本可减少或抵消该功能带来的益处。

任务  |项目  |访问纯文本密钥/密钥存储  |访问数据库   
---|---|---|---
步骤 1。 启动 PowerShell 环境并导入 SqlServer 模块。 | [导入 SqlServer 模块](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | 否 | 否
步骤 2. 连接到服务器和数据库 | [连接到数据库](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | 否 | 是
步骤 3. 如果列主密钥（保护要轮换的列加密密钥）存储在 Azure 密钥保管库中，请对 Azure 进行身份验证 | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | 是 | 否
步骤 4. 构造一组 SqlColumnEncryptionSettings 对象，每个对象对应一个要加密、重新加密或解密的数据库列。 SqlColumnMasterKeySettings 是存在于内存中的对象（在 PowerShell 中）。 它用于指定列的目标加密方案。 | [New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings) | 否 | 否
步骤 5。 设置所需加密配置，该配置在之前的步骤中创建的一组 SqlColumnMasterKeySettings 对象中指定。 根据指定的目标设置和列的当前加密配置，将加密、重新加密或解密列。| [Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)<br><br>**注意：** 此步骤可能需要较长时间。 应用程序无法通过整个操作或部分操作访问表，具体视你选择的方法（联机与脱机）而定。 | 是 | 是

## <a name="encrypt-columns-using-offline-approach---example"></a>使用脱机方法加密列 - 示例

下面的示例演示如何为多个列设置目标加密配置。 任何尚未加密的列将会被加密。 如果已使用不同密钥和/或不同加密类型加密了任何列，则将先进行解密，然后按照指定目标密钥/类型进行加密。


```PowerShell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Encrypt the selected columns (or re-encrypt, if they are already encrypted using keys/encrypt types, different than the specified keys/types.
$ces = @()
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.SSN" -EncryptionType "Deterministic" -EncryptionKey "CEK1"
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.BirthDate" -EncryptionType "Randomized" -EncryptionKey "CEK1"
Set-SqlColumnEncryption -InputObject $database -ColumnEncryptionSettings $ces -LogFileDirectory .
```
 
## <a name="encrypt-columns-using-online-approach---example"></a>使用联机方法加密列 - 示例

下面的示例演示了如何使用联机方法为多个列设置目标加密配置。 任何尚未加密的列将会被加密。 如果已使用不同密钥和/或不同加密类型加密了任何列，则将先进行解密，然后按照指定目标密钥/类型进行加密。

```PowerShell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Encrypt the selected columns (or re-encrypt, if they are already encrypted using keys/encrypt types, different than the specified keys/types.
$ces = @()
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.SSN" -EncryptionType "Deterministic" -EncryptionKey "CEK1"
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.BirthDate" -EncryptionType "Randomized" -EncryptionKey "CEK1"
Set-SqlColumnEncryption -InputObject $database -ColumnEncryptionSettings $ces -UseOnlineApproach -MaxDowntimeInSeconds 180 -LogFileDirectory .
```
## <a name="decrypt-columns---example"></a>解密列 - 示例

下面的示例演示如何解密数据库中当前已加密的所有列。


```PowerShell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Find all encrypted columns, and create a SqlColumnEncryptionSetting object for each column.
$ces = @()
$tables = $database.Tables
for($i=0; $i -lt $tables.Count; $i++){
    $columns = $tables[$i].Columns
    for($j=0; $j -lt $columns.Count; $j++) {
        if($columns[$j].isEncrypted) {
            $threeColPartName = $tables[$i].Schema + "." + $tables[$i].Name + "." + $columns[$j].Name 
            $ces += New-SqlColumnEncryptionSettings -ColumnName $threeColPartName -EncryptionType "Plaintext" 
        }
    }
}

# Decrypt all columns.
Set-SqlColumnEncryption -ColumnEncryptionSettings $ces -InputObject $database -LogFileDirectory .
```
 
## <a name="next-steps"></a>后续步骤
- [使用 Always Encrypted 开发应用程序](always-encrypted-client-development.md)

## <a name="see-also"></a>另请参阅  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Always Encrypted 密钥管理概述](overview-of-key-management-for-always-encrypted.md) 
 - [使用 PowerShell 配置“始终加密”功能](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)
 - [使用 Always Encrypted 向导配置列加密](always-encrypted-wizard.md)
 - [使用 Always Encrypted 和 DAC 包配置列加密](configure-always-encrypted-using-dacpac.md)