---
description: 创建和管理角色分配
title: 创建和管理角色分配 | Microsoft Docs
ms.date: 05/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- removing role assignments
- roles [Reporting Services], assignments
- security [Reporting Services], role assignments
- modifying role assignments
- deleting role assignments
ms.assetid: 086d0987-b43c-4834-8372-e08fb4b432f8
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e1be919afaea2693329ae6fe957fb6fc5ff3357f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015197"
---
# <a name="create-and-manage-role-assignments"></a>创建和管理角色分配

角色分配是确定用户或组权限的安全策略。 权限决定用户或组是否可以访问或修改特定报表服务器项，或执行任务。 角色分配由单个用户帐户名或组帐户名以及一个或多个角色定义组成。

角色分配的作用域为“  项级”或“ 系统级”。

- 项级角色分配会创建，用于报表服务器文件夹层次结构中的特定项或分支。 您需要导航到具体的文件夹或项，才能为其创建角色分配。

- 系统级角色分配向所选用户赋予执行影响整个报表服务器站点的任务的能力。 这些任务包括：
  - 创建共享计划
  - 管理作业
  - 正在处理报表
  - 设置属性

系统级安全性不提供对报表服务器文件夹层次结构中的项的访问权限。

## <a name="creating-an-item-level-role-assignment"></a>创建项级角色分配

在此，可以为需要访问报表服务器的每个用户或组帐户创建一个单独的角色分配。 如果帐户所属的域不是报表服务器所在的域，指定帐户时请包括域名。 指定帐户后，选择一个或多个角色定义。 这些角色定义是可累加的。 对于特定用户或组，其角色分配支持所有定义的所有任务的合集。

若要扩大访问范围，应在文件夹层次结构的上级选择项（如根文件夹“主文件夹”）。 然后，可以创建角色分配来锁定文件夹层次结构的特定区域。

您必须是报表服务器计算机上的本地管理员组的成员，才能创建角色分配。 您可以通过将其他用户分配给  内容管理员角色来委托该责任。

若要创建或管理角色分配，或了解详细信息，请参阅[授予用户对报表服务器的访问权限](../../reporting-services/security/grant-user-access-to-a-report-server.md)
  
## <a name="creating-a-system-level-role-assignment"></a>创建系统级角色分配

系统级和项级角色分配一同进行。 为拥有项级角色分配的每个用户或组创建系统级角色分配。

系统级角色分配包括的权限广泛，但不包括项级角色分配中的权限。

与计算机上的系统权限相比，报告服务器上的系统角色不提供包括所有可能任务的全面权限。 相反，系统级角色分配不过是作用域为报表服务器站点的任务的集合。 系统角色分配确定用户能否查看应用程序属性（如主页的图像或标题）、查看或管理共享计划或者使用报表生成器。

若要创建或管理系统级别的角色分配，或了解详细信息，请参阅[授予用户对报表服务器的访问权限](../../reporting-services/security/grant-user-access-to-a-report-server.md)以及[预定义角色](../../reporting-services/security/role-definitions-predefined-roles.md)。  

## <a name="modifying-a-role-assignment"></a>修改角色分配

您可以随时修改角色分配。 所做的更改将在保存角色分配后生效。 角色分配的更改不会影响用户会话。 如果在用户打开某个报表期间将角色分配修改为拒绝其访问该报表，则用户可以继续为该活动会话使用该报表。

为已经是某个角色分配一部分的组添加用户帐户时，将会出现延迟，然后该用户帐户才能够从更改中访问项。 此延迟由身份验证令牌的 Internet 信息服务 (IIS) 缓存导致。 可以等待这些令牌刷新（通常为 15 分钟），也可以重置 IIS 以立即更新缓存。

一次只能修改一个角色分配。 不能通过执行全局搜索-替换操作来更改角色定义名称、角色分配设置，也不能通过这种方式查找包括特定用户或组的所有角色分配。

## <a name="deleting-a-role-assignment"></a>删除角色分配

若要删除角色分配，请选择要删除的每个角色分配旁边的复选框，然后单击 **“删除”**。 另一种删除角色分配的方法是单击 **“恢复到父级安全设置”**。 当选中此按钮时，该项目的现有角色分配将被删除，并替换为从父项目继承的分配。

## <a name="see-also"></a>另请参阅

[授予用户对报表服务器的访问权限](../../reporting-services/security/grant-user-access-to-a-report-server.md)  
[角色分配](../../reporting-services/security/role-assignments.md)  
[角色定义](../../reporting-services/security/role-definitions.md)  
[预定义角色](../../reporting-services/security/role-definitions-predefined-roles.md)  
[授予对本机模式报表服务器的权限](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)
