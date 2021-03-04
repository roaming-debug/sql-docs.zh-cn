---
title: Microsoft Azure 中的 SQL Server 数据文件 | Microsoft Docs
description: 了解一些对于在 Microsoft Azure 存储中存储 SQL Server 数据文件至关重要的概念和注意事项以及使用 Azure 存储的一些优点。
ms.custom: ''
ms.date: 12/04/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
ms.assetid: 38ffd9c2-18a5-43d2-b674-e425addec4e4
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 952e8d393733c62f60c5b6792b422d22417ddede
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837929"
---
# <a name="sql-server-data-files-in-microsoft-azure"></a>Microsoft Azure 中的 SQL Server 数据文件
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  ![Azure 上的数据文件](../../relational-databases/databases/media/data-files-on-azure.png "Azure 上的数据文件")  
  
Microsoft Azure 中的 SQL Server 数据文件可为作为 blob 存储的 SQL Server 数据库文件提供本机支持。 通过此功能，你可以在本地或在 Microsoft Azure 中虚拟机上运行的 SQL Server 中创建数据库，而将数据存储在 Microsoft Azure blob 存储中的专用存储位置。 此功能还简化了在计算机之间移动数据库的过程。 你可以从一台计算机中分离数据库，并将它们附加到另一台计算机。 此外，它还允许你将数据库备份文件从 Microsoft Azure 存储空间还原或还原到 Microsoft Azure 存储空间，为数据库备份文件提供了备选存储位置。 因此，它在数据虚拟化、数据移动、安全性和可用性、轻松降低成本以及维护方面都具备优势，可实现高可用性和弹性扩展，支持几种混合解决方案。
 
> [!IMPORTANT]  
>  不推荐且不支持 Azure Blob 存储中的存储系统数据库。 

 本主题介绍一些概念和注意事项，这些内容对于在 Microsoft Azure 存储服务中存储 SQL Server 数据文件至关重要。  
  
 有关如何使用此新功能的实践体验，请参阅[教程：将 Microsoft Azure Blob 存储服务用于 SQL Server 2016 数据库](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)。  
  
## <a name="why-use-sql-server-data-files-in-microsoft-azure"></a>为何在 Microsoft Azure 中使用 SQL Server 数据文件？ 

- **迁移方便快捷优势：** 此功能在本地计算机之间以及在本地与云环境中的计算机之间一次迁移一个数据库，从而简化了迁移过程，而无需进行任何应用程序更改。 因此，它能在保持现有本地基础架构不变的情况下支持增量迁移。 此外，在应用程序需要在本地环境多个位置运行时，对集中数据存储的访问权限可以简化应用程序逻辑。 在某些情况下，可能需要在地理上分散的位置快速设置计算机中心，以便从很多不同来源收集数据。 通过此新增强功能，无需将数据从一个位置移动到另一个位置，你可以将很多数据库以 Microsoft Azure Blob 的形式进行存储，然后运行 Transact-SQL 脚本在本地计算机或虚拟机中创建数据库。

- **成本和无限制存储优势：** 借助此功能，不仅可以在 Microsoft Azure 中使用无限制的站外存储，还能利用本地计算机资源。 当使用 Microsoft Azure 作为存储位置时，可以轻松将精力放在应用程序逻辑上，避免了硬件管理方面的开销。 如果丢失一个本地计算节点，无需进行任何数据移动操作即可设置新节点。

- **高可用性和灾难恢复优势：** 使用 Microsoft Azure 中的 SQL Server 数据文件功能，可简化高可用性和灾难恢复解决方案。 例如，如果 Microsoft Azure 中的虚拟机或 SQL Server 实例崩溃，只需重新建立到 blob 的链接，便可在新 SQL Server 实例中重新创建数据库。

- **安全性优势：** 通过此新增强功能可将计算实例与存储实例分离。 您可以仅在计算实例而不在存储实例中对完全加密的数据库进行解密。 换句话说，使用此新增强功能，您可以使用透明数据加密 (TDE) 证书（此证书与数据在物理上是分离的）来加密在公共云中的所有数据。 TDE 密钥可存储在主数据库中，该数据库存储在物理上安全的本地计算机中，并在本地备份。 你可以使用这些本地密钥来加密驻留在 Microsoft Azure 存储空间中的数据。 即使您的云存储帐户凭据被盗，数据仍然安全，因为 TDE 证书始终驻留在本地。

