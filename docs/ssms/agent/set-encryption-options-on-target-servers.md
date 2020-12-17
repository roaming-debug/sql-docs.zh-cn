---
description: 在目标服务器上设置加密选项
title: 在目标服务器上设置加密选项
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, encryption
- target servers [SQL Server], encryption
- multiserver environments [SQL Server], setting encryption options on target servers
ms.assetid: 1a9fd539-e166-4ea8-9f21-ac400ca74dee
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 09454c54f5c0f66201df1bf1968b6dfd3aaa2705
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464348"
---
# <a name="set-encryption-options-on-target-servers"></a>在目标服务器上设置加密选项
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

如果无法在主服务器和部分/所有目标服务器之间的传输层安全性 (TLS)（以前称为“安全套接字层 (SSL)”）加密通信中使用证书，但需要对它们之间的通道进行加密，请将目标服务器配置为使用所需的安全级别。  
  
若要为特定的主服务器/目标服务器通信通道配置相应所需的安全级别，请将目标服务器上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的注册表子项 **\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\** \<*instance_name*> **\SQLServerAgent\MsxEncryptChannelOptions(REG_DWORD)** 设置为下列值之一。 \<*instance_name*> 的值是 **MSSQL.** _n_。 例如， **MSSQL.1** 或 **MSSQL.3**。  
  
|值|说明|  
|---------|---------------|  
|**0**|在该目标服务器和主服务器之间禁用加密。 请仅在目标服务器和主服务器之间的通道已使用其他方法进行了保护时才选择此选项。|  
|**1**|仅在该目标服务器和主服务器之间启用加密，但不需要证书验证。|  
|**2**|在此目标服务器和主服务器之间启用完全 TLS 加密和证书验证。 此设置为默认设置。 除非出于特定的原因要选择其他值，否则建议不要对其进行更改。|  
  
如果指定 1  或 2  ，则必须同时在主服务器和目标服务器上启用 TLS。 如果指定了 **2** ，则还必须在主服务器上安装相应的签名证书。  
  
> [!CAUTION]  
> [!INCLUDE[ssNoteRegistry](../../includes/ssnoteregistry-md.md)]  
  
## <a name="see-also"></a>另请参阅  
[如何：启用数据库引擎的加密连接（SQL Server 配置管理器）](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)  
