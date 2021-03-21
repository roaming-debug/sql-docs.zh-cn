---
title: “权限”或“安全对象”页 | Microsoft Docs
description: 使用“权限”页或“安全对象”页在 SQL Server 中查看或设置安全对象的权限。
ms.custom: ''
ms.date: 01/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.common.permissions.f1
- sql13.swb.SecurableAndEffectPermissions.f1
- sql13.swb.common.columnperm.f1
- sql13.swb.availabilitygroupproperties.permission.f1
- sql13.swb.SecurableAndEffectivePermission.f1
ms.assetid: b3bf077a-bec2-4161-ac0c-460586199906
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bc8f417dba29049a5845fe450eea82c8f753a5a1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748037"
---
# <a name="permissions-or-securables-page"></a>“权限”或“安全对象”页
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  使用 **“权限”** 或 **“安全对象”** 页可以查看或设置安全对象的权限。 此页可以从多个位置打开。 根据此页的打开方式以及其中包含的内容，此页中的内容可能会稍有不同。 此页在打开时，其顶部网格可能会进行填充，也可能为空。 若要在上部网格中添加项目，请单击 **“搜索”** 。 在上部网格中，选择一个项目，然后在 **“显式”** 选项卡上设置相应的权限。若要查看聚合权限，请使用 **“有效”** 选项卡。  
  
 若要了解安全对象和主体的可能组合，请参阅 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md) 主题中特定于安全对象的语法链接。 有关详细信息，请参阅 [Securables](../../relational-databases/security/securables.md)。  
  
## <a name="page-header"></a>页眉  
 **“权限”** 或 **“安全对象”** 页的页眉会根据安全对象或主体的不同而变化。 其中显示了与项有关的信息，如项名称。  
  
## <a name="upper-grid"></a>上部网格  
 上部网格中包含一个或多个可以设置权限的项。 该对话框提供了一个 **“搜索”** 按钮，使用该按钮可以选择要添加到上部网格中的对象或主体。 该网格的名称可能显示为 **“安全对象”** 或者一种或多种类型的安全对象或主体。 上部网格中显示的列会根据主体或安全对象的不同而变化。  
  
 **名称**  
 添加到网格中的每个主体或安全对象的名称。  
  
 类型  
 描述每个项目的类型。  
  
## <a name="explicit-tab"></a>“显式”选项卡  
 **“显式”** 选项卡列出了上部网格中所选安全对象的可能权限。 若要配置权限，请选中或清除“授予”（或“允许”）、“具有授予权限”和“拒绝”复选框   。 对于所有显式权限，全部选项均不可用。  
  
 **权限**  
 权限的名称。  
  
 **授权者**  
 授予该权限的主体。  
  
 **授予**  
 选中该选项可以将此权限授予该登录名。 清除该选项将撤消此权限。  
  
 **具有授予权限**  
 反映所列权限的 WITH GRANT 选项的状态。 此框是只读的。 若要应用此权限，请使用 [GRANT](../../t-sql/statements/grant-transact-sql.md) 语句。  
  
 **拒绝**  
 选中该选项可以拒绝该登录名具有该权限。 清除该选项将撤消此权限。  
  
 **列权限**  
 对于包含列的对象（如表、视图或表值函数），单击“列权限”按钮将打开“列权限”对话框 。 在该对话框中，可以针对表或视图中的各列设置 **“授予”** 、 **“允许”** 或 **“拒绝”** 权限。 此选项无法用于所有的对象类型或权限。  
  
## <a name="effective-tab"></a>“有效”选项卡  
 主体所拥有的与安全对象相关的权限可能来自为多个不同的主体设置的权限。 例如，对于某个登录名，可以为其单独授予权限，也可以将其作为组的成员授予权限。 **“有效”** 选项卡中显示从组或角色成员身份接收的权限与显式权限的组合结果。 将对授予权限进行聚合， 拒绝权限会覆盖所有的授予权限。  
  
 **权限**  
 权限的名称。  
  
 **列**  
 受到该权限影响的列的名称。  
  
## <a name="see-also"></a>另请参阅  
 [数据库级别的角色](../../relational-databases/security/authentication-access/database-level-roles.md)   
 [SQL Server 数据库引擎和 Azure SQL Database 的安全中心](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
