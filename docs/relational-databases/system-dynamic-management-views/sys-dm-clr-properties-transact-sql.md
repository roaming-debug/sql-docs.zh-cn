---
description: sys.dm_clr_properties (Transact-SQL)
title: sys.dm_clr_properties (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_clr_properties
- sys.dm_clr_properties_TSQL
- dm_clr_properties_TSQL
- dm_clr_properties
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_properties dynamic management view
ms.assetid: 220d062f-d117-46e7-a448-06fe48db8163
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fbf92690e35a065afc9aab936e5a8e37ef59f6f6
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100352689"
---
# <a name="sysdm_clr_properties-transact-sql"></a>sys.dm_clr_properties (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  对于与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 公共语言运行时 (CLR) 集成相关的每个属性（包括宿主 CLR 的版本和状态）返回一行。 通过运行 [CREATE assembly](../../t-sql/statements/create-assembly-transact-sql.md)、 [ALTER ASSEMBLY](../../t-sql/statements/alter-assembly-transact-sql.md)或 [DROP assembly](../../t-sql/statements/drop-assembly-transact-sql.md) 语句或通过执行任何 CLR 例程、类型或触发器来初始化托管的 clr。 **Sys.dm_clr_properties** 视图不指定是否已在服务器上启用用户 clr 代码的执行。 用户 CLR 代码的执行通过使用在[CLR enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md)选项设置为1的[sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)存储过程启用。  
  
 **Sys.dm_clr_properties** 视图包含 "**名称**" 和 "**值**" 列。 此视图中的每一行都提供了有关宿主 CLR 的某个属性的详细信息。 使用此视图搜集有关宿主 CLR 的信息，例如 CLR 安装目录、CLR 版本和宿主 CLR 的当前状态。 此视图可以帮助您确定 CLR 集成代码之所以无效是否是因为服务器上的 CLR 安装存在问题。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name |**nvarchar(128)**|属性的名称。|  
|**value**|**nvarchar(128)**|属性的名称。|  
  
## <a name="properties"></a>属性  
 **Directory** 属性指示在服务器上安装 .NET Framework 的目录。 在服务器上可能存在多个 .NET Framework 安装，该属性的值标识了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 正在使用的安装。  
  
 **Version** 属性指示服务器上的 .NET Framework 和宿主 CLR 的版本。  
  
 **Sys.dm_clr_properties** 动态托管视图可为 **state** 属性返回六个不同的值，这反映了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 托管 clr 的状态。 它们分别是：  
  
-   Mscoree 未加载。  
  
-   Mscoree 已加载。  
  
-   带 mscoree 的锁定 CLR 版本。  
  
-   CLR 已初始化。  
  
-   CLR 初始化永久失败。  
  
-   CLR 已停止。  
  
 **未加载 mscoree.dll** ，并且 **加载了 mscoree.dll** ，状态显示了服务器启动时托管 CLR 初始化的进度，并且不太可能出现。  
  
 可能会出现 **具有 mscoree.dll 状态的锁定 clr 版本** ，但未使用托管的 clr，因而尚未对其进行初始化。 当 DDL 语句 (（如 [CREATE ASSEMBLY &#40;transact-sql&#41;](../../t-sql/statements/create-assembly-transact-sql.md)) 或执行托管数据库对象）时，将初始化托管的 CLR。  
  
 **CLR 处于已初始化** 状态指示托管 CLR 已成功初始化。 请注意，这并不能指示是否启用了用户 CLR 代码的执行。 如果先启用用户 CLR 代码的执行，然后使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) 存储过程将其禁用，则状态值仍将为 **CLR 初始化**。  
  
 **Clr 初始化永久失败** 状态指示托管 CLR 初始化失败。 原因可能是内存不足，也可能是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 与 CLR 的宿主握手失败。 在此情况下，将引发错误消息 6512 或 6513。  
  
 **CLR 处于停止状态**，仅在处于关闭 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 过程中时才会出现。  
  
## <a name="remarks"></a>备注  
 此视图的属性和值在的未来版本中可能会更改， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 因为它增强了 CLR 集成功能。  
  
## <a name="permissions"></a>权限  
  
在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="examples"></a>示例  
 以下示例检索有关宿主 CLR 的信息：  
  
```  
SELECT name, value   
FROM sys.dm_clr_properties;  
```  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与公共语言运行时相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  