- **快照备份：** 利用此功能，可以使用 Azure 快照提供近乎即时的备份，并能更快速地恢复使用 Azure Blob 存储服务存储的数据库文件。 使用此功能可简化备份和还原策略。 有关详细信息，请参阅 [Azure 中数据库文件的文件快照备份](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)。  

## <a name="concepts-and-requirements"></a>概念和要求  
  
Azure 磁盘与企业范围的业务连续性和灾难恢复解决方案兼容。 如果直接将数据库存储在 blob 上或 Azure Premium 文件中，则数据不会自动与 VM 关联以构建基础结构以及进行管理和监视。

与使用 Azure 磁盘（此功能简单易用）相比，将数据库文件放在页 blob 上一种更高级的功能。

基本指导是使用 Azure 磁盘，除非你的情况确实需要避免创建用于备份的数据副本或避免将其还原为数据大小的操作。 为了实现高可用性和灾难恢复，URL 定期备份或 blob 存储托管备份也比文件快照备份更有用，因为你可以获得生命周期管理、多区域支持、软删除以及 blob 存储的所有其他功能。

### <a name="azure-storage-concepts"></a>Azure 存储概念  
使用“Azure 中的 SQL Server 数据文件”功能时，需要在 Azure 中创建一个存储帐户和一个容器。 然后，需要创建一个 SQL Server 凭据，其中包括有关容器策略的信息以及访问容器所需的共享访问签名。  

