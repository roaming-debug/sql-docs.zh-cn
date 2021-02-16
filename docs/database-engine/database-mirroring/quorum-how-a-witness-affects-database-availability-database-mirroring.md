---
title: 见证服务器如何影响数据库可用性
description: 了解数据库镜像如何影响仲裁和数据库可用性。 仲裁可确保一次只有一个伙伴拥有数据库。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- quorum [SQL Server], database mirroring
- running exposed in database mirroring [SQL Server]
- witness-to-partner quorum [SQL Server]
- partners [SQL Server], partner-to-partner quorum
- witness [SQL Server], quorum
- have quorum [SQL Server]
- quorum [SQL Server]
- mirror database [SQL Server]
- full quorum [SQL Server]
- high-availability mode [SQL Server]
ms.assetid: a62d9dd7-3667-4751-a294-a61fc9caae7c
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 5b2597b1b8b38cddeb28d9cf17beda0144b9e4bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337892"
---
# <a name="quorum-how-a-witness-affects-database-availability-database-mirroring"></a>仲裁：见证服务器如何影响数据库可用性（数据库镜像）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  每当为数据库镜像会话设置见证服务器时，都需要“仲裁  ”。 仲裁是数据库镜像会话中两个或多个服务器实例彼此连接时存在的一种关系。 仲裁通常包括三个互连的服务器实例。 设置见证服务器时，必须具有仲裁才能使用数据库。 仲裁旨在用于具有自动故障转移功能的高安全性模式，可确保一个数据库一次只属于一个伙伴。  
  
 如果特定的服务器实例与镜像会话断开连接，则该实例将失去仲裁。 如果没有连接任何服务器实例，则会话将失去仲裁，并无法使用数据库。 可以进行的仲裁有三种：  
  
-   “完全仲裁  ”包含伙伴双方以及见证服务器。  
  
-   见证服务器-伙伴仲裁包含见证服务器和一个伙伴。  
  
-   伙伴-伙伴仲裁包含伙伴双方。  
  
 下图显示了这三种类型的仲裁。  
  
 ![仲裁：完全；见证服务器和伙伴；伙伴双方](../../database-engine/database-mirroring/media/dbm-failovautoquorum.gif "仲裁：完全；见证服务器和伙伴；伙伴双方")  
  
 只要当前的主体服务器具有仲裁，它就拥有主体服务器的角色并可继续操作数据库，除非数据库所有者执行手动故障转移。 如果主体服务器失去仲裁，它将停止操作数据库。 仅当主体服务器失去仲裁时，才会发生自动故障转移，这确保它不再操作数据库。  
  
 断开连接的服务器实例将保存其在会话中的最新角色。 通常，断开连接的服务器实例将在重新启动并重新获得仲裁时重新连接到会话。  
  
> [!IMPORTANT]  
>  只有在需要使用具有自动故障转移功能的高安全性模式时，才应设置见证服务器。 在高性能模式下，由于从不需要见证服务器，因此极力建议将 WITNESS 属性设置为 OFF。 有关见证服务器对高性能模式影响的信息，请参阅 [数据库镜像运行模式](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)。  
  
## <a name="quorum-in-high-safety-mode-sessions"></a>高安全性模式会话中的仲裁  
 在高安全性模式下，仲裁通过提供上下文来允许自动故障转移，在这个上下文中，具有仲裁的服务器实例可以判定哪个伙伴拥有主体服务器角色。 主体服务器如果具有仲裁就可以操作数据库。 如果在同步的镜像服务器和见证服务器仍具有仲裁的时候主体服务器丢失仲裁，则会发生自动故障转移。  
  
 高安全性模式的仲裁方案包括：  
  
-   包含伙伴双方和见证服务器的“完全仲裁  ”。  
  
     所有三个服务器实例通常都参与三方仲裁，这称为完全仲裁。 使用完全仲裁，主体服务器和镜像服务器一直执行其各自的角色（除非发生手动故障转移）。  
  
