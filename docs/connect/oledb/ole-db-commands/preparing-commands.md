---
title: 准备命令（OLE DB 驱动程序）
description: 如果单个命令要运行多次，OLE DB Driver for SQL Server 支持通过命令准备提高性能。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, commands
- prepared statements [OLE DB Driver for SQL Server]
- commands [OLE DB]
- command preparation [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6b682c54374bed8645ff45f7f4fadc59b9d0f828
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735507"
---
# <a name="preparing-commands"></a>准备命令
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  适用于 SQL Server 的 OLE DB 驱动程序支持命令准备，以优化单个命令的多次执行。不过，命令准备会带来开销，使用者不必准备执行次数多于一次的命令。 一般而言，如果命令的执行次数超过三次，则应当进行准备。  
  
 出于性能方面的考虑，命令准备会延迟至命令执行之时。 此选项为默认行为。 待准备命令中的任何错误，直到执行命令或执行元属性操作时才会发现。 将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 属性 SSPROP_DEFERPREPARE 设置为 FALSE 可以关闭此默认行为。  
  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中，直接执行命令（即不提前准备命令）时，会创建并缓存一个执行计划。 如果再次执行该 SQL 语句，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 会采用有效的算法将新的语句与缓存中的现有执行计划进行匹配，然后再次使用该语句的执行计划。  
  
 对于准备好的命令，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 为准备和执行命令语句提供了本机支持。 在准备语句时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 创建一个执行计划，进行缓存，然后将该执行计划的句柄返回访问接口。 随后，访问接口使用该句柄来重复执行该语句。 不创建存储过程。 由于句柄会直接识别 SQL 语句的执行计划，而不用将语句与缓存中的现有语句进行匹配（如直接执行那样），因此，如果知道语句将多次执行，那么准备语句比直接执行语句更高效。  
  
 在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 中，无法使用预定义语句创建临时对象，也无法引用创建临时对象（如临时表）的系统存储过程。 必须直接执行这些过程。  
  
 始终不应准备某些命令。 例如，不应对指定存储过程执行的命令进行准备，也不应准备包含对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 存储过程创建无效的文本的命令。  
  
 如果创建了临时存储过程，则适用于 SQL Server 的 OLE DB 驱动程序会执行临时存储过程，并返回结果，就像执行语句本身一样。  
  
 临时存储过程的创建由特定于适用于 SQL Server 的 OLE DB 驱动程序的初始化属性 SSPROP_INIT_USEPROCFORPREP 控制。 如果属性值为 SSPROPVAL_USEPROCFORPREP_ON 或 SSPROPVAL_USEPROCFORPREP_ON_DROP，则适用于 SQL Server 的 OLE DB 驱动程序会在准备命令时尝试创建存储过程。 如果应用程序用户拥有足够的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 权限，存储过程将创建成功。  
  
 对于很少断开连接的使用者，临时存储过程的创建可能需要大量的 tempdb（即在其中创建临时对象的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 系统数据库）资源。 如果 SSPROP_INIT_USEPROCFORPREP 的值为 SSPROPVAL_USEPROCFORPREP_ ON，那么由适用于 SQL Server 的 OLE DB 驱动程序创建的临时存储过程在仅当创建命令的会话断开与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的连接时才会被删除。 如果该连接为数据源初始化时创建的默认连接，则临时存储过程仅当数据源变成未初始化状态时才删除。  
  
 如果 SSPROP_INIT_USEPROCFORPREP 的值为 SSPROPVAL_USEPROCFORPREP_ON_DROP，那么当发生以下情况之一时，会删除适用于 SQL Server 的 OLE DB 驱动程序临时存储过程：  
  
-   使用者使用 ICommandText::SetCommandText 指示新的命令  。  
  
-   使用者使用 ICommandPrepare::Unprepare 指示它不再需要命令文本  。  
  
-   使用者使用临时存储过程释放对命令对象的所有引用。  
  
 命令对象在 tempdb 中最多具有一个临时存储过程  。 任何现有的临时存储过程都表示特定命令对象的当前命令文本。  
  
## <a name="see-also"></a>另请参阅  
 [命令](../../oledb/ole-db-commands/commands.md)  
  
  
