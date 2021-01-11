---
title: 启用加密连接 | Microsoft Docs
ms.custom: contperf-fy20q4
ms.date: 08/29/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
description: 跨通信通道加密数据。 使用 SQL Server 配置管理器为 SQL Server 数据库引擎的实例启用加密连接。
helpviewer_keywords:
- connections [SQL Server], encrypted
- SSL [SQL Server]
- Secure Sockets Layer (SSL)
- TLS [SQL Server]
- Transport Layer Security (TLS)
- encryption [SQL Server], connections
- cryptography [SQL Server], connections
- certificates [SQL Server], installing
- requesting encrypted connections
- installing certificates
- security [SQL Server], encryption
- TLS certificates
ms.assetid: e1e55519-97ec-4404-81ef-881da3b42006
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b18a3131329e0485221a0ae2cdaafd0726a4f31c
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878947"
---
# <a name="enable-encrypted-connections-to-the-database-engine"></a>启用数据库引擎的加密连接

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  了解如何跨通信通道加密数据。  通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器指定证书来为 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例启用加密连接。
 
 必须为服务器计算机预配证书。 在服务器计算机上预配证书，即[将其导入 Windows](#single-server)。 必须将客户端计算机设置为[信任证书的根颁发机构](#about)。  
  
> [!IMPORTANT]
> 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 开始，安全套接字层 (SSL) 已停止使用。 请改用传输层安全性 (TLS)。

## <a name="transport-layer-security-tls"></a>传输层安全 (TLS) (Transport Layer Security) (TLS)

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以使用传输层安全性 (TLS) 对通过网络在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例与客户端应用程序之间传输的数据进行加密。 TLS 加密在协议层执行，并可用于所有支持的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客户端。

客户端连接请求加密时，TLS 可用于服务器验证。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例在某个计算机上运行，该计算机已分配有一个由公共证书颁发机构颁发的证书，则计算机的标识和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例由受信任的根证书颁发机构的证书链担保。 这种服务器验证需要对运行客户端应用程序的计算机进行配置，使其信任服务器使用的证书的根证书颁发机构。 

也可以使用带有自签名的证书进行加密，如下一部分所述，但是自签名证书只能提供有限的保护。
TLS 使用的加密级别是 40 位还是 128 位，取决于应用程序和数据库计算机上运行的 Microsoft Windows 操作系统版本。

> [!WARNING]
> 使用 40 位加密级别被认为是不安全的。

> [!WARNING]
> 使用自签名证书加密的 TLS 连接不提供强安全性。 它们易遭受中间人攻击。 在生产环境中或在连接到 Internet 的服务器上，不应依赖使用自签名证书的 TLS。

启用 TLS 加密将增强通过网络在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例与应用程序之间传输的数据的安全性。 但如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 与客户端应用程序之间的所有通信流量都使用 TLS 加密，则还需要进行以下额外处理：
-  连接时需要进行额外的网络通信。
-  从应用程序发送到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的数据包必须由客户端 TLS 堆栈加密并由服务器 TLS 堆栈解密。
-  从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例发送到应用程序的数据包必须由服务器 TLS 堆栈加密并由客户端 TLS 堆栈解密。

## <a name="about-certificates"></a><a name="about"></a> 关于证书

 必须颁发证书才能进行 **服务器身份验证**。 证书名称必须为计算机的完全限定域名 (FQDN)。  
  
 用户的证书存储在本地计算机上。 若要安装证书以供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用，必须使用具有本地管理员权限的帐户运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器。

 客户端必须能够验证服务器所用证书的所有权。 如果客户端具有对服务器证书进行签名的证书颁发机构所颁发的公钥证书，则不需要进一步的配置。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 包含多个证书颁发机构所颁发的公钥证书。 如果服务器证书由公共或私人证书颁发机构进行签名，而客户端没有该机构颁发的公钥证书，则必须安装对服务器证书进行签名的证书颁发机构所颁发的公钥证书。  
  
> [!NOTE]  
> 若要在故障转移群集中使用加密，必须在故障转移群集的所有节点上安装带有虚拟服务器的完全限定 DNS 名称的服务器证书。 例如，如果你有一个双节点群集，节点名称分别为 _test1.\_\<your company>\*.com* 和 **_test2.\_\<your company>\*.com*** 并且又拥有名为 *virtsql_ 的虚拟服务器，你需要在两个节点上为 _ *_virtsql.\_\<your company>\*.com*** 安装证书。 可设置“SQL Server 网络配置”的“virtsql 的协议”属性框上的“ForceEncryption”选项，将其值设置为“是”   。

> [!NOTE]
> 在 Azure VM 上创建从 Azure 搜索索引器到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的加密连接时，请参阅 [在 Azure VM 上配置从 Azure 搜索索引器到 SQL Server 的连接](/azure/search/search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers)。 

## <a name="certificate-requirements"></a>证书要求

若要让 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 加载 TLS 证书，该证书必须满足以下条件：

- 该证书必须位于本地计算机证书存储区中，或者位于当前用户证书存储区中。

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户必须拥有访问 TLS 证书所必需的权限。

- 当前系统时间必须晚于证书的“有效起始时间”属性，并早于证书的“有效截止时间”属性。

- 该证书必须用于服务器身份验证。 这就要求，证书的“增强型密钥使用”属性必须指定“服务器身份验证(1.3.6.1.5.5.7.3.1)”。

- 必须使用 AT_KEYEXCHANGE 的 KeySpec 选项创建证书。 证书的密钥使用属性 (KEY_USAGE) 通常还包括密钥加密 (CERT_KEY_ENCIPHERMENT_KEY_USAGE)。

- 证书的“使用者”属性必须指明，证书公用名称 (CN) 与服务器计算机的主机名或完全限定的域名 (FQDN) 相同。 使用主机名时，必须在证书中指定 DNS 后缀。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在故障转移群集中运行，则公用名称必须与虚拟服务器的主机名或 FQDN 相同，并且在故障转移群集的所有节点上都必须提供这些证书。

- [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] 和 [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] Native Client (SNAC) 支持通配符证书。 SNAC 已被弃用，并已替换为 [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) 和 [Microsoft ODBC Driver for SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md)。 其他客户端可能不支持通配符证书。 有关详细信息，请参阅客户端文档和[知识库 (KB) 258858](https://support.microsoft.com/kb/258858)。       
  无法使用 SQL Server 配置管理器选择通配符证书。 要使用通配符证书，必须编辑 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQLServer\SuperSocketNetLib` 注册表项，并为“证书”值输入证书的指纹（不留空格）。  

  > [!WARNING]  
  > [!INCLUDE[ssnoteregistry_md](../../includes/ssnoteregistry-md.md)]  

## <a name="install-on-single-server"></a><a name="single-server"></a>在单一服务器上安装

在 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 中，证书管理已集成到 SQL Server 配置管理器。 适用于 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 的 SQL Server 配置管理器可用于早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 在单个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上添加证书时，请参阅[证书管理（SQL Server 配置管理器）](../../database-engine/configure-windows/manage-certificates.md)。

如果使用 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]，但适用于 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 的 SQL Server 配置管理器不可用，请执行以下步骤：

1. 在“开始”  菜单上，单击“运行” ，并在“打开”  框中键入 **MMC** ，然后单击“确定” 。  
  
2. 在 MMC 控制台的“文件”菜单上，单击“添加/删除管理单元”。  
  
3. 在“添加/删除管理单元”对话框中，单击“添加”。  
  
4. 在“添加独立管理单元”对话框中，单击“证书”，再单击“添加”。  
  
5. 在“证书管理单元”对话框中，单击“计算机帐户”，再单击“完成”。  
  
6. 在“添加独立管理单元”对话框中，单击“关闭”。  
  
7. 在“添加/删除管理单元”对话框中，单击“确定”。  
  
8. 在“证书”管理单元中，依次展开“证书”和“个人”，右键单击“证书”，指向“所有任务”，然后单击“导入”。  

9. 右键单击导入的证书，指向“所有任务”，然后单击“管理私钥”。 在“安全”对话框中，为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户使用的用户帐户添加读取权限。  
  
10. 完成 **证书导入向导**，将证书添加到计算机中，然后关闭 MMC 控制台。 有关向计算机添加证书的详细信息，请参阅 Windows 文档。  

> [!IMPORTANT]
> 对于生产环境，建议从证书颁发机构获取可信证书。    
> 在测试中，也可以使用自签名证书。 若要创建自签名证书，请参阅 [Powershell Cmdlet New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) 或 [certreq 命令](/windows-server/administration/windows-commands/certreq_1)。
  
## <a name="install-across-multiple-servers"></a>跨多个服务器安装

在 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 中，证书管理已集成到 SQL Server 配置管理器。 适用于 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 的 SQL Server 配置管理器可用于早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 在故障转移群集配置中或在可用性组配置中添加证书时，请参阅[证书管理（SQL Server 配置管理器）](../../database-engine/configure-windows/manage-certificates.md)。

如果使用 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]，但适用于 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 的 SQL Server 配置管理器不可用，请对每个服务器执行[在单个服务器中预配（安装）证书](#single-server)一节中的步骤。

## <a name="export-server-certificate"></a>导出服务器证书  
  
1. 在“证书”管理单元中的“证书” / “个人”文件夹中找到证书，右键单击“证书”，指向“所有任务”，然后单击“导出”。  
  
2. 完成 **“证书导出向导”** ，将证书文件存储在方便的位置。  
  
## <a name="configure-server"></a>配置服务器

将服务器配置为强制使用加密连接。

> [!IMPORTANT]
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户必须具有对用于在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上强制加密的证书的读取权限。 对于非特权服务帐户，需要将读取权限添加到证书中， 否则可能会导致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务重启失败。
  
1. 在“SQL Server 配置管理器”中，展开“SQL Server 网络配置”，右键单击“\<server instance> 的协议”，然后选择“属性”。    
  
2. 在“ 属性的协议”对话框中的“证书”选项卡上，从“证书”框的下拉菜单中选择所需证书，然后单击“确定”。 _\<instance name>_      
  
3. 在 **“标志”** 选项卡的 **“ForceEncryption”** 框中，选择 **“是”** ，然后单击 **“确定”** 关闭该对话框。  
  
4. 重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。  

> [!NOTE]
> 要确保客户端和服务器之间的安全连接，请将客户端配置为请求加密连接。 [本文稍后部分](#configure-client)将介绍更多详细信息。

## <a name="configure-client"></a><a name="configure-client"></a>配置客户端

将客户端配置为请求加密连接。

1. 将原始证书或导出的证书文件复制到客户端计算机。  
  
2. 在客户端计算机上，使用“证书”管理单元来安装根证书或导出的证书文件。  
  
3. 使用 SQL Server 配置管理器，右键单击“SQL Server Native Client 配置”，然后单击“属性” 。  
  
4. 在 **“标志”** 选项卡的 **“强制协议加密”** 框中，单击 **“是”** 。  
  
## <a name="use-sql-server-management-studio"></a>使用 SQL Server Management Studio
  
若要从 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 加密连接，请执行以下操作：  

1. 在对象资源管理器工具栏上，单击 **“连接”** ，再单击 **“数据库引擎”** 。  
  
2. 在 **“连接到服务器”** 对话框中，填写连接信息，然后单击 **“选项”** 。  
  
3. 在 **“连接属性”** 选项卡上，单击 **“加密连接”** 。  

## <a name="internet-protocol-security-ipsec"></a>Internet 协议安全性 (IPSec)
可以使用 IPSec 在传输过程中对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据进行加密。 IPSec 是由客户端和服务器操作系统提供的，不需要进行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置。 有关 IPSec 的信息，请参阅 Windows 文档或网络文档。

## <a name="next-steps"></a>后续步骤

+ [针对 Microsoft SQL Server 的 TLS 1.2 支持](https://support.microsoft.com/kb/3135244)     
+ [配置 Windows 防火墙以允许 SQL Server 访问](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)     
+ [Powershell Cmdlet New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate)