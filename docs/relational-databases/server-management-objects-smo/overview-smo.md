---
description: 概述 (SMO)
title: 概述 (SMO) |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
ms.assetid: e988f9e8-6801-41d1-8069-726f487244d5
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8b3a9c15979d162ca345a0d440f7093c4bd15ad9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439830"
---
# <a name="overview-smo"></a>概述 (SMO)
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]管理对象 (SMO) 是设计用于的编程管理的对象 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 可以使用 SMO 生成自定义 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理应用程序。 尽管 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 是用于管理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的使用广泛的强大应用程序，但有时 SMO 应用程序可能对您更为适用。  
  
 例如，可能需要简化控制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理任务的用户应用程序，以满足新用户的需要并且降低培训成本。 您可能需要创建自定义 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库，或创建用于创建和监控索引效率的应用程序。 还可以使用 SMO 应用程序将第三方硬件或软件无缝纳入数据库管理应用程序。  
  
 由于 SMO 与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本兼容，因此，您可以轻松管理多版本环境。  
  
 SMO 中的功能包括：  
  
-   创建缓存对象模型和优化的对象实例。 仅在专门引用时才加载对象。 创建对象时仅部分加载对象属性。 其余对象和属性在直接引用时加载。  
  
-   成批执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 对语句执行批处理以提高网络性能。  
  
-   捕获 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 允许将任意操作捕获到脚本中。 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 使用此功能编写操作脚本，而不立即执行该操作。  
  
-   使用 WMI 提供程序管理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。 可以通过编程方式启动、停止和暂停 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。  
  
-   高级脚本编写。 可以生成 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本以重新创建描述与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上的其他对象之间的关系的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象。  
  
-   使用唯一资源名称 (URN)。 URN 允许您创建 SMO 对象的实例并引用 SMO 对象。  
  
 SMO 还表示为 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中引入的许多功能和组件中的新对象或属性。 这些新增的功能和组件包括：  
  
-   用于在分区方案上存储数据的表和索引分区。 有关详细信息，请参阅 [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md)。  
  
-   用于管理 SOAP 请求的 HTTP 端点。 有关详细信息，请参阅 [实现终结点](../../relational-databases/server-management-objects-smo/tasks/implementing-endpoints.md)。  
  
-   旨在提高并发性的快照隔离和行级版本控制。 有关详细信息，请参阅[使用快照隔离](../../relational-databases/native-client/features/working-with-snapshot-isolation.md)。  
  
-   XML 架构集合、XML 索引和 XML 数据类型提供对 XML 数据的验证和存储。 有关详细信息，请参阅 &#40;SQL Server&#41;和[使用 Xml 架构](../../relational-databases/server-management-objects-smo/tasks/using-xml-schemas.md)的[xml 架构集合](../../relational-databases/xml/xml-schema-collections-sql-server.md)。  
  
-   用于创建数据库只读副本的快照数据库。  
  
-   针对基于消息的通信的 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 支持。 有关详细信息，请参阅 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)。  
  
-   针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库对象的多个名称的同义词支持。 有关详细信息，请参阅 [同义词 &#40;数据库引擎&#41;](../../relational-databases/synonyms/synonyms-database-engine.md)。  
  
-   数据库邮件的管理，允许您在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中创建电子邮件服务器、电子邮件配置文件和电子邮件帐户。 有关详细信息，请参阅[数据库邮件](../../relational-databases/database-mail/database-mail.md)。  
  
-   用于注册连接信息的已注册服务器支持。 有关详细信息，请参阅 [注册服务器](../../ssms/register-servers/register-servers.md)。  
  
-   跟踪和重播 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件。 有关详细信息，请参阅 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)、 [SQL Trace](../../relational-databases/sql-trace/sql-trace.md)、 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)和 [扩展事件](../../relational-databases/extended-events/extended-events.md)。  
  
-   针对用于安全控制的证书和密钥的支持。 有关详细信息，请参阅 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)。  
  
-   用于在出现 DDL 事件时增强功能的 DDL 触发器。 有关详细信息，请参阅 [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md)。  
  
 SMO 命名空间为 <xref:Microsoft.SqlServer.Management.Smo>。 SMO 作为 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 程序集实现。 这意味着在使用 SMO 对象之前，必须先安装 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 版本 2.0 中的公共语言运行时。 SMO 程序集随 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SDK 选件默认安装到全局程序集缓存 (GAC) 中。 这些程序集位于 C:\Program Files\Microsoft SQL Server\130\SDK\Assemblies\. 中。 有关详细信息，请参阅 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 文档。  
  
## <a name="smo-classes"></a>SMO 类  
 SMO 类包括两个类别：实例类和实用工具类。  
  
 **实例类**  
  
 实例类表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象，如服务器、数据库、表、触发器和存储过程。  类用于与  实例建立连接，并且控制发送到它的命令的捕获模式。  
  
 SMO 实例对象所构成的层次结构代表了数据库服务器的层次结构。 顶部为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，其下为数据库，再下面是表、列、触发器等。 如果存在一个父级对多个子级的关系（如某个表具有一个或多个列）是符合逻辑的，那么子级将由对象集合来表示。 否则子级仅由对象来表示。  
  
 **实用工具类**  
  
 实用工具类是为执行特定任务而显式创建的一组对象。 根据功能将它们划分为不同的对象层次结构：  
  
-   传输类。 用于向其他数据库传输架构和数据。  
  
-   备份和还原类。 用于备份和还原数据库。  
  
-   脚本程序类。 用于创建脚本文件，以重新生成对象及其依赖关系。  
  
