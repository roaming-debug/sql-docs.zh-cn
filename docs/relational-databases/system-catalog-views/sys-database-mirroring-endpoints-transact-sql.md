---
description: sys.database_mirroring_endpoints (Transact-SQL)
title: sys.database_mirroring_endpoints (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.database_mirroring_endpoints_TSQL
- database_mirroring_endpoints
- sys.database_mirroring_endpoints
- database_mirroring_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], endpoint
- HADR [SQL Server], endpoint
- database mirroring [SQL Server], catalog views
- sys.database_mirroring_endpoints catalog view
ms.assetid: f2285199-97ad-473c-a52d-270044dd862b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a402c209d94c74a7709462a6f4db64ed73fbb318
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464580"
---
# <a name="sysdatabase_mirroring_endpoints-transact-sql"></a>sys.database_mirroring_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的数据库镜像端点对应一行。  
  
> [!NOTE]  
>  数据库镜像端点既支持数据库镜像伙伴之间的会话，也支持 Always On 可用性组的主副本与其辅助副本之间的见证服务器和会话。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|-|从 **sys.databases** 中继承列 (有关详细信息，请参阅 [transact-sql&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)) 中的 sys.databases &#40;。|  
|**role**|**tinyint**|镜像角色，为以下值之一：<br /><br /> **0** = 无<br /><br /> **1** = 伙伴<br /><br /> **2** = 见证服务器<br /><br /> **3** = 全部<br /><br /> 注意：此值仅适用于数据库镜像。|  
|**role_desc**|**nvarchar(60)**|镜像角色的说明，为以下值之一：<br /><br /> **NONE**<br /><br /> **PARTNER**<br /><br /> **见证**<br /><br /> **ALL**<br /><br /> 注意：此值仅适用于数据库镜像。|  
|**is_encryption_enabled**|**bit**|**1** 表示启用了加密。<br /><br /> **0** 表示禁用加密。|  
|**connection_auth**|**tinyint**|连接到此端点所需的连接身份验证的类型，为以下值之一：<br /><br /> **1** -NTLM<br /><br /> **2** -KERBEROS<br /><br /> **3** -协商<br /><br /> **4** -证书<br /><br /> **5** -NTLM，证书<br /><br /> **6** -KERBEROS，证书<br /><br /> **7** -协商，证书<br /><br /> **8** 证书，NTLM<br /><br /> **9** 证书，KERBEROS<br /><br /> **10** -证书，协商|  
|**connection_auth_desc**|**Nvarchar (60)**|连接到此端点所需的身份验证类型的说明，为以下值之一：<br /><br /> NTLM<br /><br /> KERBEROS<br /><br /> NEGOTIATE<br /><br /> CERTIFICATE<br /><br /> NTLM、CERTIFICATE<br /><br /> KERBEROS、CERTIFICATE<br /><br /> NEGOTIATE、CERTIFICATE<br /><br /> CERTIFICATE、NTLM<br /><br /> CERTIFICATE、KERBEROS<br /><br /> CERTIFICATE、NEGOTIATE|  
|**certificate_id**|**int**|身份验证所用证书的 ID（如果有）。<br /><br /> 0 = 使用 Windows 身份验证。|  
|**encryption_algorithm**|**tinyint**|加密算法，为以下值之一：<br /><br /> **0** -无<br /><br /> **1** -RC4<br /><br /> **2** -AES<br /><br /> **3** -无，RC4<br /><br /> **4** -无、AES<br /><br /> **5** -RC4、AES<br /><br /> **6** -AES、RC4<br /><br /> **7** -无、RC4、AES<br /><br /> **8** -无、AES、RC4|  
|**encryption_algorithm_desc**|**nvarchar(60)**|加密算法的说明，为以下值之一：<br /><br /> 无<br /><br /> RC4<br /><br /> AES<br /><br /> NONE, RC4<br /><br /> NONE, AES<br /><br /> RC4, AES<br /><br /> AES, RC4<br /><br /> NONE, RC4, AES<br /><br /> NONE, AES, RC4|  
  
## <a name="remarks"></a>备注  
  
> [!NOTE]  
>  RC4 算法仅用于支持向后兼容性。 仅当数据库兼容级别为 90 或 100 时，才能使用 RC4 或 RC4_128 对新材料进行加密。 （建议不要使用。）而是使用一种较新的算法，如 AES 算法之一。 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本中，可以在任何兼容级别对使用 RC4 或 RC4_128 加密的材料进行解密。  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [在添加或修改可用性副本时指定终结点 URL &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)   
 [sys.availability_replicas (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [sys.database_mirroring &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)   
 [sys.database_mirroring_witnesses &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   
 [数据库镜像终结点 (SQL Server)](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
