---
description: 标识和访问控制（复制）
title: 标识和访问控制（复制）| Microsoft Docs
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- access controls [SQL Server replication]
- security [SQL Server replication], identity and access control
- authentication [SQL Server replication]
- identity [Replication]
ms.assetid: 4da0e793-1ee4-4f69-a80b-45c6732a238d
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: c44efad10aeade68f42e76d4aa5622807159110f
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88404823"
---
# <a name="identity-and-access-control-replication"></a>标识和访问控制（复制）
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  身份验证是指一个实体（在此上下文中通常是计算机）验证另一实体（也称“主体” ，通常是另一计算机或用户）是否与其自称的身份相符的过程。 授权是给予通过身份验证的主体对资源（例如，文件系统中的文件或数据库中的表）的访问权限的过程。  
  
 复制安全性使用身份验证和授权，来控制对复制数据库对象和涉及复制处理的计算机及代理的访问。 这是通过三种机制实现的：  
  
-   代理安全性  
  
     用复制代理安全模式可以对复制代理运行和建立连接所用的帐户进行精细粒度的控制。 有关代理安全模式的详细信息，请参阅 [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md)。 
  
-   管理角色  
  
     确保使用正确的服务器和数据库角色进行复制的建立、维护和处理。 有关详细信息，请参阅 [Security Role Requirements for Replication](../../../relational-databases/replication/security/security-role-requirements-for-replication.md)。  
  
-   发布访问列表 (PAL)  
  
     通过 PAL 授予对发布的访问权限。 PAL 的功能与 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 的访问控制列表相似。 当有订阅服务器连接到发布服务器或分发服务器并请求访问发布时，将对照 PAL 检查代理传递的身份验证信息。 有关 PAL 的详细信息和最佳做法，请参阅[保护发布服务器](../../../relational-databases/replication/security/secure-the-publisher.md)。  
  
## <a name="filtering-published-data"></a>筛选已发布数据  
 除了使用身份验证和授权来控制对被复制的数据和对象的访问之外，复制还包含两个选项，用来控制订阅服务器上哪些数据可用：列筛选和行筛选。 有关筛选的详细信息，请参阅[筛选已发布数据](../../../relational-databases/replication/publish/filter-published-data.md)。  
  
 定义项目时，可以只发布那些发布所必需的列，而省略那些不必要或包含敏感数据的行。 例如，从 Adventure Works 数据库向现场销售代表发布 **Customer** 表时，可以省略 **AnnualSales** 列，该列可能仅与公司的高层管理者有关。  
  
 通过筛选已发布数据，可以限制对数据的访问，并可指定订阅服务器上的可用数据。 例如，可以筛选 **Customer** 表，以使公司合作伙伴们只收到其 **ShareInfo** 列的值为“yes”的客户的信息。 对于合并复制，如果使用包括 HOST_NAME() 的参数化筛选器，则需要考虑安全问题。 有关详细信息，请参阅 [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)中“使用 HOST_NAME() 筛选”部分。  

## <a name="manage-logins-and-passwords-in-replication"></a>管理复制中的登录名和密码
在配置复制时，为复制代理指定登录名和密码。 配置复制后，可以更改登录名和密码。 有关详细信息，请参阅 [View and Modify Replication Security Settings](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)。 若要更改复制代理所使用的帐户的密码，请执行 [sp_changereplicationserverpasswords (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-changereplicationserverpasswords-transact-sql.md)。  

SQL Server 2014 中支持使用组托管服务帐户 (gMSA)。 有关详细信息，请参阅博客 [托管服务帐户复制和分组](https://repltalk.com/2019/03/26/replication-and-group-managed-service-accounts/)。
  
## <a name="see-also"></a>另请参阅  
 [威胁和漏洞缓解（复制）](../../../relational-databases/replication/security/threat-and-vulnerability-mitigation-replication.md) [复制代理安全模式](../../../relational-databases/replication/security/replication-agent-security-model.md)   
 [复制安全最佳做法](../../../relational-databases/replication/security/replication-security-best-practices.md)   

  
  
