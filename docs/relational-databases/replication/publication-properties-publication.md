---
title: 发布属性 - 对话框
description: 介绍 SQL Server Management Studio (SSMS) 中的“发布属性”对话框中发现的页面。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.pubproperties.agentsecurity.f1
- sql13.rep.newpubwizard.pubproperties.agentsecurity.f1
- sql13.rep.newpubwizard.pubproperties.filterrows.f1
- sql13.rep.newpubwizard.pubproperties.internetsynchronization.f1
- sql13.rep.newpubwizard.pubproperties.general.f1
- sql13.rep.newpubwizard.pubproperties.publicationaccesslist.f1
- sql13.rep.newpubwizard.pubproperties.snapshotformat.f1
helpviewer_keywords:
- Publication Properties dialog box
ms.assetid: 66e845e9-1308-4288-9110-ad2f22f1fc58
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: fa48c38c00732759cf49c8fb694a54f8bc210971
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483018"
---
# <a name="sql-server-replication-publication-properties--dialog-box"></a>SQL Server 复制“发布属性”对话框
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

此页介绍在“发布属性”对话框中找到的页面。 

## <a name="general"></a>常规
 **“发布属性”** 对话框的 **“常规”** 页包含发布的基本信息，包括名称、说明和订阅过期策略。  
  
