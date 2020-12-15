---
title: 比较复制表之间的差异（复制 SP）
description: 使用复制存储过程来比较发布服务器和订阅服务器上的复制表之间的差异。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- tablediff utility
- comparing replicated tables
ms.assetid: cd253a17-0c85-42b4-912c-690169ebe799
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 0b9717a6c71d5aef31dae428a67de37e9d9188ca
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97416808"
---
# <a name="compare-differences-between-replicated-tables-replication-programming"></a>比较复制表之间的差异（复制编程）
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  项目验证用于确定发布服务器和订阅服务器上的表项目的已发布数据是否不同，这可能表明无法收敛。 有关详细信息，请参阅[验证已复制的数据](../../../relational-databases/replication/validate-data-at-the-subscriber.md)。 但是，验证仅返回通过或失败信息，而不会提供任何有关源表和目标表之间存在哪些差异的信息。 **tablediff** 命令提示实用工具返回两个表之间存在的详细差异信息，甚至可生成 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 脚本，以使订阅与发布服务器上的数据实现收敛。  
  
> [!NOTE]  
>  **tablediff** 实用工具仅受 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务器的支持。  
  
### <a name="to-compare-replicated-tables-for-differences-using-tablediff"></a>使用 tablediff 比较复制的表之间的不同  
  
1.  从复制拓扑中任何服务器的命令提示符处，运行 [tablediff Utility](../../../tools/tablediff-utility.md)。 指定下列参数：  
  
    -   **-sourceserver** - 已知其上数据正确的服务器的名称，通常为发布服务器。  
  
    -   **-sourcedatabase** - 包含正确数据的数据库的名称。  
  
    -   **-sourcetable** - 要比较的项目的源表的名称。  
  
    -   （可选） **-sourceschema** - 源表的架构所有者（如果不为默认架构）。  
  
    -   （可选） **-sourceuser** 和 **-sourcepassword** （当使用 SQL Server 身份验证连接到发布服务器时。）  
  
        > [!IMPORTANT]  
        >  请尽可能使用 Windows 身份验证。 如果必须使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证，则在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
    -   **-destinationserver** - 要比较其上数据的服务器的名称，通常为订阅服务器。  
  
    -   **-destinationdatabase** - 要比较的数据库的名称。  
  
    -   **-destinationtable** - 要比较的表的名称。  
  
    -   （可选） **-destinationschema** - 目标表的架构所有者（如果不为默认架构）。  
  
    -   （可选） **-destinationuser** 和 **-destinationpassword** （当使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证连接到订阅服务器时。）  
  
        > [!IMPORTANT]  
        >  请尽可能使用 Windows 身份验证。 如果必须使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证，则在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
    -   （可选）使用 **-c** 来执行列级比较。  
  
    -   （可选）使用 **-q** 来执行快速的行计数和仅限架构的比较。  
  
    -   （可选）为 **-o** 指定文件名和路径以将结果输出到某个文件。  
  
    -   （可选）为 **-et** 指定要将结果插入其中的订阅数据库中的表。 如果该表已经存在，则指定 **-dt** 以首先删除该表。  
  
    -   （可选）使用 **-f** 生成 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 文件以修复订阅服务器上的数据，以便与发布服务器上的数据匹配。 使用 **-df** 指定每个文件中的 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句数量。  
  
    -   （可选）使用 **-rc** 和 **-ri** 指定重试某项操作的次数和重试时间间隔。  
  
    -   （可选）使用 **-strict** 以强制在源表和目标表之间执行严格的架构比较。  
  
## <a name="see-also"></a>另请参阅  
 [在订阅服务器上验证数据](../../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
  
