---
description: sys.service_broker_endpoints (Transact-SQL)
title: sys.service_broker_endpoints (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.service_broker_endpoints_TSQL
- service_broker_endpoints
- service_broker_endpoints_TSQL
- sys.service_broker_endpoints
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_broker_endpoints catalog view
ms.assetid: 6979ec9b-0043-411e-aafb-0226fa26c5ba
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7a3859eea7237eebb83b994fef037e377b433940
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212107"
---
# <a name="sysservice_broker_endpoints-transact-sql"></a>sys.service_broker_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Service Broker 端点在此目录视图中占一行。 对于此视图中的每一行，都有一个对应的行在包含 TCP 配置元数据的 **sys.tcp_endpoints** 视图中具有相同的 **endpoint_id** 。 TCP 是唯一允许 Service Broker 使用的协议。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|**--**|从 sys.databases 中继承列 [&#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)。|  
|**is_message_forwarding_enabled**|**bit**|端点支持消息转发。 这最初设置为 **0** (禁用) 。 不可为 NULL。|  
|**message_forwarding_size**|**int**|允许用于转发消息的 **tempdb** 空间的最大兆字节数。 最初设置为 **10**。 不可为 NULL。|  
|**connection_auth**|**tinyint**|连接到此端点所需的连接身份验证的类型，为以下值之一：<br /><br /> **1** -NTLM<br /><br /> **2** -KERBEROS<br /><br /> **3** -协商<br /><br /> **4** -证书<br /><br /> **5** -NTLM，证书<br /><br /> **6** -KERBEROS，证书<br /><br /> **7** -协商，证书<br /><br /> **8** 证书，NTLM<br /><br /> **9** 证书，KERBEROS<br /><br /> **10** -证书，协商<br /><br /> 不可为 NULL。|  
|**connection_auth_desc**|**nvarchar(60)**|连接到此端点所需的连接身份验证类型的说明，可以是下列值之一：<br /><br /> NTLM<br /><br /> KERBEROS<br /><br /> NEGOTIATE<br /><br /> CERTIFICATE<br /><br /> NTLM、CERTIFICATE<br /><br /> KERBEROS、CERTIFICATE<br /><br /> NEGOTIATE、CERTIFICATE<br /><br /> CERTIFICATE、NTLM<br /><br /> CERTIFICATE、KERBEROS<br /><br /> CERTIFICATE、NEGOTIATE<br /><br /> 可以为 null.|  
|**certificate_id**|**int**|身份验证所用证书的 ID（如果有）。<br /><br /> 0 = 使用 Windows 身份验证。|  
|**encryption_algorithm**|**tinyint**|加密算法。 下面是可能的值及其说明和相应的 DDL 选项。<br /><br /> **0** ：无。 对应的 DDL 选项：已禁用。<br /><br /> **1** ： RC4。 对应的 DDL 选项： {必需 &#124; 必需的算法 RC4}。<br /><br /> **2** ： AES。 对应的 DDL 选项：所需的算法 AES。<br /><br /> **3** ：无，RC4。 对应的 DDL 选项： {支持的 &#124; 支持的算法 RC4}。<br /><br /> **4** ： NONE、AES。 对应的 DDL 选项：支持的算法 AES。<br /><br /> **5** ： RC4、AES。 对应的 DDL 选项：必需的算法 RC4 AES。<br /><br /> **6** ： AES、RC4。 对应的 DDL 选项： Required 算法 AES RC4。<br /><br /> **7** ：无、RC4、AES。 对应的 DDL 选项：支持的算法 RC4 AES。<br /><br /> **8** ：无、AES、RC4。 对应的 DDL 选项：支持的算法 AES RC4。<br /><br /> 不可为 NULL。|  
|**encryption_algorithm_desc**|**nvarchar(60)**|加密算法说明。 下面列出了可能的值及其相应的 DDL 选项：<br /><br /> 无：已禁用<br /><br /> RC4： {必需 &#124; 必需的算法 RC4}<br /><br /> AES：所需算法 AES<br /><br /> 无，RC4： {受支持的 &#124; 支持的算法 RC4}<br /><br /> 无、AES：支持的算法 AES<br /><br /> RC4，AES：所需的算法 RC4 AES<br /><br /> AES，RC4：所需算法 AES RC4<br /><br /> 无、RC4、AES：支持的算法 RC4 AES<br /><br /> 无、AES、RC4：支持的算法 AES RC4<br /><br /> 可以为 null.|  
  
## <a name="remarks"></a>备注  
  
> [!NOTE]  
>  RC4 算法仅用于支持向后兼容性。 仅当数据库兼容级别为 90 或 100 时，才能使用 RC4 或 RC4_128 对新材料进行加密。 （建议不要使用。）而是使用一种较新的算法，如 AES 算法之一。 在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 和更高版本中，可以在任何兼容性级别对使用 RC4 或 RC4_128 加密的材料进行解密。  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)  
  
  
