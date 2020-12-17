---
description: View Job Step Information
title: View Job Step Information
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- displaying job step information
- jobs [SQL Server Agent], viewing
- SQL Server Agent jobs, viewing
- viewing job step information
ms.assetid: e3f06492-dc86-4e06-b186-ea58aff6d591
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: c3141dfe74272758450b33a1302be4fe339eeb82
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480658"
---
# <a name="view-job-step-information"></a>View Job Step Information
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题说明如何在“作业步骤属性”对话框中查看作业步骤的详细信息。 它还包括有关查看作业步骤输出的信息。  
  
-   **开始之前：**  
  
    [限制和局限](#Restrictions)  
  
    [安全性](#Security)  
  
-   **若要查看作业步骤信息，可使用：**  
  
    [SQL Server Management Studio](#SSMS)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>限制和局限  
如果已将作业步骤配置为将其输出写入到表或文件，并且作业已经至少运行过一次，则可以在 **“作业步骤属性”** 对话框的 **“高级”** 页上查看其输出。 作业或作业步骤删除时，输出日志也将自动删除。  
  
### <a name="security"></a><a name="Security"></a>安全性  
  
#### <a name="permissions"></a><a name="Permissions"></a>权限  
您只能查看自己拥有的那些作业，除非您是 **sysadmin** 固定服务器角色的成员。 此角色的成员可以查看所有作业和作业步骤的详细信息。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>使用 SQL Server Management Studio  
  
#### <a name="to-view-job-step-information"></a>查看作业步骤信息  
  
1.  在“对象资源管理器”中，连接到 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)] 的实例，然后展开此实例。  
  
2.  依次展开“SQL Server 代理”和“作业”，右键单击包含要查看的作业步骤的作业，再单击“属性”。  
  
3.  在 **“作业属性”** 对话框中，单击 **“步骤”** 页。  
  
4.  单击要查看的作业步骤，再单击 **“编辑”**。  
  
5.  在 **“作业步骤属性”** 对话框的 **“常规”** 页上，可以查看作业步骤的类型和用途。  
  
6.  单击 **“高级”** 页以查看作业步骤成功或失败时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理所执行的操作、该作业步骤应尝试的次数、该作业步骤输出的位置以及运行该作业步骤的用户。  
  
#### <a name="to-view-job-step-output"></a>查看作业步骤输出  
  
1.  在 **“作业步骤属性”** 对话框中，单击 **“高级”** 页。  
  
2.  根据所连接的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的版本，可以查看作业步骤输出文件或表，如下所示：  
  
    -   连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或更高版本时，只有选中 **“记录到表”** ，才能单击 **“查看”** 。 在这种情况下，作业步骤输出将写入 **msdb** 数据库的 **sysjobstepslogs** 表中。  
  
    -   在作业步骤输出写入到文件时，禁用 **“查看”** 按钮。 若要查看作业步骤输出文件，请使用记事本。  