### <a name="options"></a>选项  
 **名称**  
 发布的名称（只读）。  
  
 **Database**  
 发布数据库的名称（只读）。  
  
 **说明**  
 发布的说明。  
  
 类型  
 发布的类型（只读）。  
  
 **订阅过期**  
 选择以下订阅过期选项之一：“订阅永不过期”或带有明确时间段（“间隔”）的“订阅过期”  。  
  
 对于快照和事务发布， [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议您接受默认选项 **“订阅永不过期”** 。  
  
 对于合并复制， [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议您接受默认选项 **“订阅过期”** ，并设置尽可能低的 **“间隔”** 。 元数据的存储量会随着订阅过期时间的增加而增加，这会影响性能。 这需要在要求订阅服务器在更长的时间内保持断开或不同步与存储和处理大量元数据所带来的性能问题之间取得平衡。  
  
 有关详细信息，请参阅 [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md)。  
  
 **兼容性级别**  
 仅限 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本；仅限合并发布。 选择与此发布同步的订阅服务器所需的最低 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本。 有多个与确定兼容级别有关的规则。  

## <a name="filter-rows"></a>筛选行
  可以使用 **“发布属性”** 对话框的 **“筛选行”** 页，执行添加、编辑或删除操作：  
  
-   将静态行筛选器应用于快照发布、事务发布和合并发布中的表项目。   
-   将参数化行筛选器应用于合并发布中的表项目。    
-   使用联接筛选器将合并表项目的筛选器扩展到相关表项目。 有关筛选选项的详细信息，请参阅[筛选已发布数据](../../relational-databases/replication/publish/filter-published-data.md)。  
  
> [!NOTE]  
>  添加、编辑或删除筛选器需要新的发布快照，并且需要重新初始化所有订阅。  
  
为了获得最佳的应用程序性能并减少所需的远程存储量，或者要限定某些数据仅供特定的订阅服务器使用，您应该只发布所需数据。 发布中既可以包含未筛选的表，也可以包含已筛选的表。 例如，可以包含公司产品的完整（未筛选）表，然后使用行筛选器提供一个仅包含特定区域客户的筛选表。 通过筛选已发布数据，可以：  
  
-   最大程度地减少通过网络发送的数据量。    
-   减少订阅服务器上需要的存储空间量。    
-   根据各个订阅服务器的要求，自定义发布和应用程序。    
-   由于可以将不同的数据分区发送到不同的订阅服务器（没有两个订阅服务器会同时更新相同的数据值），因此可以避免或减少订阅服务器更新数据时的冲突。    
-   避免传输敏感数据。 行筛选器和列筛选器可以用于限制订阅服务器对数据的访问。 对于合并复制，如果使用包括 HOST_NAME() 的参数化筛选器，则需要考虑安全问题。 有关详细信息，请参阅 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中的“使用 HOST_NAME() 进行筛选”部分。  
  
### <a name="options"></a>选项  
 **筛选的表**  
 此窗格使用您向发布中的表项目添加的筛选器进行填充。 带行筛选器的表在窗格中显示为顶级节点。 对于合并发布，筛选操作通过联接筛选器扩展到的表显示为子节点。  
  
 **添加**  
 单击 **“添加”** 可以启动一个用于对表项目进行筛选的对话框。 对于快照发布或事务发布，单击 **“添加”** 将立即启动对话框。 对于合并发布，单击“添加”将会显示三个选项 ：“添加筛选器”、“添加联接以扩展所选筛选器”和“自动生成筛选器”  。  
  
-   选择 **“添加筛选器”** 将启动 **“添加筛选器”** 对话框。 使用此对话框可以将行筛选器应用于表项目。 例如，在 **“添加筛选器”** 对话框中，可以指定在将包含客户数据的表复制到订阅服务器时，该表应只包含法国客户的相关数据。  
  
-   选择 **“添加联接以扩展所选筛选器”** 将启动 **“添加联接”** 对话框。 使用 **“添加联接”** 对话框，可以对行筛选器进行扩展，这样就可以筛选与行筛选器所在表相关的表中的数据。 例如，如果筛选一个客户表以便它仅包含法国客户的相关数据，并且还有一个与客户订单相关的表，则可以在两个表之间定义一个联接，以便该订单表仅包括来自法国客户的订单。  
  
    > [!NOTE]  
    >  只有在筛选器窗格中选择了联接的基表后，此选项才可用。  
  
-   选择 **“自动生成筛选器”** 将启动 **“生成筛选器”** 对话框。 使用此对话框可以为合并发布中的某个表定义行筛选器，复制会自动将该行筛选器扩展到通过外键关系相关联的其他表。 例如，一个发布可以包括三个表：客户表、订单表（带有指向客户表的外键）和订单详细信息表（带有指向订单表的外键）。 为客户表定义行筛选器后，复制会将该行筛选器扩展到其他表。  
  
    > [!NOTE]  
    >  当通过复制自动生成筛选器时，会删除发布中的所有现有筛选器。 若要同时包括自动生成的筛选器和手动指定的筛选器，请首先生成筛选器。 对于每个发布，只能指定一组自动生成的筛选器。  
  
 **编辑**  
 在筛选器窗格中选择一个行筛选器或联接筛选器，然后单击 **“编辑”** 以启动 **“编辑筛选器”** 或 **“编辑联接”** 对话框。  
  
 **删除**  
 在筛选器窗格中选择一个行筛选器或联接筛选器，然后单击 **“删除”** 以删除该筛选器。  
  
 **查找表**  
 仅限合并发布。 单击 **“查找表”** 可以在复杂的筛选器树中查找表。 在关系复杂的数据库中，一个表可以联接到多个表，因此可能出现在筛选器树中的多个位置。  
  
 实际的表只显示在树中的一个位置，该表在其他位置使用快捷方式来表示。 表的快捷方式只是对该表的引用；它不显示该表的子节点。 快捷方式节点标记有快捷方式箭头，展开该节点将会显示文本“单击‘查找表’可查看 \<tablename> 表”。  
  
 在窗格中选择快捷方式节点并单击 **“查找表”** 。窗格将展开并突出显示该表。 如果单击 **“查找表”** 而没有选定快捷方式节点，将会启动 **“查找表”** 对话框。  
  
 **筛选器**  
 包含筛选器窗格中选定筛选器的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 定义。  

## <a name="publication-access-list"></a>发布访问列表
  可以使用 **“发布属性”** 对话框的 **“发布访问列表”** 页，在发布访问列表 (PAL) 中添加和删除登录名、帐户和组。 PAL 是用于保护发布服务器的主要机制。 创建发布时，复制将为该发布创建 PAL。 PAL 的功能类似于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 访问控制列表，PAL 包含被授予该发布的访问权的登录名、帐户和组的列表。  
  
 当订阅服务器连接到发布服务器或分发服务器并请求访问发布时，订阅服务器的登录名将与 PAL 中的身份验证信息进行比较。 这样，通过防止客户端工具使用发布服务器和分发服务器登录名在发布服务器上直接进行修改，从而为发布服务器提供额外的安全性。 有关详细信息，请参阅[保护发布服务器](../../relational-databases/replication/security/secure-the-publisher.md)。  
  
### <a name="options"></a>选项  
 **添加**  
 将新项添加到列表。 只能添加那些在发布服务器和分发服务器上均已定义的登录名、帐户或组名称。 如果使用域帐户或在两个服务器上都已创建本地帐户，则这些信息在两个服务器上均已定义。  
  
 **删除**  
 从列表中删除所选项。  
  
 **全部删除**  
 从列表中删除所有项。  

## <a name="ftp-snapshot-and-internet"></a>FTP 快照和 Internet

-   设置通过文件传输协议 (FTP) 传递快照的属性。 有关详细信息，请参阅[通过 FTP 传递快照](../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md)。 若要使用 FTP 传递快照，则必须设置 FTP 服务器。 有关详细信息，请参阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 文档。  
  
    > [!NOTE]  
    >  对任意 FTP 设置的更改都要求生成新的快照。  
  
-   设置 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本上合并复制的 Web 同步的属性，Web 同步允许通过 HTTPS（安全超文本传输协议）来同步订阅。 若要使用 Web 同步，则必须配置 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet 信息服务 (IIS) 服务器。 有关详细信息，请参阅 [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md)。  
  
### <a name="options"></a>选项  
 **通过 FTP 访问快照文件**  
 若要允许订阅服务器使用 FTP 传递快照，请选择 **“允许订阅服务器使用 FTP (文件传输协议)下载快照文件”** ，并指定 **“FTP 服务器名称”** 、 **“端口号”** 、 **“从 FTP 根文件夹开始的路径”** 、 **“登录名”** 和 **“密码”** 。  
  
 此选项允许订阅服务器使用 FTP 检索快照文件，但不要求订阅服务器进行此操作。 如果选择此选项，新建订阅向导将默认为允许订阅服务器通过 FTP 检索快照文件。 若要更改该设置，请使用 **“订阅属性”** 对话框。 如果允许订阅服务器通过 FTP 访问快照文件，请将 FTP 文件夹指定为 **“发布属性”** 对话框的 **“快照”** 页上快照文件的位置。 这会导致在生成新快照时快照代理将自动更新 FTP 文件夹中的文件。 如果未将快照文件的位置设置为 FTP 文件夹，则在生成新快照时必须手动更新文件。 有关详细信息，请参阅[通过 FTP 传递快照](../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md)。  
  
 **Web 同步**  
 仅限合并复制。 若要允许合并订阅服务器使用 Web 同步，请选择 **“允许订阅服务器通过连接到 Web 服务器进行同步”** ，并指定 Web 服务器地址。 Web 服务器必须使用传输层安全性 (TLS)（旧称为“安全套接字层 (SSL)”），且 Web 地址必须为完全限定的地址（如 `https://server.domain.com/synchronize`）。 有关详细信息，请参阅 [Configure Web Synchronization](../../relational-databases/replication/configure-web-synchronization.md)。  


## <a name="agent-security"></a>代理安全性
  可以使用 **“发布属性”** 对话框的 **“代理安全性”** 页，访问运行以下代理的帐户的设置以及连接到复制拓扑中的计算机：  
  
-   所有发布的快照代理。  
  
-   所有事务发布的日志读取器代理。 对于使用事务复制发布的每个数据库，都有一个日志读取器代理。 更改日志读取器代理的设置会影响数据库中的所有事务发布。  
  
-   允许排队更新订阅的事务发布的队列读取器代理。 每个分发数据库都有一个队列读取器代理。 更改队列读取器代理的设置会影响所有具有排队更新订阅（使用相同的分发数据库）的事务发布。 有关队列读取器代理安全设置的详细信息，请参阅[查看和修改复制安全设置](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)。  
  
 有关每个代理所需安全设置和权限的详细信息，请参阅 [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
### <a name="options"></a>选项  
 **“安全设置”** 或 **“创建代理”**  
 如果已创建代理作业，那么请单击 **“安全设置”** 以访问一个对话框，在其中可以更改代理安全设置。 如果未创建代理作业，请单击 **“创建代理”** 来创建代理，并指定安全设置。  

## <a name="data-partitions"></a>数据分区
数据分区  
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]  
  可以使用 **“发布属性”** 对话框的 **“数据分区”** 页，定义使用参数化筛选的合并发布的数据分区。 在定义分区后，您随后还可以生成这些分区的快照，为基于订阅服务器的连接属性（登录名和/或计算机名称）的不同订阅服务器提供不同的初始数据集。 如果订阅服务器在第一次同步时对其分区没有可用的快照，您还可以选择允许订阅服务器请求快照的传递和生成。 有关详细信息，请参阅 [为包含参数化筛选器的合并发布创建快照](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。  
  
### <a name="options"></a>选项  
 **添加**  
 单击 **“添加”** 可以定义分区。 在 **“添加数据分区”** 对话框中，为 **HOST_NAME()** 和/或 **SUSER_SNAME()** 指定值，再定义计划以刷新快照即可。  
  
 **编辑**  
 选择网格中的现有分区，再单击 **“编辑”** 可以编辑相应的分区。  
  
 **删除**  
 选择网格中的现有分区，再单击 **“删除”** 可以删除相应的分区。  
  
 **立即生成所选快照**  
 选择网格中的一个或多个分区，再单击 **“立即生成所选快照”** 可以生成这些分区的快照。  
  
 **清除现有快照**  
 选择网格中的一个或多个分区，再单击 **“清除现有快照”** 可以清除这些分区的快照。  
  
 **在新订阅服务器尝试同步时，根据需要自动定义分区并生成快照**  
 如果希望允许订阅服务器请求快照的生成和应用程序，请选择此选项。 如果订阅服务器在第一次同步时对其分区没有可用的快照，则订阅服务器可能要求设置此选项。  

## <a name="snapshot"></a>快照
快照  
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]  
  可以使用 **“发布属性”** 对话框的 **“快照”** 页设置快照格式、快照文件夹位置以及应用快照前后运行的脚本。 快照文件夹必须指定为共享文件夹，并且对于将文件读/写到快照文件夹的代理有足够的权限。 有关正确保护文件夹的详细信息，请参阅[保护快照文件夹](../../relational-databases/replication/security/secure-the-snapshot-folder.md)。  
  
> [!NOTE]  
>  若要进行更改，则需要发布的新快照。 有关详细信息，请参阅[更改发布和项目属性](../../relational-databases/replication/publish/change-publication-and-article-properties.md)。  
  
### <a name="options"></a>选项  
 **快照格式**  
 选择快照格式的本机模式或字符模式。  
  
-   如果所有订阅服务器都是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例，而不是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssEW](../../includes/ssew-md.md)] 的实例，请选择“本机 SQL Server - 所有订阅服务器都必须是运行 SQL Server 的服务器”。 本机快照格式可以提供最佳性能。    
-   如果有任何订阅服务器正在运行 **，或者为非** 订阅服务器，请选择 [!INCLUDE[ssEW](../../includes/ssew-md.md)] “字符 - 如果发布服务器或订阅服务器没有运行 SQL Server，则需要此项”[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。    
 **快照文件的位置**  
 选择要存储快照文件的位置。 可以将快照文件存储在默认位置，也可以将其存储在默认位置之外的其他位置。 可以压缩存储在其他位置的文件。  
  
-   选择 **“将文件放入默认文件夹”** 可以使用发布服务器的默认快照文件夹。 此对话框中的快照文件夹位置是只读的，因为对于发布服务器来说，只能在 **“分发服务器属性”** 对话框中更改该位置。 有关详细信息，请参阅[修改快照属性](../../relational-databases/replication/snapshot-options.md)。    
-   选择 **“将文件放入下列文件夹”** 可以指定默认位置之外的其他位置。 在文本框中输入路径，或单击 **“浏览”** 定位到某个位置。 选择 **“压缩此文件夹中的快照文件”** 可以压缩其他快照位置中的文件。 其他位置可以为其他服务器、网络驱动器或诸如 CD-ROM、可移动磁盘等可移动介质上。 有关详细信息，请参阅[修改快照属性](../../relational-databases/replication/snapshot-options.md)。  
  
 **运行其他脚本**  
 指定在订阅服务器上应用快照之前和之后要执行的脚本。 如果 **“快照格式”** 为 **“字符”** ，则不能指定脚本。  
  
 脚本是可选的，不过，脚本提供了一种便捷方法，可以在订阅服务器上执行命令和应用管理更改。 有关执行脚本的详细信息，请参阅[在应用快照之前和之后执行脚本](../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied)。  
  
-   在 **“应用快照之前执行此脚本”** 文本框中输入路径，或单击 **“浏览”** 指定脚本的位置。    
-   在 **“应用快照之后执行此脚本”** 文本框中输入路径，或单击 **“浏览”** 指定脚本的位置。 
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [查看和修改发布属性](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   

  
  
