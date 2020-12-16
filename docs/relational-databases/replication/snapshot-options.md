---
title: 修改快照初始化选项
description: 在 SQL Server Management Studio 中修改各种复制快照初始化选项（如快照格式和快照文件夹位置）。
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- snapshot replication [SQL Server], options
- snapshots [SQL Server replication], options
ms.assetid: 759fab42-66c7-4541-a7a3-bb6fb868493c
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 4a51a126cb16aed9159177b60472c4e5262eb2ad
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479708"
---
# <a name="modify-snapshot-initialization-options-for-sql-replication"></a>修改 SQL 复制的快照初始化选项 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[使用快照初始化订阅](initialize-a-subscription-with-a-snapshot.md)时有许多可用选项。

## <a name="specify-snapshot-format-sql-server-management-studio"></a>指定快照格式 (SQL Server Management Studio)
  在“发布属性 -  \<Publication>”对话框的“快照”页上指定快照格式。 有关访问此对话框的详细信息，请参阅 [View and Modify Publication Properties](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
### <a name="to-specify-snapshot-format"></a>指定快照格式  
  
1.  在“发布属性 - \<Publication>”对话框的“快照”页上，选择“本机 SQL Server - 所有订阅服务器都必须是运行 SQL Server 的服务器”或“字符 - 如果发布服务器或订阅服务器没有运行 SQL Server，则需要此项”。  
  
    > [!NOTE]  
    >  建议选择本机格式，除非此发布必须支持对 SQL Server Compact 数据库或非 SQL Server 数据库的订阅。  
  
2.  选择“确定”。   

## <a name="snapshot-folder-locations"></a>快照文件夹位置

### <a name="default-snapshot-location"></a>默认快照位置
可以在配置分发向导的 **“快照文件夹”** 页上指定默认快照位置。 有关使用此向导的详细信息，请参阅[配置发布和分发](../../relational-databases/replication/configure-publishing-and-distribution.md)。 如果在未配置为分发服务器的服务器上创建发布，请在新建发布向导的 **“快照文件夹”** 页上指定默认快照位置。 有关使用此向导的详细信息，请参阅[创建发布](../../relational-databases/replication/publish/create-a-publication.md)。  
  
 在“分发服务器属性 - \<Distributor>”对话框的“发布服务器”页上，修改默认快照位置 。 有关详细信息，请参阅[查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)。 在“发布属性 - \<Publication>”对话框中设置每个发布的快照文件夹。 有关详细信息，请参阅 [View and Modify Publication Properties](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
### <a name="to-modify-the-default-snapshot-location"></a>修改默认快照位置  
  
1.  在“分发服务器属性 - \<Distributor>”对话框的“发布服务器”页上，单击要更改其默认快照位置的发布服务器的属性按钮 (...)  。    
2.  在“分发服务器属性 - \<Publisher>”对话框中，为“默认快照文件夹”属性输入一个值 。  
  
    > [!NOTE]  
    >  快照代理必须对指定的目录具有写权限，而分发代理或合并代理必须具有读权限。 如果使用的是请求订阅，则必须指定一个共享目录作为通用命名约定 (UNC) 路径，如 \\\computername\snapshot。 有关详细信息，请参阅[保护快照文件夹](../../relational-databases/replication/security/secure-the-snapshot-folder.md)。    
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
 

### <a name="alternate-snapshot-locations"></a>备用快照位置
通过使用备用快照位置，可以将快照文件存储在默认位置以外的位置，默认位置通常在分发服务器上。 备用位置可以在另一台服务器、网络驱动器或可移动介质（如 CD-ROM 或可移动磁盘）上。  
  
备用快照位置存储为发布的一项属性。 因为备用快照位置是一项发布属性，所以分发代理和合并代理在同步处理的过程中就能定位适当的快照。  
  
如果要指定一个备用快照文件夹位置或压缩快照文件，请创建发布但不立即创建初始快照，为该快照位置设置发布属性，然后为该发布运行快照代理。 如果在创建初始快照后更改了备用位置，则为该发布生成的任何快照的位置都不会重定位到新的备用位置。 在这种情况下，根据发布设置的不同，合并代理或分发代理可能找不到新备用位置的快照文件。  
  
> [!NOTE]  
>  请不要（使用“发布属性”对话框或 [sp_changepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)）指定与默认快照文件夹位置相同的备用位置。  
  
> [!CAUTION]  
>  不要同时使用 WebSync 和备用快照文件夹位置。  
  
#### <a name="use-sql-server-management-studio"></a>使用 SQL Server Management Studio
1.  在“发布属性 -  \<Publication>”对话框的“快照”页上：  
  
    1.  选择 **“将文件放入下列文件夹”** ，然后单击 **“浏览”** 定位到某个目录，或者输入用于存储快照文件的目录的路径。  
  
        > [!NOTE]  
        >  快照代理必须对指定的目录具有写权限，而分发代理或合并代理必须具有读权限。 如果使用的是请求订阅，则必须指定一个共享目录作为通用命名约定 (UNC) 路径，如 \\\computername\snapshot。 有关详细信息，请参阅[保护快照文件夹](../../relational-databases/replication/security/secure-the-snapshot-folder.md)。  
  
    2.  除非需要将快照文件同时写入两个位置，否则应清除 **“将文件放入默认文件夹”** 。  
  
     若要压缩快照文件，请选择 **“压缩此文件夹中的快照文件”** 。 压缩通常用于低带宽连接和可移动介质（如 CD-ROM）上的备用快照位置。  
  
2.  选择“确定”。  
  
#### <a name="use-transact-sql"></a>使用 Transact-SQL 

在[配置快照属性（复制 Transact-SQL 编程）](../../relational-databases/replication/publish/configure-snapshot-properties-replication-transact-sql-programming.md)时，将 snapshot_in_defaultfolder 的值指定为 false。 

## <a name="compressed-snapshots"></a>压缩的快照
  如果要在慢速网络上传输快照，或者将快照存储到可移动介质上而未压缩的快照太大而不适于存储在介质上，则应该压缩快照文件。 在这些情况下，压缩快照文件很有用，但压缩增加了生成和应用快照的时间。  
  
 压缩快照文件采用了 [!INCLUDE[msCoName](../../includes/msconame-md.md)] CAB 文件格式，这种格式可以压缩不超过 2 GB 的文件（无法压缩超过 2 GB 的快照文件）。 若要压缩文件，必须将这些文件写入一个备用快照文件夹（无法压缩写入默认快照文件夹中的文件）。 
  
 文件在分发代理或合并代理运行的位置解压缩；请求通常将订阅与压缩快照一起使用，以便在订阅服务器上解压缩文件。 订阅服务器接收到压缩文件时，首先将文件写入一个临时位置。 压缩文件复制到订阅服务器后，压缩文件中的快照文件将由 CAB 实用工具按顺序逐个解压缩。 订阅服务器上所需的空间为压缩文件的大小与最大的未压缩文件的大小之和。  
  
> [!NOTE]  
>  在某些情况下，压缩的快照可以提高通过网络传输快照文件的性能。 但是，如果压缩快照，则在生成快照文件时需要通过快照代理进行额外处理，而在应用快照文件时需要通过分发代理或合并代理进行额外处理。 在某些情况下，这可能会降低生成快照的速度，并增加应用快照所需的时间。 此外，如果发生网络故障，压缩的快照将无法恢复，因此不适用于不可靠的网络。 在网络中使用压缩的快照时，应仔细权衡这些利弊。  
  
### <a name="use-sql-server-management-studio"></a>使用 SQL Server Management Studio
1.  在“发布属性 -  \<Publication>”对话框的“快照”页上：  
  
    1.  选择 **“将文件放入下列文件夹”** ，然后单击 **“浏览”** 定位到某个目录，或者输入用于存储快照文件的目录的路径。  
  
        > [!NOTE]  
        >  快照代理必须对指定的目录具有写权限，而分发代理或合并代理必须具有读权限。 如果使用的是请求订阅，则必须指定一个共享目录作为通用命名约定 (UNC) 路径，如 \\\computername\snapshot。 有关详细信息，请参阅[保护快照文件夹](security/secure-the-snapshot-folder.md)  
  
    2.  除非需要将快照文件同时写入两个位置，否则应清除 **“将文件放入默认文件夹”** 。  
  
        > [!NOTE]  
        >  如果选中了此复选框，则默认文件夹中存储的文件将不压缩。 压缩文件只能存储在上一步骤中指定的备用位置中。  
  
2.  选择 **“压缩此文件夹中的快照文件”** 。    
3.  选择“确定”。   

### <a name="use-transact-sql"></a>使用 Transact-SQL

[配置快照属性](../../relational-databases/replication/publish/configure-snapshot-properties-replication-transact-sql-programming.md)时，将值 compress_snapshot 指定为 True 。 

## <a name="execute-scripts-before-and-after-snapshot-is-applied"></a>在应用快照之前和之后执行脚本
可以指定在应用快照之前或之后在订阅服务器上执行的脚本。 使用脚本的原因有多种，例如在每台订阅服务器上创建登录帐户和架构（对象所有者）。  
  
 可以为每个脚本指定一个文件位置，每次发生快照处理时，快照代理都会将脚本文件复制到当前快照文件夹中。 应用快照时，分发代理或合并代理会在执行任何复制的对象脚本之前执行快照前脚本。 分发代理或合并代理在所有其他复制的对象脚本和数据应用之后，才会执行快照后脚本。 在完成快照应用程序且成功执行脚本文件之后，这些脚本文件将从订阅服务器上的工作目录中删除。  
  
 通过启动 **sqlcmd** 实用工具来执行脚本。 部署脚本之前，请先用 **sqlcmd** 执行该脚本，以确保它可以按预期方式执行。 应用快照之前和之后执行的脚本的内容必须可重复。 例如，如果在脚本中创建了一个表，则应该首先检查该表是否存在，如果存在就执行相应的操作。 脚本必须可重复是因为，如果需要重新初始化已应用了脚本的订阅，则在重新初始化期间应用新的快照时，将再次应用该脚本。  
  
 如果压缩快照文件（采用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] CAB 文件格式压缩），则也会压缩脚本并将其置于此 CAB 文件中。 压缩的快照文件传输到订阅服务器并解压缩到订阅服务器上的工作目录后，将执行所有指示为快照前脚本的脚本。 同样，在应用快照的最后一个步骤中，所有快照后脚本在订阅服务器上解压缩并执行。  

### <a name="execute-a-script"></a>执行脚本 

1.  在“发布属性 -  \<Publication>”对话框的“快照”页上：    
    -   若要指定在应用快照之前执行的脚本，请单击 **“浏览”** 导航到该脚本，或者在 **“应用快照之前执行此脚本”** 文本框中输入该脚本的路径。  
  
        > [!NOTE]  
        >  分发代理或合并代理必须对指定目录具有读取权限。 如果使用的是请求订阅，则必须指定一个共享目录作为通用命名约定 (UNC) 路径，如 \\\computername\scripts\myscript.sql。  
  
    -   若要指定在应用快照之后执行的脚本，请单击 **“浏览”** 导航到该脚本，或者在 **“应用快照之后执行此脚本”** 文本框中输入该脚本的 UNC 路径。   
2.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  


## <a name="see-also"></a>另请参阅  
 [使用快照初始化订阅](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)   
 [通过 FTP 传输快照](../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md)   
 [配置快照属性（复制 Transact-SQL 编程）](../../relational-databases/replication/publish/configure-snapshot-properties-replication-transact-sql-programming.md)     
  
  
