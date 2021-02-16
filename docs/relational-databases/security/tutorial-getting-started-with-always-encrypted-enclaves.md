---
title: Tutorial:在 SQL Server 中开始使用具有安全 enclave 的 Always Encrypted
description: 本教程介绍如何使用基于虚拟化的安全性 (VBS) enclave 和用于证明的主机监护服务 (HGS) 为 SQL Server 中具有安全 enclave 的 Always Encrypted 创建基本环境。 你还将了解如何使用 SQL Server Management Studio (SSMS) 就地加密数据并就加密列发出各种机密查询。
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: a33db9a451e8ccf01ecd2f50b832afe4565ebc8c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077815"
---
# <a name="tutorial-getting-started-with-always-encrypted-with-secure-enclaves-in-sql-server"></a>Tutorial:在 SQL Server 中开始使用具有安全 enclave 的 Always Encrypted
[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

本教程介绍如何在 [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] 中开始使用[具有安全 enclave 的 Always Encrypted](encryption/always-encrypted-enclaves.md)。 它将介绍：

> [!div class="checklist"]
> - 如何创建基本环境，以便测试和评估具有安全 enclave 的 Always Encrypted。
> - 如何使用 SQL Server Management Studio (SSMS) 就地加密数据并就加密列发出各种机密查询。

## <a name="prerequisites"></a>先决条件

若要开始使用具有安全 enclave 的 Always Encrypted，需要至少两台计算机（可以是虚拟机）：

- [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] 计算机，用于托管 [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] 和 SSMS。
- HGS 计算机，用于运行 enclave 证明所需的主机保护者服务。

### <a name="sql-server-computer-requirements"></a>SQL Server 计算机要求

- [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] 或更高版本。
- Windows 10 企业版 1809 或更高版本；或是 Windows Server 2019 Datacenter 版本。 其他版本的 Windows 10 和 Windows Server 不支持通过 HGS 进行证明。
- 针对虚拟化技术的 CPU 支持：
  - 具有扩展页表的 Intel VT-x。
  - 具有快速虚拟化索引的 AMD-V。
  - 如果在 VM 中运行 [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)]，则虚拟机监控程序和物理 CPU 必须提供嵌套虚拟化功能。 
    - 在 Hyper-V 2016 或更高版本中，[在 VM 处理器上启用嵌套虚拟化扩展](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization#configure-nested-virtualization)。
    - 在 Azure 中，选择支持嵌套虚拟化的 VM 大小。 这包括所有 v3 系列 VM，例如 Dv3 和 Ev3。 请参阅[创建可嵌套的 Azure VM](/azure/virtual-machines/windows/nested-virtualization#create-a-nesting-capable-azure-vm)。
    - 在 VMware vSphere 6.7 或更高版本中，为 VM 启用基于虚拟化的安全支持，如 [VMware 文档](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-C2E78F3E-9DE2-44DB-9B0A-11440800AADD.html)中所述。
    - 其他虚拟机监控程序和公有云可能支持嵌套虚拟化功能，这些功能也实现具有 VBS Enclave 的 Always Encrypted。 有关兼容性和配置说明，请查看虚拟化解决方案的文档。
- [SQL Server Management Studio (SSMS) 18.3 或更高版本](../../ssms/download-sql-server-management-studio-ssms.md)。

或者，可在另一台计算机上安装 SSMS。

> [!WARNING]
> 在生产环境中，切不可使用 SSMS 或其他工具来管理 Always Encrypted 密钥，或对 SQL Server 计算机上的加密数据运行查询，因为这将削弱或完全违背使用 Always Encrypted 的目的。 有关详细信息，请参阅[密钥管理安全注意事项](encryption/overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)。

### <a name="hgs-computer-requirements"></a>HGS 计算机要求

- Windows Server 2019 Standard 或 Datacenter 版本
- 2 个 CPU
- 8 GB RAM
- 100 GB 存储

> [!NOTE]
> 在开始之前，不应将 HGS 计算机加入域。

## <a name="step-1-configure-the-hgs-computer"></a>步骤 1：配置 HGS 计算机

在此步骤中，将配置 HGS 计算机，以运行支持主机密钥证明的主机保护者服务。

1. 以管理员身份（本地管理员）登录 HGS 计算机，打开提升的 Windows PowerShell 控制台并运行以下命令，添加主机保护者服务角色：

   ```powershell
   Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
   ```

2. 重启 HGS 计算机后，再次使用管理员帐户登录，打开提升的 Windows PowerShell 控制台并运行以下命令，安装主机保护者服务并配置它的域。 此处指定的密码仅适用于 Active Directory 的目录服务修复模式密码，而不会更改管理员帐户的登录密码。 可以为 -HgsDomainName 提供所选的任何域名。

   ```powershell
   $adminPassword = ConvertTo-SecureString -AsPlainText '<password>' -Force
   Install-HgsServer -HgsDomainName 'bastion.local' -SafeModeAdministratorPassword $adminPassword -Restart
   ```

3. 再次重启计算机后，使用管理员帐户（现在也是域管理员）登录，打开提升的 Windows PowerShell 控制台，配置 HGS 实例的主机密钥证明。 

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey  
   ```

4. 运行以下命令，找到 HGS 计算机的 IP 地址。 保存此 IP 地址，以供后续步骤使用。

   ```powershell
   Get-NetIPAddress  
   ```

> [!NOTE]
> 或者，如果想通过 DNS 名称来引用 HGS 计算机，可设置一个转发器，从公司 DNS 服务器指向新的 HGS 域控制器。  

## <a name="step-2-configure-the-sql-server-computer-as-a-guarded-host"></a>步骤 2：将 SQL Server 计算机配置为受保护的主机
在此步骤中，需将 SQL Server 计算机配置为受保护的主机，并使用主机密钥证明向 HGS 注册。

> [!WARNING]
> 建议仅在测试环境中使用主机密钥证明。 对于生产环境，应使用 TPM 证明。

1. 以管理员身份登录到 SQL Server 计算机，打开提升的 Windows PowerShell 控制台，并通过访问 computername 变量检索计算机名称。

   ```powershell
   $env:computername 
   ```

2. 安装受保护的主机功能，还将安装 Hyper-V（如果尚未安装）。

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All
   ```

3. 系统提示 Hyper-V 安装完成时，请重启 SQL Server 计算机。

4. 如果 SQL Server 计算机是虚拟机、不支持 UEFI 安全启动的物理计算机或未配备 IOMMU 的物理计算机，则需要删除平台安全功能的 VBS 要求。
    1. 通过在提升的 PowerShell 控制台中的 SQL Server 计算机上运行以下命令，删除对安全启动和 IOMMU 的要求：

        ```powershell
       Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name RequirePlatformSecurityFeatures -Value 0
       ```

    1. 再次重启 SQL Server 计算机，使 VBS 以较低的要求联机。

        ```powershell
       Restart-Computer
       ```

5. 再次以管理员身份登录 SQL Server 计算机，打开提升的 Windows PowerShell 控制台，生成唯一主机密钥，并将生成的公钥导出到文件。

   ```powershell
   Set-HgsClientHostKey 
   Get-HgsClientHostKey -Path $HOME\Desktop\hostkey.cer
   ```

6. 将上一步生成的主机密钥文件手动复制到 HGS 计算机。 以下说明假定文件名为 hostkey.cer 并且你正在将其复制到 HGS 计算机的桌面。

7. 在 HGS 计算机上，打开提升的 Windows PowerShell 控制台并向 HGS 注册 SQL Server 计算机的主机密钥：

   ```powershell
   Add-HgsAttestationHostKey -Name <your SQL Server computer name> -Path $HOME\Desktop\hostkey.cer
   ```

8. 在 SQL Server 计算机上，在提升的 Windows PowerShell 控制台中运行以下命令，告诉 SQL Server 计算机前往何处进行证明。 请确保已指定位于两个地址的 HGS 计算机的 IP 地址或 DNS 名称。 

   ```powershell
   # use http, and not https
   Set-HgsClientConfiguration -AttestationServerUrl http://<IP address or DNS name>/Attestation -KeyProtectionServerUrl http://<IP address or DNS name>/KeyProtection/  
   ```

上述命令的结果应显示 AttestationStatus = Passed。

如果出现 HostUnreachable 错误，这意味着 SQL Server 计算机无法与 HGS 通信。 请确保可以连接到 HGS 计算机。

UnauthorizedHost 错误表示公钥未向 HGS 服务器注册，请重复步骤 5 和 6 以解决该错误。

如果所有其他方法均失败，请运行 Remove-HgsClientHostKey 并重复步骤 4-7。

## <a name="step-3-enable-always-encrypted-with-secure-enclaves-in-sql-server"></a>步骤 3：在 SQL Server 中启用具有安全 enclave 的 Always Encrypted

在此步骤中，将在 SQL Server 实例中启用使用 enclave 的 Always Encrypted 功能。

1. 以 sysadmin 身份使用 SSMS 连接到 SQL Server 实例，无需为数据库连接启用 Always Encrypted。
    1. 启动 SSMS。
    1. 在“连接到服务器”对话框中，指定服务器名称，选择身份验证方法，并指定凭据。
    1. 单击“选项 >>”，并选择“Always Encrypted”选项卡。
    1. 务必取消选中“启用 Always Encrypted (列加密)”复选框。

          ![连接到没有 Always Encrypted 的服务器](./encryption/media/always-encrypted-enclaves/connect-without-always-encrypted-ssms.png)

    1. 选择“连接”。

2. 打开新查询窗口，并执行下面的语句，以将安全 enclave 类型设置为基于虚拟化的安全性 (VBS)。

   ```sql
   EXEC sys.sp_configure 'column encryption enclave type', 1;
   RECONFIGURE;
   ```

3. 重启 SQL Server 实例，前述更改才会生效。 可在“对象资源管理器”中右键单击实例并选择“重启”，从而在 SSMS 中重启实例。 重启实例后，请重新连接到它。

4. 通过运行以下查询确认现已加载安全 enclave：

   ```sql
   SELECT [name], [value], [value_in_use] FROM sys.configurations
   WHERE [name] = 'column encryption enclave type';
   ```

    该查询应返回以下结果：  

    | name                           | 值 | value_in_use |
    | ------------------------------ | ----- | -------------- |
    | 列加密 enclave 类型 | 1     | 1              |

## <a name="step-4-create-a-sample-database"></a>步骤 4：创建示例数据库
在此步骤中，将创建包含一些示例数据的数据库，稍后将对其进行加密。

1. 使用上一步中的 SSMS 实例在查询窗口中执行下面的语句，以新建名为“ContosoHR”的数据库。

    ```sql
    CREATE DATABASE [ContosoHR];
    ```

1. 新建名为“Employees”的表。

    ```sql
    USE [ContosoHR];
    GO

    CREATE SCHEMA [HR];
    GO
    
    CREATE TABLE [HR].[Employees]
    (
        [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
        [SSN] [char](11) NOT NULL,
        [FirstName] [nvarchar](50) NOT NULL,
        [LastName] [nvarchar](50) NOT NULL,
        [Salary] [money] NOT NULL
    ) ON [PRIMARY];
    ```

1. 向“Employees”表添加一些员工记录。

    ```sql
    USE [ContosoHR];
    GO

    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('795-73-9838'
            , N'Catherine'
            , N'Abel'
            , $31692);

    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('990-00-6818'
            , N'Kim'
            , N'Abercrombie'
            , $55415);
    ```

## <a name="step-5-provision-enclave-enabled-keys"></a>步骤 5：预配已启用 enclave 的密钥

在此步骤中，将创建允许 enclave 计算的列主密钥和列加密密钥。

1. 使用上一步中的 SSMS 实例在“对象资源管理器”中展开数据库，并依次转到“安全性” > “Always Encrypted 密钥”。
1. 预配已启用 enclave 的新列主密钥：
    1. 右键单击“Always Encrypted 密钥”，然后选择“新列主密钥...” 。
    2. 选择列主密钥名称：CMK1。
    3. 请确保选择“Windows 证书存储(当前用户或本地计算机)”或“Azure Key Vault” 。
    4. 选择“允许 enclave 计算”。
    5. 如果选择了 Azure Key Vault，请登录到 Azure 并选择密钥保管库。 若要深入了解如何创建 Always Encrypted 的密钥保管库，请参阅 [Manage your key vaults from Azure portal](/archive/blogs/kv/manage-your-key-vaults-from-new-azure-portal)（从 Azure 门户管理密钥保管库）。
    6. 选择证书或 Azure Key Value 密钥（如果已存在），或单击“生成证书”按钮，创建新证书。
    7. 选择“确定”。

        ![允许 enclave 计算](encryption/media/always-encrypted-enclaves/allow-enclave-computations.png)

1. 创建已启用 enclave 的新列加密密钥：

    1. 右键单击“Always Encrypted 密钥”，然后选择“新建列加密密钥” 。
    2. 输入新列加密密钥的名称：CEK1。
    3. 在“列主密匙”下拉列表中，选择在上一步骤创建的列主密钥。
    4. 选择“确定”。

## <a name="step-6-encrypt-some-columns-in-place"></a>步骤 6：就地加密部分列

在这一步中，你将在服务器端 enclave 内加密“SSN”和“Salary”列中存储的数据，然后对数据测试 SELECT 查询。

1. 打开新 SSMS 实例，并连接到已为数据库连接启用 Always Encrypted 的 SQL Server 实例。
    1. 启动新 SSMS 实例。
    1. 在“连接到服务器”对话框中，指定服务器名称，选择身份验证方法，并指定凭据。
    1. 单击“选项 >>”，并选择“Always Encrypted”选项卡。
    1. 选中“启用 Always Encrypted (列加密)”复选框，并指定 enclave 证明 URL（例如，ht<span>tp://</span>hgs.bastion.local/Attestation）。

          ![使用 SSMS 连接到具有证明的服务器](./encryption/media/always-encrypted-enclaves/column-encryption-setting.png)

    1. 选择“连接”。
    1. 如果系统提示启用 Always Encrypted 参数化查询，请选择“启用”。

1. 使用相同的 SSMS 实例（已启用 Always Encrypted）打开新查询窗口，并通过运行以下查询来加密“SSN”和“Salary”列。

    ```sql
    USE [ContosoHR];
    GO

    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [SSN] [char] (11) COLLATE Latin1_General_BIN2
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [Salary] [money]
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
    ```

    > [!NOTE]
    > 请注意上面的脚本中用于为数据库清除查询计划缓存的 ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE 语句。 更改表后，需要清除访问该表的所有批处理和存储过程的计划，以刷新参数加密信息。 

1. 若要验证“SSN”和“Salary”列现在是否已加密，请使用没有为数据库连接启用 Always Encrypted 的 SSMS 实例打开新查询窗口，并执行以下语句。 查询窗口应返回“SSN”和“Salary”列中的加密值。 如果使用已启用 Always Encrypted 的 SSMS 实例执行相同查询，应该会看到已解密的数据。

    ```sql
    SELECT * FROM [HR].[Employees];
    ```

## <a name="step-7-run-rich-queries-against-encrypted-columns"></a>步骤 7：对加密列运行丰富查询

现在，可对加密列运行丰富查询。 部分查询处理将在服务器端 enclave 内执行。 

1. 在已启用 Always Encrypted 的SSMS 实例中，请确保也启用了 Always Encrypted 参数化。
    1. 在 SSMS 的主菜单中，选择“工具”。
    2. 选择“选项...”。
    3. 导航到“查询执行” > “SQL Server” > “高级”。
    4. 务必选中“启用 Always Encrypted 参数化”。
    5. 选择“确定”。
2. 打开新查询窗口，并粘贴和执行以下查询。 该查询应返回满足指定搜索条件的纯文本值和行。

    ```sql
    DECLARE @SSNPattern [char](11) = '%6818';
    DECLARE @MinSalary [money] = $1000;
    SELECT * FROM [HR].[Employees]
    WHERE SSN LIKE @SSNPattern AND [Salary] >= @MinSalary;
    ```

3. 在未启用 Always Encrypted 的 SSMS 实例中重试同一查询，并记录发生的故障。

## <a name="next-steps"></a>后续步骤

完成本教程之后，可以继续学习以下教程之一：

- [教程：使用具有安全 enclave 的 Always Encrypted 开发 .NET 应用程序](../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [教程：开发使用具有安全 enclave 的 Always Encrypted 的 .NET Framework 应用程序](tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)
- [教程：对使用随机加密的启用了 enclave 的列创建和使用索引](./tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md)

## <a name="see-also"></a>另请参阅

- [配置和使用具有安全 enclave 的 Always Encrypted](encryption/configure-always-encrypted-enclaves.md)
- [教程：[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中具有安全 enclave 的 Always Encrypted](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
