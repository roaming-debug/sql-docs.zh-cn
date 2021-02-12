---
description: '删除 SSMA for DB2 组件 (DB2ToSQL) '
title: 删除 SSMA for DB2 组件 (DB2ToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4ee0d698-6246-48eb-b963-d62be81cab6a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 63957996baa6f65b864a3fe74bdb6b5c90aab3a4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072001"
---
# <a name="removing-ssma-for-db2-components-db2tosql"></a>删除 SSMA for DB2 组件 (DB2ToSQL) 
完成将数据库从 DB2 迁移到之后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，你可能想要卸载 SSMA 组件。 你可以随时卸载客户端组件。 但是， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 除非您迁移的数据库不再使用 **sysdb** 数据库的 **ssma_DB2** 架构中的函数，否则不应从卸载扩展包。  
  
## <a name="uninstalling-the-ssma-for-db2-client"></a>卸载 DB2 客户端的 SSMA  
可以使用 " **添加或删除程序**" 卸载 SSMA。  
  
**卸载 SSMA**  
  
1.  在“控制面板”中，打开“添加或删除程序”  。  
  
2.  选择 " **[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 迁移助手**"，然后单击 "**删除**"。  
  
3.  若要确认是否要卸载 SSMA，请单击 **"是"**。  
  
## <a name="uninstalling-the-extension-pack"></a>正在卸载扩展包  
如果你确定已迁移的数据库不使用 **sysdb.ssma_DB2** 架构中的对象，则可以通过将其从架构中删除来删除扩展包-无 Windows 卸载  
  
## <a name="see-also"></a>另请参阅  
[安装 SSMA for DB2 Client &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-for-db2-client-db2tosql.md)  
[在 SQL Server 上安装 SSMA 组件 &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-components-on-sql-server-db2tosql.md)  
  
