---
title: 启用和配置 FILESTREAM | Microsoft Docs
description: 要使用 FILESTREAM，请先在 SQL Server 数据库引擎实例上启用它。 了解如何使用 SQL Server 配置管理器启用 FILESTREAM。
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], enabling
ms.assetid: 78737e19-c65b-48d9-8fa9-aa6f1e1bce73
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ea678a62f4453232fe8442cf2053c3b951e361ef
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250656"
---
# <a name="enable-and-configure-filestream"></a>启用和配置 FILESTREAM

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  在开始使用 FILESTREAM 之前，必须在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]实例中启用 FILESTREAM。 本主题说明了如何使用 SQL Server 配置管理器来启用 FILESTREAM。  
  
##  <a name="enabling-filestream"></a><a name="enabling"></a> 启用 FILESTREAM  
  
#### <a name="to-enable-and-change-filestream-settings"></a>启用和更改 FILESTREAM 设置  
  
1.  在 **“开始”** 菜单中，依次指向 **“所有程序”** 、 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]、 **“配置工具”** ，然后单击 **“SQL Server 配置管理器”** 。  
  
2.  在服务列表中，右键单击“SQL Server 服务”  ，然后单击“打开”  。  
  
3.  在“SQL Server 配置管理器”  管理单元中，找到要在其中启用 FILESTREAM 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
4.  右键单击该实例，然后单击“属性”  。  
  
5.  在 **“SQL Server 属性”** 对话框中，单击 **“FILESTREAM”** 选项卡。  
  
6.  选中“针对 Transact-SQL 访问启用 FILESTREAM”  复选框。  
  
7.  如果要在 Windows 中读取和写入 FILESTREAM 数据，请单击“针对文件 I/O 流访问启用 FILESTREAM”  。 在 **“Windows 共享名”** 框中输入 Windows 共享的名称。  
  
8.  如果远程客户端必须访问存储在此共享中的 FILESTREAM 数据，请选择 **“允许远程客户端针对 FILESTREAM 数据启用流访问”** 。  
  
9. 单击“应用”  。  
  
10. 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，单击 **“新建查询”** 以显示查询编辑器。  
  
11. 在查询编辑器中，输入以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码：  
  
    ```sql  
    EXEC sp_configure filestream_access_level, 2  
    RECONFIGURE  
    ```  
  
12. 单击“执行”  。  
  
13. 重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。  

##  <a name="best-practices"></a><a name="best"></a>最佳做法  
  
###  <a name="physical-configuration-and-maintenance"></a><a name="config"></a>物理配置和维护  
 设置 FILESTREAM 存储卷时，请考虑下列准则：  
  
-   禁用 FILESTREAM 计算机系统中的短文件名。 创建短文件名需要花费相当长的时间。 若要禁用短文件名，请使用 Windows **fsutil** 实用工具。  
  
-   定期对 FILESTREAM 计算机系统进行碎片整理。  
  
-   使用 64-KB NTFS 簇。 压缩卷必须设置为 4-KB NTFS 簇。  
  
-   在 FILESTREAM 卷上禁用索引并设置 disablelastaccess  。 若要设置 disablelastaccess，请使用 Windows fsutil 实用程序   。  
  
-   除非必要，否则请禁止对 FILESTREAM 卷进行防病毒扫描。 如果需要进行防病毒扫描，请避免设置将自动删除有问题文件的策略。  
  
-   设置并调整 RAID 级别，以达到应用程序所需的容错能力和性能。  
  
|RAID 级别|写性能|读性能|容错|备注|  
|-|-|-|-|-|   
|RAID 5|一般|一般|很好|性能比一个磁盘或 JBOD 更好；比 RAID 0 或条带化 RAID 5 差。|  
|RAID 0|很好|很好|无||  
|RAID 5 ＋ 条带化|很好|很好|很好|成本最高的选项。|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |
  
  
###  <a name="physical-database-design"></a><a name="database"></a>物理数据库设计  
 设计 FILESTREAM 数据库时，应考虑下列准则：  
  
-   FILESTREAM 列必须附带相应的 **uniqueidentifier** ROWGUID 列。 这些类型的表还必须附带唯一索引。 此索引通常不是聚集索引。 如果数据库业务逻辑需要聚集索引，则必须确保该索引中存储的值不是随机的。 随机值将导致每次向表中添加行或从表中删除行时，索引都会重新排序。  
  
-   出于性能方面的考虑，FILESTREAM 文件组和容器应驻留在操作系统、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志、tempdb 或分页文件以外的卷上。  
  
-   FILESTREAM 不直接支持空间管理和策略。 但是，您可以通过将每个 FILESTREAM 文件组分配到独立的卷并使用该卷的管理功能来间接地管理空间和应用策略。  
  
  