-   包含见证服务器和一个伙伴的见证服务器-伙伴仲裁。  
  
     如果因为其中一个伙伴丢失而中断伙伴之间的网络连接，则可能发生下列情况：  
  
    -   镜像服务器丢失，主体服务器和见证服务器仍具有仲裁。  
  
         在这种情况下，主体服务器将数据库设置为 DISCONNECTED，并在镜像处于 SUSPENDED 状态的情况下运行。 （因为数据库当前尚未镜像，所以这称为公开运行。）镜像服务器重新联接会话时，它将作为镜像服务器重新获得仲裁，并开始与其数据库副本重新同步。  
  
    -   主体服务器丢失，见证服务器和镜像服务器仍具有仲裁。  
  
         在这种情况下，发生自动故障转移。 有关详细信息，请参阅 [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)。  
  
    -   所有服务器实例都将失去仲裁，但随后镜像和见证服务器将重新连接。 在此情况下将不提供数据库服务。  
  
     两个伙伴与见证服务器之间保持连接时，故障转移伙伴之间的网络连接很少会断开。 在这种情况下，存在两个分开的见证服务器-伙伴仲裁，见证服务器作为连接。 见证服务器将通知镜像服务器：主体服务器仍在连接状态。 因此，不会出现自动故障转移。 而镜像服务器将保留镜像角色并等待重新连接到主体服务器。 如果此时重做队列包含日志记录，镜像服务器将继续前滚镜像数据库。 重新连接后，镜像服务器将与镜像数据库重新同步。  
  
-   包含伙伴双方的伙伴-伙伴仲裁。  
  
     只要伙伴仍具有仲裁，数据库就会继续处于 SYNCHRONIZED 状态，手动故障转移就可以进行。 如果没有见证服务器，则无法使用自动故障转移功能；但当见证服务器重新获得仲裁时，会话将恢复正常操作，并重新支持自动故障转移。  
  
-   会话丢失仲裁。  
  
     如果所有服务器实例此间的连接断开，就称为会话“丢失仲裁 ”。 当服务器实例恢复彼此间的连接时，它们将重新获得相互仲裁。  
  
    -   如果主体服务器与其他服务器实例中的任何一个重新连接，即可使用数据库。  
  
    -   如果主体服务器依旧断开连接，但镜像服务器和见证服务器恢复了相互之间的连接，则不能进行自动故障转移，因为可能会丢失数据。 因此，在主体服务器重新加入会话之前，依旧不能使用数据库。  
  
    -   当三个服务器实例全部恢复连接时，将重新获得完全仲裁，会话将恢复其正常操作。  
  
> [!IMPORTANT]  
>  当会话具有伙伴-伙伴仲裁时，如果任一伙伴失去仲裁，会话将失去仲裁。 因此，如果您希望见证服务器在很长一段时间内保持断开，我们建议您暂时将见证服务器从会话中删除。 如果删除见证服务器，则不再需要仲裁。 然后，如果镜像服务器断开连接，则主体服务器可以继续操作数据库。 有关如何添加或删除见证服务器的信息，请参阅 [Database Mirroring Witness](../../database-engine/database-mirroring/database-mirroring-witness.md)。  
  
### <a name="how-quorum-affects-database-availability"></a>仲裁如何影响数据库可用性  
 下图显示的是见证服务器与伙伴如何相互作用，以确保在给定时间内，只有一个伙伴拥有主体服务器角色并且只有当前主体服务器才能使其数据库联机。 两个方案都以完全仲裁（ **Partner_A** 具有主体角色， **Partner_B** 具有镜像角色）为起点。  
  
 ![见证服务器与伙伴的协作方式](../../database-engine/database-mirroring/media/dbm-quorum-scenarios.gif "见证服务器与伙伴的协作方式")  
  
 方案 1 显示的是：在原始主体服务器 (**Partner_A**) 失败后，见证服务器和镜像服务器如何同时认定主体 **Partner_A** 不再可用并构成仲裁。 然后，镜像服务器 **Partner_B** 承担主体角色。 出现自动故障转移时， **Partner_B** 使其数据库副本联机。 然后 **Partner_B** 出现故障，数据库脱机。 随后，先前的主体服务器 **Partner_A** 重新连接到见证服务器重新获取仲裁，但是通过与见证服务器通信， **Partner_A** 获知不能使其数据库副本联机，因为 **Partner_B** 现在拥有主体角色。 当 **Partner_B** 重新加入会话时，将使数据库恢复联机。  
  
 在方案 2 中，见证服务器丢失仲裁，而伙伴 **Partner_A** 和 **Partner_B** 彼此保留仲裁，数据库保持联机。 然后，伙伴们也失去其仲裁，数据库处于脱机状态。 随后，主体服务器 **Partner_A** 重新连接到见证服务器以重新获取仲裁。 见证服务器确认 **Partner_A** 仍拥有主体角色，并且 **Partner_A** 使数据库恢复联机。  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像运行模式](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)   
 [数据库镜像会话期间的角色切换 (SQL Server)](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [数据库镜像见证服务器](../../database-engine/database-mirroring/database-mirroring-witness.md)   
 [数据库镜像期间可能出现的故障](../../database-engine/database-mirroring/possible-failures-during-database-mirroring.md)   
 [镜像状态 (SQL Server)](../../database-engine/database-mirroring/mirroring-states-sql-server.md)  
  
  
