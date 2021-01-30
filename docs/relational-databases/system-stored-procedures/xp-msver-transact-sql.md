---
description: xp_msver (Transact-SQL)
title: xp_msver (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_msver_TSQL
- xp_msver
dev_langs:
- TSQL
helpviewer_keywords:
- xp_msver
ms.assetid: 9264cf8c-92ba-45ad-b2d6-15d26d805a16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8a23619f5a1bf12ddcde718fc95db484f6d3cf6e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99123843"
---
# <a name="xp_msver-transact-sql"></a>xp_msver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关的版本信息 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 **xp_msver** 还会返回有关服务器的实际内部版本号和服务器环境的信息。 **Xp_msver** 返回的信息可用于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句、批处理、存储过程等，从而增强了与平台无关的代码的逻辑。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
xp_msver [ optname ]  
```  
  
## <a name="arguments"></a>参数  
 *optname*  
 是选项名，可以是下列值之一。  
  
|选项/列名|说明|  
|-------------------------|-----------------|  
|ProductName|产品名称;例如， [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|**ProductVersion**|产品版本。|  
|**语言**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的语言版本。|  
|**平台**|运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机的操作系统名称、制造商名称以及芯片系列名称。|  
|**注释**|有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的杂项信息。|  
|**CompanyName**|生产 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的公司名称，例如，[!INCLUDE[msCoName](../../includes/msconame-md.md)] Corporation。|  
|**FileDescription**|操作系统。|  
|**FileVersion**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可执行文件的版本。|  
|**InternalName**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内部名称，例如，SQLSERVR。|  
|**LegalCopyright**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所需的合法版权信息。例如，Copyright© [!INCLUDE[msCoName](../../includes/msconame-md.md)] Corp. 1988-2005。|  
|**LegalTrademarks**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所需的合法商标信息。 例如，[!INCLUDE[msCoName](../../includes/msconame-md.md)] 是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Corporation 的注册商标。|  
|**OriginalFilename**|启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时执行的文件名。例如，Sqlservr.exe。|  
|**PrivateBuild**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**SpecialBuild**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**WindowsVersion**|运行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 的计算机上所安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows 的版本。|  
|**ProcessorCount**|运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机中的处理器数目。|  
|**ProcessorActiveMask**|指示运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机中安装的、可由 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 启动并使用的处理器。|  
|**ProcessorType**|处理器类型。 类似于 **平台**。|  
|**PhysicalMemory**|运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机上安装的 RAM 的容量 (MB)。|  
|**产品 ID**|产品 ID (PID) 号。 该参数在安装时指定。 该号码印在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 原始光盘盒的不干胶标签上。|  
  
## <a name="return-code-values"></a>返回代码值  
 1 (成功)   
  
## <a name="result-sets"></a>结果集  
 **xp_msver** 没有任何参数的情况下，将返回一个由四列组成的结果集，其中列出了所有选项值。 **xp_msver**（对于任何参数），将返回四列结果集，其中包含该选项的值。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [System Functions (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [&#40;Transact-sql&#41;的常规扩展存储过程 ](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [@@VERSION (Transact-SQL)](../../t-sql/functions/version-transact-sql-configuration-functions.md)  
  
  
