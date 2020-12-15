---
title: " (SQLXML) 的大容量加载安全注意事项"
description: 了解有关在 SQLXML 4.0 中使用 XML 大容量加载的安全准则。
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, XML Bulk Load
- bulk load [SQLXML], security
- security [SQLXML], XML Bulk Load
- XML Bulk Load [SQLXML], security
ms.assetid: 192fc6d4-ecbc-4a4d-a5cb-55e1f64af318
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ec361c23bfd946aaf47e817030237f80cdc4a3ef
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482938"
---
# <a name="bulk-load-security-considerations-sqlxml-40"></a>大容量加载安全性注意事项 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  下面是使用 XML 大容量加载的安全准则：  
  
-   当指定大容量加载操作作为事务执行时，可以使用 **TempFilePath** 属性指定用于创建临时文件的文件夹。  
  
     大容量加载进程用以下权限创建这些临时文件：  
  
    -   将读取/写入/删除访问权授予大容量加载进程。  
  
    -   读取权限将授予所有用户，因为 Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 用来访问这些文件的帐户是未知的。 通过对包含这些临时文件的文件夹设置适当的权限，可以限制对它们的访问。  
  
-   XML 大容量加载自身没有任何权限设置。 它假定数据库已正确设置，并且用户上下文（即大容量加载使用的登录信息）有合适的权限集。  
  
-   在非事务模式中，如果在大容量加载进程期间发生错误，则数据最后可能处于部分加载状态中。 大容量加载只是停止于发生这种情况时它所在的点上。 可以用事务模式缓解此问题。  
  
-   发生大容量加载错误时，这些错误可能包括有关数据库的信息。 例如，它们可能包括表或列的名称，或列类型信息。 使用大容量加载时，应当小心地从大容量加载进程捕获错误，并返回一般性错误消息，而不要将错误直接显示给用户。  
  
-   大容量加载不对它处理的数据的数量设置任何限制。 大容量加载不对要加载的数据的大小做任何检查。 确保有足够内存来处理指定的文件，并确保数据库有足够空间以存储所加载的数据，这二者是执行大容量加载的用户的责任。  
  
-   大容量加载不会尝试将提供给它的数据作为代码使用。 输入的数据永远不会以任何方式执行。 输入数据中的任何代码或命令均被视为正常数据，并且不会执行。  
  
-   大容量加载可能基于 XML 和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据模型之间的差异对给定的数据进行格式更改。 例如，二者指定时间所采用的格式各不相同。 大容量加载将尝试解决这些差异。 因此，可能会丢失某些精度信息。  
  
-   大容量加载对于它处理数据所用的时间长度没有设置任何限制。 处理将始终继续，直到处理完成，或发生错误。  
  
-   大容量加载可以在数据库中创建和删除临时表，但需要相应权限才能执行该操作。 对这些表的权限将授予正连接到数据库执行大容量加载进程的相同用户。  
  
-   大容量加载可以创建和删除在事务模式处理期间所使用的临时文件，但需要权限才能执行该操作。 创建这些文件时所用的权限将与正在运行大容量加载的线程的当前用户所拥有的权限相同。  
  
-   如果用户设置错误日志文件以便让 SQLXML 将错误写入其中，则在每次执行大容量加载时将以来自最后一个大容量加载进程的数据覆盖该文件。  
  
## <a name="see-also"></a>另请参阅  
 [对 XML 数据执行大容量加载 &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)  
  
  