在 [Windows Azure](https://azure.microsoft.com) 中，[Azure 存储](https://azure.microsoft.com/services/storage/)帐户代表用于访问 blob 的命名空间的最高级别。 一个存储帐户可包含无限数量的容器，前提是这些容器的总大小低于存储限制。 有关存储限制的最新信息，请参阅 [Azure 订阅与服务限制、配额和约束](/azure/azure-subscription-service-limits)。 容器对 [blob ](/azure/storage/common/storage-introduction#blob-storage)集进行分组。 所有 Blob 必须都在一个容器中。 一个帐户可以包含无限数量的容器。 同样，一个容器可以存储无限数量的 blob。 可将两类 Blob 存储到 Azure 存储中：块 Blob 和页 Blob。 这一新功能使用页 Blob，在文件字节数经常发生修改的情况下这些 Blob 更为高效。 你可以使用以下 URL 格式访问 blob：`https://storageaccount.blob.core.windows.net/<container>/<blob>`。  

### <a name="azure-billing-considerations"></a>Azure 计费注意事项  

 在决策制定和规划过程中，估计使用 Azure 服务的成本是一项非常重要的工作。 在 Azure 存储中存储 SQL Server 数据文件时，需要支付与存储和事务相关的成本。 此外，要实现“Azure 存储中的 SQL Server 数据文件”功能，需要每 45 到 60 秒隐式进行一次 blob 续租。 这也会对每个数据库文件（如 .mdf 或 .ldf）造成事务成本。 使用 [Azure 定价](https://azure.microsoft.com/pricing/)页面上的信息来帮助估计每月与使用 Azure 存储和 Azure 虚拟机相关的成本。  
  
### <a name="sql-server-concepts"></a>SQL Server 概念  

使用此新增强功能时，需要执行以下操作：

- 创建容器策略并生成共享访问签名 (SAS) 密钥。

- 对于数据或日志文件使用的每个容器，请创建名称与容器路径匹配的 SQL Server 凭据。

- 在 SQL Server 凭据存储中存储有关 Azure 存储容器、其关联策略名称以及 SAS 密钥的信息。

以下示例假定已经创建 Azure 存储容器以及读取、写入和列出权限策略。 创建容器策略会生成一个 SAS 密钥，它以未加密状态安全存储在内存中，SQL Server 需要使用它来访问容器中的 blob 文件。 

在以下代码段中，将 `'<your SAS key>'` 替换为 SAS 密钥。 SAS 密钥类似于 `'sr=c&si=<MYPOLICYNAME>&sig=<THESHAREDACCESSSIGNATURE>'`。

```sql
CREATE CREDENTIAL [https://testdb.blob.core.windows.net/data]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = '<your SAS key>'  
  
CREATE DATABASE testdb   
ON  
( NAME = testdb_dat,  
    FILENAME = 'https://testdb.blob.core.windows.net/data/TestData.mdf' )  
 LOG ON  
( NAME = testdb_log,  
    FILENAME =  'https://testdb.blob.core.windows.net/data/TestLog.ldf')  
```  

>[!IMPORTANT]
>如果当前存在任何对容器中数据文件的引用，则尝试删除相应的 SQL Server 凭据会失败。

有关详细信息，请参阅 [管理对 Azure 存储资源的访问权限](/azure/storage/blobs/storage-manage-access-to-resources)  

### <a name="security"></a>安全性  
 以下是在 Azure 存储中存储 SQL Server 数据文件时的安全注意事项和要求。

- 为 Azure Blob 存储服务创建容器时，建议将访问权限设置为“私有”。 将访问权限设置为“私有”后，只有 Azure 帐户所有者才可读取容器和 Blob 数据。

- 在 Azure 存储中存储 SQL Server 数据库文件时，需要使用共享访问签名，它是授予对容器、Blob、队列和表的受限访问权限的 URI。 通过使用共享访问签名，可以让 SQL Server 在不共享 Azure 存储帐户密钥的情况下访问存储帐户中的资源。

- 此外，建议对数据库继续实施传统的本地安全做法。  
  
### <a name="installation-prerequisites"></a>安装的先决条件  
 以下是在 Azure 中存储 SQL Server 数据文件时的安装先决条件。

- **本地 SQL Server：** SQL Server 2016 及更高版本包括此功能。 若要了解如何下载最新版本的 SQL Server，请参阅 [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)。

- 在 Azure 虚拟机中运行的 SQL Server：如果要在 [Azure 虚拟机上安装 SQL Server](https://azuremarketplace.microsoft.com/marketplace/apps?search=sql%20server&page=1)，请安装 SQL Server 2016，或更新现有实例。 同样，也可以使用 SQL Server 2016 平台映像在 Azure 中创建新虚拟机。

  
###  <a name="limitations"></a><a name="bkmk_Limitations"></a> 限制  
  
- 由于 SQL Server 工作负载的性能特征，SQL Server 数据文件实现为 Azure Blob 存储中的页 blob。 不支持其他类型的 blob 存储，如块 blob 或 [Azure Data Lake Storage](/azure/storage/blobs/data-lake-storage-introduction)。

- 在此功能的最新版本中，不支持在 Azure 存储中存储 **FileStream** 数据。 可以在数据库中存储 **FileStream** 文件，该数据库还包含存储在 Azure 存储中的数据文件，但所有 FileStream 数据文件都必须存储在本地存储中。  由于 FileStream 数据必须驻留在本地存储中，不能使用 Azure 存储在计算机之间移动它，我们建议继续使用[传统技术](../../relational-databases/blob/move-a-filestream-enabled-database.md)在不同的计算机之间移动与 FileStream 关联的数据。  
  
- 目前，此新增强功能不支持多个 SQL Server 实例同时访问 Azure 存储中的相同数据库文件。 如果 ServerA 处于联机状态并且包含一个活动数据库文件，ServerB 意外启动并且也包含一个指向相同数据文件的数据库，则第二个服务器将无法启动该数据库，并显示错误代码 5120，指示无法打开物理文件“%.ls”\* **。操作系统错误 %d：“%ls”** 。  
  
- 只有 .mdf、.ldf 和 .ndf 文件才可以通过使用 Azure 功能中的 SQL Server 数据文件存储在 Azure 存储中。  
  
- 使用“Azure 中的 SQL Server 数据文件”功能时，存储帐户不支持异地复制。 如果对存储帐户进行地理复制并发生地理故障转移，则可能出现数据库损坏。  
  
- 有关容量限制，请参阅 [Blob 存储简介](/azure/storage/blobs/storage-blobs-introduction)。  
  
- 无法使用“Azure 存储中的 SQL Server 数据文件”功能在 blob 存储中存储内存中 OLTP 数据。 这是因为内存 OLTP 依赖于 **FileStream** ，并且，在此功能的最新版本中，不支持在 Azure 存储中存储 **FileStream** 数据。  
  
- 使用“Azure 中的 SQL Server 数据文件”功能时，SQL Server 使用 master 数据库中的排序规则集来执行所有 URL 或文件路径比较。  
  
- 只要不在主数据库中添加新数据库文件，就支持 AlwaysOn 可用性组。 如果某个数据库操作需要在主数据库中创建新文件，请首先在次要节点上禁用可用性组。 然后对主数据库执行该数据库操作，并在主节点中备份该数据库。 接下来，将该数据库还原到次要节点。 完成后，重新启用次要节点中的 Alwys On 可用性组。 

   >[!NOTE]
   >使用“Azure 中的 SQL Server 数据文件”功能时，不支持 Always On 故障转移群集实例。
  
- 在正常操作期间，SQL Server 使用临时租约来保留用于存储的 blob，并且每 45 到 60 秒续租每个 blob。 如果服务器崩溃，并启动另一个配置为使用相同 blob 的 SQL Server 实例，则新实例将等待 60 秒，以待现有 blob 租约过期。 如果要将数据库附加到另一个实例，又无法在 60 秒内等待租约过期，则可显式释放 blob 租约。
  
## <a name="tools-and-programming-reference-support"></a>工具和编程参考支持  
 本节介绍在 Azure 存储中存储 SQL Server 数据文件时可使用的工具和编程参考库。  
  
### <a name="powershell-support"></a>PowerShell 支持  
 通过引用 blob 存储 URL 路径而不是文件路径，可以使用 PowerShell cmdlet 将 SQL Server 数据文件存储在 blob 存储服务中。 使用以下 URL 格式访问 blob：`https://storageaccount.blob.core.windows.net/<container>/<blob>`。  
  
### <a name="sql-server-object-and-performance-counters-support"></a>SQL Server 对象和性能计数器支持  
 从 SQL Server 2014 起，增加了一个与“Azure 存储中的 SQL Server 数据文件”功能结合使用的新 SQL Server 对象。 这个新的 SQL Server 对象称为 [SQL Server, HTTP_STORAGE_OBJECT](../../relational-databases/performance-monitor/sql-server-http-storage-object.md)。系统监视器可使用它在用 Azure 存储运行 SQL Server 时监视活动。  
  
### <a name="sql-server-management-studio-support"></a>SQL Server Management Studio 支持  
 使用 SQL Server Management Studio 时，您可通过多个对话框窗口使用此功能。 例如，`https://teststorageaccnt.blob.core.windows.net/testcontainer/` 表示存储容器的 URL 路径。
 
 将其作为一些对话框窗口的“路径”，例如“新建数据库”、“附加数据库”和“还原数据库”。    有关详细信息，请参阅[教程：将 Microsoft Azure Blob 存储服务用于 SQL Server 2016 数据库](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)。  
  
### <a name="sql-server-management-objects-smo-support"></a>SQL Server 管理对象 (SMO) 支持  
 使用“Azure 中的 SQL Server 数据文件”功能时，支持所有 SQL Server 管理对象 (SMO)。 如果 SMO 对象需要文件路径，请使用 BLOB URL 格式而不是本地文件路径，如 `https://teststorageaccnt.blob.core.windows.net/testcontainer/`。 有关 SQL Server 管理对象的详细信息，请参阅 SQL Server 联机丛书中的 [SQL Server 管理对象 (SMO) 编程指南](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md)。  
  
### <a name="transact-sql-support"></a>Transact-SQL 支持  
 此新功能在 Transact-SQL 外围应用中引入了以下更改：

- **sys.master_files** 系统视图中新增一个 **int** 列，即 **credential_id** 。 credential_id 列用于实现将 Azure 存储数据文件交叉引用回为其创建的凭据的 `sys.credentials`。 你可以使用它来排除故障，例如，当有数据库文件使用凭据时无法删除凭据。  
  
##  <a name="troubleshooting-for-sql-server-data-files-in-microsoft-azure"></a><a name="bkmk_Troubleshooting"></a> 排查“Microsoft Azure 中的 SQL Server 数据文件”功能问题  
 为避免不受支持的功能或限制造成的错误，首先请查看 [Limitations](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md#bkmk_Limitations)。  
  
 下面列出了使用“Azure 存储中的 SQL Server 数据文件”功能时可能遇到的错误。  
  
 **身份验证错误**  
  
- *凭据 '%.\*ls' 被某活动数据库文件使用，因此无法删除它。*    
    解决方法：尝试删除 Azure 存储中活动数据库文件仍在使用的凭据时，可能会看到此错误。 要删除凭据，必须先删除拥有此数据库文件的关联 Blob。 要删除具有活动租约的 blob，必须先释放租约。  
  
- *未在容器上正确创建共享访问签名。*    
     解决方法：确保已在容器上正确创建了共享访问签名。 有关第 2 课中提供的说明，请参阅[教程：将 Microsoft Azure Blob 存储服务用于 SQL Server 2016 数据库](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature)。  
  
- *尚未正确创建 SQL Server 凭据。*    
    解决方法：确保已对“标识”字段使用了“共享访问签名”，并正确创建了机密。 有关第 3 课中提供的说明，请参阅[教程：将 Microsoft Azure Blob 存储服务用于 SQL Server 2016 数据库](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#3---database-backup-to-url)。  
  
 **租赁 Blob 错误：**  
  
- 在使用相同 Blob 文件的 SQL Server 实例崩溃之后，尝试启动另一个 SQL Server 时发生错误。 解决方法：在正常操作期间，SQL Server 使用临时租约来保留用于存储的 blob，并且每 45 到 60 秒续租每个 blob。 如果服务器崩溃，并启动另一个配置为使用相同 blob 的 SQL Server 实例，则新实例将等待 60 秒，以待现有 blob 租约过期。 如果要将数据库附加到另一个实例，又无法在 60 秒内等待租约过期，则可显式释放 blob 租约，以免执行附加操作时发生任何故障。  
  
 **数据库错误**  
  
**创建数据库时出错** 解决方法：有关第 4 课中提供的说明，请参阅 [教程：将 Microsoft Azure Blob 存储服务用于 SQL Server 2016 数据库](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#4----restore-database-to-virtual-machine-from-url)。  
  
**运行 Alter 语句时出错** 解决方案：确保在数据库联机时执行 Alter Database 语句。 将数据文件复制到 Azure 存储时，始终创建页 Blob 而不是块 Blob。 否则，ALTER Database 将失败。 有关第 7 课中提供的说明，请参阅[教程：将 Microsoft Azure Blob 存储服务用于 SQL Server 2016 数据库](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)。  
  
**错误代码 - 5120 无法打开物理文件“%.\*ls”。操作系统错误 %d：“%ls”**   

解决方法：目前，此新增强功能不支持多个 SQL Server 实例同时访问 Azure 存储中的相同数据库文件。 如果 ServerA 处于联机状态并且包含一个活动数据库文件，ServerB 意外启动并且也包含一个指向相同数据文件的数据库，则第二个服务器将无法启动该数据库，并显示错误代码 5120，指示无法打开物理文件“%.ls”\* *。操作系统错误 %d：“%ls”* 。  
  
要解决此问题，首先需要确定是否需要 ServerA 访问 Azure 存储中的数据库文件。 如果不需要，请删除 ServerA 与 Azure 存储中数据库文件之间的任何连接。 为此，请按照下列步骤进行操作：  

1.  使用 ALTER Database 语句将 ServerA 的文件路径设置为本地文件夹。  

2.  在 ServerA 中将数据库设置为脱机状态。  

3.  然后，将数据库文件从 Azure 存储复制到 Server A 中的本地文件夹。这可确保 ServerA 仍有一个本地数据库副本。  

4.  将数据库设置为联机状态。

**错误代码 833 - I/O 请求完成所用的时间超过 15 秒** 
   
   此错误表示存储系统无法满足 SQL Server 工作负荷的需求。 要么减少应用程序层的 IO 活动，要么提高存储层的吞吐能力。 若要了解更多信息，请参阅[错误 833](../errors-events/mssqlserver-833-database-engine-error.md)。 如果性能问题仍然存在，请考虑将文件移到其他存储层，如 Premium 或 UltraSSD。 有关 Azure VM 上的 SQL Server，请参阅[优化存储性能](/azure/virtual-machines/premium-storage-performance)。


## <a name="next-steps"></a>后续步骤  
  
[创建数据库](create-a-database.md)