---
title: SQL Server 属性（“文件流”页）| Microsoft Docs
description: 熟悉 SQL Server 中的 FILESTREAM 设置。 了解如何打开 FILESTREAM，以及如何配置远程客户端访问和其他属性。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.filestream.f1
helpviewer_keywords:
- FILESTREAM [SQL Server], properties page
ms.assetid: 8a8d38d3-e97a-4b09-a40b-659b2e3a5c47
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bde5d28db431bb99e1721ee3984496dd10e6cf1c
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98764802"
---
# <a name="server-properties---filestream-page"></a>服务器属性 -“文件流”页
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用此页可针对此 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]安装启用 FILESTREAM。  
  
## <a name="ui-element-list"></a>UI 元素列表  
 **针对 Transact-SQL 访问启用 FILESTREAM**  
 选中此项可针对 [!INCLUDE[tsql](../../includes/tsql-md.md)] 访问启用 FILESTREAM。 必须选中此控制选项，才能使用其他控制选项。  
  
 **针对文件 I/O 流访问启用 FILESTREAM**  
 选中此项可针对 FILESTREAM 启用 Win32 流访问。  
  
 **Windows 共享名**  
 使用此控制选项可输入将用来存储 FILESTREAM 数据的 Windows 共享的名称。  
  
 **允许远程客户端针对 FILESTREAM 数据启用流访问**  
 选中此控制选项可允许远程客户端访问此服务器上的此 FILESTREAM 数据。  
  
## <a name="see-also"></a>另请参阅  
 [FILESTREAM (SQL Server)](../../relational-databases/blob/filestream-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