## <a name="smo-features"></a>SMO 功能  
 **优化性能**  
  
 SMO 体系结构的工作效率非常高，因为对象首先仅部分实例化，并从服务器请求最小属性信息。 直到显式引用对象时才将对象完全实例化。 当请求在最初检索的属性集中不存在的某个属性或者调用的方法需要这样的属性时，才将对象完全实例化。 从部分实例化对象到完全实例化对象的转换对用户是透明的。 此外，从不检索某些占用大量内存空间的属性，除非显式引用该属性。  对象属性的  属性就是这样的例子。 不过，部分实例化确实需要更多的网络往返时间，所以可能对您的应用程序而言不是最佳执行选项。  
  
 您可以对实例化进行控制以适应系统环境。 尽管依赖延迟实例化可能在引用属性时触发许多服务器请求，但它将应用程序所需的内存空间降至最低。  
  
 实例类（表示实际数据库对象的对象）可存在三种级别的实例化， 分别是：最小实例化（在一个块中仅读取尽量少的必需属性）、部分实例化（在一个块中读取占用相对较大的内存空间的所有属性），以及完全实例化。 非实例化和完全实例化是两种传统的实例化状态。 由于部分实例化对象不包含完整对象属性集的值，所以部分实例化提高了效率。 部分实例化是未直接引用的对象的默认状态。 引用这些属性之一时，将生成错误，提示需要将对象完全实例化。  
  
 **捕获执行**  
  
 直接执行是通常的执行方法。 语句一旦引发，就会直接发送到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的某个实例。 捕获执行是直接执行的替代方式。  
  
 捕获执行允许您捕获通常会执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理。 这使 SMO 编程人员得以延迟脚本、存储它以便以后执行，或者为最终用户提供预览。 例如， **create database**、 **create table** 和 **create index** 语句可以在一个批处理中发送，然后作为三个连续的步骤运行。 用户可以通过使用 <xref:Microsoft.SqlServer.Management.Smo.Server.%23ctor%2A> 对象控制此功能。  
  
 **WMI 提供程序**  
  
 WMI 提供程序对象由 SMO 包装。 这为 SMO 编程人员提供了与 SMO 类极为相似的简单对象模型，而无需了解命名空间所表示的编程模型以及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] WMI 提供程序的详细信息。 WMI 提供程序支持您配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务、别名以及客户端和服务器网络库。  
  
 **脚本**  
  
 在 SMO 中，脚本编写已经过增强，并移 **到了脚本编写类** 中。 **脚本编写** 类可以发现依赖关系，了解对象之间的关系，并允许操作依赖关系层次结构。 主要脚本对象是脚本 **编写** 对象。 还有若干支持对象用于处理依赖关系并响应进度或错误事件。  
  
 **脚本编写** 对象支持以下高级脚本编写选项：  
  
-   1 段式简单脚本编写（在一步中创建脚本）  
  
-   高级3阶段脚本编写 (以三个步骤创建脚本;依赖项发现、列表生成、脚本生成)   
  
-   双向依赖关系发现（允许发现依赖关系或依赖项）  
  
-   响应进度事件  
  
-   响应错误事件  
  
 **唯一资源名称**  
  
 使用 SMO 对象库时的一个关键概念是唯一资源名称 (URN)。 URN 使用类似于 XPath 的语法。 XPath 是一种用于指定对象的层次结构路径，该路径中的每个级别都具有限定符和函数。 在 SMO 中，URN 具有两个元素：路径和属性命名（功能有限）。 路径用于指定对象的位置，属性命名支持一定程度的筛选。  
  
 针对数据库的 URN 的示例如下：  
  
```  
/Server/Database[@Name='Adventureworks2012']  
```  
  
 可以通过引用对象的 URN 属性来检索该对象的 URN。 脚本编写对象还使用 Urn 作为参数，将对象引用传递给 **脚本编写** 对象的方法。 此外，可以为 **服务器** 对象的 **GETSMOOBJECT** 方法指定 URN。 用于创建 SMO 对象的实例。  
  
## <a name="sql-server-features-represented-in-smo"></a>SMO 中表示 SQL Server 功能  
 **表和索引分区**  
  
 索引表分区支持您管理跨多个文件组的表和索引中的数据的分布。 此新增功能由 SMO 对象表示。  
  
 **终结点**  
  
 SOAP 和数据库镜像请求通过使用 <xref:Microsoft.SqlServer.Management.Smo.Endpoint> 对象由端点处理。  
  
 **快照隔离/行级版本控制**  
  
 快照隔离级别（行级版本控制）由新的 <xref:Microsoft.SqlServer.Management.Smo.Database> 对象属性表示。  
  
 **XML 架构命名空间、XML 索引和 XML 数据类型**  
  
 XML 架构命名空间在 SMO 中通过对象集合来表示。 XML 索引在 SMO 中由 **索引** 对象属性表示。  
  
 **全文搜索增强功能**  
  
 SMO 中提供了新对象，这些对象表示针对全文搜索的增强。  
  
 **页验证**  
  
 <xref:Microsoft.SqlServer.Management.Smo.DatabaseOptions.PageVerify%2A> 对象表示数据库页验证选项。  
  
 **快照数据库**  
  
 快照数据库是特定时间点的指定数据库的只读副本。 可以通过使用 <xref:Microsoft.SqlServer.Management.Smo.Database.IsDatabaseSnapshot%2A> 对象的 <xref:Microsoft.SqlServer.Management.Smo.Database> 属性指定快照数据库。  
  
 **Service Broker**  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 及其功能由一组对象表示  
  
 **索引增强功能**  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 索引增强功能由 <xref:Microsoft.SqlServer.Management.Smo.Index> 对象中的新增属性表示。  
  
## <a name="see-also"></a>另请参阅  
 [复制管理对象概念](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)  
  
