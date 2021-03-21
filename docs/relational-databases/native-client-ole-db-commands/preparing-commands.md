---
description: 在 SQL Server Native Client 中准备命令
title: " (Native Client OLE DB 提供程序准备命令) "
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, commands
- prepared statements [SQL Server Native Client]
- commands [OLE DB]
- command preparation [SQL Server Native Client]
ms.assetid: 09ec0c6c-0a44-4766-b9b7-5092f676ee54
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 485c7d55a528e3fb8709afb4243b17b5d2b9346e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749007"
---
# <a name="preparing-commands-in-sql-server-native-client"></a>在 SQL Server Native Client 中准备命令
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口支持命令准备，以优化多次执行的单个命令。不过，命令准备会带来开销，使用者不必准备执行次数多于一次的命令。 一般而言，如果命令的执行次数超过三次，则应当进行准备。  
  
 出于性能方面的考虑，命令准备会延迟至命令执行之时。 此选项为默认行为。 待准备命令中的任何错误，直到执行命令或执行元属性操作时才会发现。 将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 属性 SSPROP_DEFERPREPARE 设置为 FALSE 可以关闭此默认行为。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，直接执行命令（即不提前准备命令）时，会创建并缓存一个执行计划。 如果再次执行该 SQL 语句，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会采用有效的算法将新的语句与缓存中的现有执行计划进行匹配，然后再次使用该语句的执行计划。  
  
 对于准备好的命令，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为准备和执行命令语句提供了本机支持。 在准备语句时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 创建一个执行计划，进行缓存，然后将该执行计划的句柄返回访问接口。 随后，访问接口使用该句柄来重复执行该语句。 不创建存储过程。 由于句柄会直接识别 SQL 语句的执行计划，而不用将语句与缓存中的现有语句进行匹配（如直接执行那样），因此，如果知道语句将多次执行，那么准备语句比直接执行语句更高效。  
  
 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中，无法使用预定义语句创建临时对象，也无法引用创建临时对象（如临时表）的系统存储过程。 必须直接执行这些过程。  
  
 始终不应准备某些命令。 例如，不应对指定存储过程执行的命令进行准备，也不应准备包含对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程创建无效的文本的命令。  
  
 如果创建了临时存储过程，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口会执行临时存储过程，并返回结果，就好像执行语句本身一样。  
  
 临时存储过程的创建由特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口的初始化属性 SSPROP_INIT_USEPROCFORPREP 控制。 如果属性值为 SSPROPVAL_USEPROCFORPREP_ON 或 SSPROPVAL_USEPROCFORPREP_ON_DROP，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口会在准备命令时尝试创建存储过程。 如果应用程序用户拥有足够的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 权限，存储过程将创建成功。  
  
 对于很少断开连接的使用者，临时存储过程的创建可能需要大量的 tempdb（即在其中创建临时对象的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据库）资源。 如果 SSPROP_INIT_USEPROCFORPREP 的值为 SSPROPVAL_USEPROCFORPREP_ ON，那么由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口创建的临时存储过程，仅当创建命令的会话断开与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的连接时才会删除。 如果该连接为数据源初始化时创建的默认连接，则临时存储过程仅当数据源变成未初始化状态时才删除。  
  
 如果 SSPROP_INIT_USEPROCFORPREP 的值为 SSPROPVAL_USEPROCFORPREP_ON_DROP，那么当发生以下情况之一时，会删除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口临时存储过程：  
  
-   使用者使用 ICommandText::SetCommandText 指示新的命令  。  
  
-   使用者使用 ICommandPrepare::Unprepare 指示它不再需要命令文本  。  
  
-   使用者使用临时存储过程释放对命令对象的所有引用。  
  
 命令对象在 tempdb 中最多具有一个临时存储过程  。 任何现有的临时存储过程都表示特定命令对象的当前命令文本。  
  
## <a name="see-also"></a>另请参阅  
 [命令](../../relational-databases/native-client-ole-db-commands/commands.md)  
  
  
