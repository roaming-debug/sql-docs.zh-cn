---
title: 设置加密的镜像数据库 | Microsoft Docs
description: 了解如何通过使用 sp_control_dbmasterkey_password 创建凭据来启用镜像数据库的数据库主密钥的自动解密。
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- cryptography [SQL Server], database mirroring
- encryption [SQL Server], database mirroring
- database master key [SQL Server], database mirroring
- mirror database [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: 7329a575-be29-46e0-abc6-1344db37920c
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 78db0b212c0aaf2706f54478b59039b68ced9ec1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352293"
---
# <a name="set-up-an-encrypted-mirror-database"></a>设置加密的镜像数据库
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  若要对镜像数据库的数据库主密钥启用自动解密，必须提供用于加密该镜像服务器实例的主密钥的口令。 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本包括传输密码的机制。 在开始数据库镜像之前，请使用 **sp_control_dbmasterkey_password** 为数据库主密钥创建一个凭据。 必须为要镜像的每个数据库重复此过程。 有关详细信息，请参阅 [sp_control_dbmasterkey_password (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-control-dbmasterkey-password-transact-sql.md)。  
  
> [!CAUTION]  
>  对于 **sa** 和其他特权级别高的服务器主体无法访问的数据库，不要启用其故障转移解密功能。 可以对数据库进行配置，以便服务主密钥无法对其密钥层次结构进行解密。 对于包含 **sa** 或其他高特权服务器主体不应访问的信息的数据库来说，此选项是一项深层防御措施。 为此类数据库启用故障转移解密将解除此深层防御措施，使 **sa** 和其他高特权服务器主体能够解密该数据库。  
  
## <a name="see-also"></a>另请参阅  
 [sp_control_dbmasterkey_password (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-control-dbmasterkey-password-transact-sql.md)   
 [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md)   
 [ALTER MASTER KEY (Transact-SQL)](../../t-sql/statements/alter-master-key-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [设置数据库镜像 (SQL Server)](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)  
  
  
