---
description: 导入基于策略的管理策略
title: 导入基于策略的管理策略 | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, import policy
ms.assetid: 850b7ef9-d2b7-4754-bf04-7cb419ffb776
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d843da0059418602778c0b0b0b2de6f954a55e43
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048970"
---
# <a name="import-a-policy-based-management-policy"></a>导入基于策略的管理策略
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中导入基于策略的管理策略实例。  
  
## <a name="permissions"></a>权限
 要求具有 msdb 数据库中 PolicyAdministratorRole 角色的成员身份。

  
##  <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio  
  
### <a name="to-import-a-policy-instance"></a>导入策略实例  
  
1.  在“对象资源管理器” 中，单击加号以展开新导入的策略实例要存储到的服务器。  
  
2.  单击加号以便展开 **“管理”** 文件夹。  
  
3.  单击加号以便展开 **“策略管理”**。  
  
4.  右键单击“策略”文件夹，然后选择“导入策略”。  
  
5.  在“导入”对话框中，键入该文件的路径和名称；或者使用“浏览”按钮 (**...**) 查找包含策略的 XML 文件，然后选择该文件。 有关 **“导入”** 对话框中可用选项的详细信息，请参阅 [Import Policies Dialog Box](../../relational-databases/policy-based-management/import-policies-dialog-box.md)。  
  
6.  完成后，单击 **“确定”** 。  


## <a name="example-policies"></a>示例策略
 [SQL Server 示例代码存储库](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/epm-framework/sample-policies)中提供了示例策略。 可导入这些策略，并将其用作你自己基于策略的管理策略的基础。
