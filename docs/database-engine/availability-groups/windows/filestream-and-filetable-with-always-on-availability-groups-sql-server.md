---
title: 将 FILESTREAM 和 FileTable 与可用性组一起使用
description: 将 FILESTREAM 或 FileTable 与加入 Always On可用性组的数据库一起使用的步骤。
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- FileTables [SQL Server], Availability Groups
- FILESTREAM [SQL Server], Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: fdceda9a-a9db-4d1d-8745-345992164a98
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7019f87f5b6476c273a20008fb6d081f816a4e2e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353872"
---
# <a name="use-filestream-and-filetable-with-always-on-availability-groups"></a>将 FILESTREAM 和 FileTable 与 AlwaysOn 可用性组一起使用

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  本主题包含将 FILESTREAM 和 FileTable 功能与 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 中的 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]一起使用有关的信息。  
  
 支持所有 FILESTREAM 功能。 故障转移后，FILESTREAM 数据在可读辅助副本和新的主副本上均可访问。  
  
 支持部分 FileTable 功能。 故障转移后，FileTable 数据在主副本上是可访问的，但是在可读辅助副本上不可访问。  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
  
-   在将使用 FILESTREAM 的数据库（具有或不具有 FileTable）添加到某一可用性组之前，请确保在承载该可用性组的可用性副本的每个服务器实例上都启用 FILESTREAM。 有关详细信息，请参阅 [Enable and Configure FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)。  
  
##  <a name="using-virtual-network-names-vnns-for-filestream-and-filetable-access"></a><a name="vnn"></a> 为 FILESTREAM 和 FileTable 访问使用虚拟网络名称 (VNN)  
 当您在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例上启用 FILESTREAM 时，将创建实例级别共享以便提供对 FILESTREAM 数据的访问。 可通过按以下格式使用计算机名称来访问此共享：  
  
 `\\<computer_name>\<filestream_share_name>`  
  
 但在 AlwaysOn 可用性组中，通过使用虚拟网络名称 (VNN) 虚拟化计算机的名称。 在该计算机是某一可用性组中的主副本，并且该可用性组中的数据库包含 FILESTREAM 数据时，还创建 VNN 范围的共享以便提供对 FILESTREAM 数据的访问。 这并不影响对 FILESTREAM 数据的 Transact-SQL 访问。 但是，使用文件系统 API 的应用程序必须使用 VNN 范围的共享，它具有以下格式的路径：  
  
 `\\<VNN>\<filestream_share_name>`  
  
 在发生以下事件之一时将创建 VNN 范围的共享。  
  
-   你将包含 FILESTREAM 数据的数据库添加到主要副本上的 AlwaysOn 可用性组。 在此情况下，共享 `\\<computer_name>\<filestream_share_name>` 已存在。 创建共享 `\\<VNN>\<filestream_share_name>` 。  
  
-   您对具有可用性组的主副本上的文件 i/o 流访问启用 FILESTREAM。 创建以下共享：  
  
    1.  `\\<computer_name>\<filestream_share_name>`  
  
    2.  `\\<VNN1>\<filestream_share_name>` 。  
  
    3.  `\\<VNN2>\<filestream_share_name>` 。  
  
 这些 VNN 范围的共享也传播到所有辅助副本。  
  
 在包含 FILESTREAM 或 FileTable 数据的数据库属于某一 AlwaysOn 可用性组时：  
  
-   FILESTREAM 和 FileTable 函数接受或返回虚拟网络名称 (VNN)，而非计算机名称。 有关这些函数的详细信息，请参阅 [Filestream 和 FileTable 函数 (Transact-SQL)](../../../relational-databases/system-functions/filestream-and-filetable-functions-transact-sql.md)。  
  
-   通过文件系统 API 对 FILESTREAM 或 FileTable 数据进行的所有访问都应该使用 VNN，而非计算机名称。  
  
 在该数据库是某一可用性组的一部分时，如果您的应用程序尝试使用格式为 `\\<computer_name>\<filestream_share_name>` 的计算机名称访问该共享，将会引发错误。  
  
 在该数据库不是某一可用性组的一部分时，如果您的应用程序尝试使用 VNN 范围的路径访问该共享，则该请求可能会成功。 在此情况下，虚拟网络名称将解析为计算机名称。 但是，强烈不推荐这一用法，因为如果删除该可用性组，该 VNN 范围的路径将会停止。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [启用和配置 FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)  
  
-   [启用 FileTable 的先决条件](../../../relational-databases/blob/enable-the-prerequisites-for-filetable.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
 无。  
  
## <a name="see-also"></a>另请参阅  
 [AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
