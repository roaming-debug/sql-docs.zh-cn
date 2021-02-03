---
description: 管理发布访问列表中的登录名
title: 管理发布访问列表中的登录名 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- logins [SQL Server replication], publication access list
- publications [SQL Server replication], publication access lists
- publication access list (PAL)
- PAL (publication access list)
- logins [SQL Server replication], managing
ms.assetid: fceb216b-0b18-4e3b-8ae0-13e35920dcbc
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 526d46149bbafccf8592a20b8a592012e3d6b6aa
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250634"
---
# <a name="manage-logins-in-the-publication-access-list"></a>管理发布访问列表中的登录名
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中管理发布访问列表中的登录名。 对发布的访问是由发布访问列表 (PAL) 控制的。 可以在 PAL 中添加和删除登录名和组。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [先决条件](#Prerequisites)  
  
-   **管理发布访问列表中的登录名，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   在将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名添加到 PAL 前，必须将该登录名与发布数据库中的数据库用户关联。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可使用“发布属性 - \<Publication>”对话框的“发布访问列表”页面上的发布访问列表 (PAL) 来管理登录名 。 有关如何访问此对话框的详细信息，请参阅[查看和修改发布属性](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)。  
  
#### <a name="to-manage-logins-in-the-pal"></a>管理 PAL 中的登录名  
  
1.  在“发布属性 - \<Publication>”对话框的“发布访问列表”页面上，使用“添加”、“删除”和“全部删除”按钮在 PAL 中添加和删除登录名和组    。 不要从 PAL 中删除 **distributor_admin** 。 复制将使用此帐户。  
  
    > [!NOTE]  
    >  如果使用远程分发服务器，则 PAL 中的帐户必须在发布服务器和分发服务器中都可用。 帐户必须是在这两个服务器中定义的域帐户或本地帐户。 与这两个登录名关联的密码必须相同。  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-view-groups-and-logins-that-belong-to-the-pal"></a>查看属于 PAL 的组和登录名  
  
1.  在发布服务器上，对发布数据库执行 [sp_help_publication_access](../../../relational-databases/system-stored-procedures/sp-help-publication-access-transact-sql.md)。 为 `@publication` 指定发布名称。 这将显示有关 PAL 中组和登录名的信息。  
  
#### <a name="to-add-groups-and-logins-to-the-pal"></a>将组和登录名添加到 PAL  
  
1.  在发布服务器上，对发布数据库执行 [sp_grant_publication_access](../../../relational-databases/system-stored-procedures/sp-grant-publication-access-transact-sql.md)。 为 `@publication` 指定发布名称；为 `@login` 指定要添加的登录名或组名。  
  
#### <a name="to-remove-groups-and-logins-from-the-pal"></a>从 PAL 删除组和登录名  
  
1.  在发布服务器上，对发布数据库执行 [sp_revoke_publication_access](../../../relational-databases/system-stored-procedures/sp-revoke-publication-access-transact-sql.md)。 为 `@publication` 指定发布名称；为 `@login` 指定要删除的登录名或组名。  
  
## <a name="see-also"></a>另请参阅  
 [管理发布访问列表中的登录名](../../../relational-databases/replication/security/manage-logins-in-the-publication-access-list.md)   
 [复制代理安全模式](../../../relational-databases/replication/security/replication-agent-security-model.md)   
 [保护复制拓扑](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [保护发布服务器](../../../relational-databases/replication/security/secure-the-publisher.md)  
  
  
