---
description: Define Transact-SQL Job Step Options
title: Define Transact-SQL Job Step Options
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL job step
- job steps [Transact-SQL]
- SQL Server Agent jobs, Transact-SQL step
ms.assetid: b2a47057-f6fb-432b-a7b6-5d61f33a5d9c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 6d129a33d373bf85c1773a65851d413f0503bcae
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234166"
---
# <a name="define-transact-sql-job-step-options"></a>Define Transact-SQL Job Step Options
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍了如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 SQL Server 管理对象在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中定义 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理 [!INCLUDE[tsql](../../includes/tsql-md.md)] 作业步骤的选项。  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>开始之前  
  
### <a name="security"></a><a name="Security"></a>安全性  
有关详细信息，请参阅[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>使用 SQL Server Management Studio  
  
#### <a name="to-define-transact-sql-job-step-options"></a>定义 Transact-SQL 作业步骤选项  
  
1.  在 **对象资源管理器** 中，展开 **“SQL Server 代理”**，展开 **“作业”**，右键单击要编辑的作业，然后单击 **“属性”**。  
  
2.  单击 **“步骤”** 页，单击一个作业步骤，再单击 **“编辑”**。  
  
3.  在 **“作业步骤属性”** 对话框上，确认作业类型为 **“Transact-SQL 脚本(TSQL)”**，然后选择 **“高级”** 页。  
  
4.  如果作业成功，请从 **“成功时要执行的操作”** 列表中进行选择，指定要采取的操作。  
  
5.  在 **“重试次数”** 框中输入 0 到 9999 之间的一个数字，指定重试的次数。  
  
6.  在 **“重试间隔”** 框中输入 0 到 9999 的数字，指定重试的时间间隔。  
  
7.  如果作业失败，请从 **“失败时要执行的操作”** 列表中进行选择，指定要采取的操作。  
  
8.  如果作业是一个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本，则可以从下列选项中进行选择：  
  
    -   输入 **输出文件** 的名称。 默认情况下，每次执行作业步骤时都覆盖该文件。 如果不希望覆盖输出文件，请选中 **“将输出追加到现有文件”**。 只有 **sysadmin** 固定服务器角色的成员才能设置此选项。 请注意， [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 不允许用户查看文件系统中的任意文件。因此您不能使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 查看写入文件系统的作业步骤日志。  
  
    -   如果希望将作业步骤记录到一个数据库表中，请选中 **“记录到表”** 。 默认情况下，每次执行作业步骤时都覆盖该表的内容。 如果不希望覆盖表内容，则请选中 **“将输出追加到表中的现有条目”**。 在执行作业步骤后，您可以通过单击 **“查看”** 来查看此表的内容。  
  
    -   如果希望将输出包括在步骤的历史记录中，请选中 **“在历史记录中包含步骤输出”** 。 仅当没有错误时，才会显示输出结果。 此外，输出可能会被截断。  
  
9. 如果您是 **sysadmin** 固定服务器角色的成员，并且希望以其他 SQL 登录身份运行此作业步骤，请从 **“作为以下用户运行”** 列表中选择 SQL 登录名。  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>使用 SQL Server 管理对象  
**定义 Transact-SQL 作业步骤选项**  
  
通过使用所选编程语言（如 Visual Basic、Visual C# 或 PowerShell）来使用 **JobStep** 类。  
