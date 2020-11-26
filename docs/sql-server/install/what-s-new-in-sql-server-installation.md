---
title: SQL Server 安装中的新增功能 | Microsoft Docs
description: 本文总结了对 SQL Server 安装过程的一些更改，包括 SysPrep 支持和从 SQL Server 2005 升级。
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 9b136b27-4779-4284-904f-b5a1c12bdcc0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 459a40bdd51f1762e81bf17a362d1941ee7b01be
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96120896"
---
# <a name="what39s-new-in-sql-server-installation"></a>SQL Server 安装中的新增功能
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

 仅 x64 处理器支持安装。 有关详细信息，请参阅[安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。
  
 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的安装将提示您指定要将提取的包保存到的目录。 如果未输入位置，服务器将默认为计算机的系统驱动器。 在 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 安装完毕后，提取的文件仍将保持。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的所有安装均支持 SysPrep。 SysPrep 现在支持故障转移群集安装。 有关详细信息，请参阅 [使用 SysPrep 安装 SQL Server 的注意事项](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md) 和 [使用 SysPrep 安装 SQL Server](../../database-engine/install-windows/install-sql-server-using-sysprep.md)。  
  
 支持从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 升级，但不支持\-并行\-升级。 有关 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]支持的详细信息，请参阅 [支持的版本升级](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)。  
 
  
## <a name="see-also"></a>另请参阅  
[SQL Server 中的新增功能](../../sql-server/what-s-new-in-sql-server-2017.md)

[SQL Server 的最大容量规范](../../sql-server/maximum-capacity-specifications-for-sql-server.md)   

[计划 SQL Server 安装](../../sql-server/install/planning-a-sql-server-installation.md)   

[安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)  
  
