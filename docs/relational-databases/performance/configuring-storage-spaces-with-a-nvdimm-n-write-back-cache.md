---
title: 配置存储 - NVDIMM-N 回写式缓存
description: 了解如何将具有镜像 NVDIMM-N 回写式缓存的镜像存储空间设置为虚拟驱动器，以存储 SQL Server 事务日志。
ms.custom: seo-dt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: 861862fa-9900-4ec0-9494-9874ef52ce65
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8cde267305ee7e7666443d0cf9c7f96dee92d072
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505334"
---
# <a name="configuring-storage-spaces-with-a-nvdimm-n-write-back-cache"></a>配置具有 NVDIMM-N 回写式缓存的存储空间
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Windows Server 2016 支持允许以极快地速度进行输入/输出 (I/O) 操作的 NVDIMM-N设备。 使用这种设备的一种新颖方式是作为回写式缓存来实现低写入延迟。 本主题讨论如何将具有镜像 NVDIMM-N 回写式缓存的镜像存储空间设置为虚拟驱动器，以存储 SQL Server 事务日志。 如果你也打算利用其来存储数据表或其他数据，则你可能需在存储池中包含更多磁盘，或创建多个池（如果隔离很重要）。  
  
 若要使用此技术查看 Channel 9 视频，请查看 [Using Non-volatile Memory (NVDIMM-N) as Block Storage in Windows Server 2016](https://channel9.msdn.com/Events/Build/2016/P466)（在 Windows Server 2016 中将稳定存储用作块存储）。  
  
## <a name="identifying-the-right-disks"></a>确定正确的磁盘  
 在 Windows Server 2016 中安装存储空间，尤其是安装具有高级功能（例如回写式缓存）的存储空间可非常轻易地通过 PowerShell 实现。 第一步是确定该存储空间池（将从此池创建虚拟磁盘）要包括的磁盘。 NVDIMM-N 具有 SCM（存储级内存）的介质类型和总线类型，可通过 Get-PhysicalDisk PowerShell cmdlet 查询。  
  
```  
Get-PhysicalDisk | Select FriendlyName, MediaType, BusType  
```  
  
 ![Windows Powershell 窗口的屏幕截图，其中显示了 Get-PhysicalDisk cmdlet 的输出。](../../relational-databases/performance/media/get-physicaldisk.png "Get-PhysicalDisk")  
  
> [!NOTE]  
>  使用 NVDIMM-N 设备，你不再需要具体选择可作为回写式缓存目标的设备。  
  
 若要生成带镜像回写式缓存的镜像虚拟磁盘，至少需要 2 个 NVDIMM-N 和其他 2 个磁盘。 生成池前，将所需物理磁盘分配到一个变量可使此过程更简单。  
  
```  
$pd =  Get-PhysicalDisk | Select FriendlyName, MediaType, BusType | WHere-Object {$_.FriendlyName -like 'MK0*' -or $_.FriendlyName -like '2c80*'}  
```  
  
 屏幕截图显示 $pd 变量以及其使用以下 PowerShell cmdlet 分配返回的 2 个 SSD 和 2 个 NVDIMM-N。  
  
```  
$pd | Select FriendlyName, MediaType, BusType  
```  
  
 ![Windows Powershell 窗口的屏幕截图，其中显示了 $pd cmdlet 的输出。](../../relational-databases/performance/media/select-friendlyname.png "选择 FriendlyName")  
  
## <a name="creating-the-storage-pool"></a>创建存储池  
 使用包含 PhysicalDisks 的 $pd 变量，可通过 New-StoragePool PowerShell cmdlet 轻松创建存储池。  
  
```  
New-StoragePool -StorageSubSystemFriendlyName "Windows Storage*" -FriendlyName NVDIMM_Pool -PhysicalDisks $pd  
```  
  
 ![Windows Powershell 窗口的屏幕截图，其中显示了 New-StoragePool cmdlet 的输出。](../../relational-databases/performance/media/new-storagepool.png "New-StoragePool")  
  
## <a name="creating-the-virtual-disk-and-volume"></a>创建虚拟磁盘和卷  
 现已创建一个池，下一步是分出一个虚拟磁盘并对其进行格式化。 这种情况下只会创建 1 个虚拟磁盘，且 New-Volume PowerShell cmdlet 可用于简化该过程：  
  
```  
New-Volume -StoragePool (Get-StoragePool -FriendlyName NVDIMM_Pool) -FriendlyName Log_Space -Size 300GB -FileSystem NTFS -AccessPath S: -ResiliencySettingName Mirror  
```  
  
 ![Windows Powershell 窗口的屏幕截图，其中显示了 New-Volume cmdlet 的输出。](../../relational-databases/performance/media/new-volume.png "New-Volume")  
  
 该虚拟磁盘已通过 NTFS 创建、初始化并格式化。 下面的屏幕截图显示其大小为 300GB，回写式缓存大小为 1GB（将托管于 NVDIMM-N 上）。  
  
 ![Windows Powershell 窗口的屏幕截图，其中显示了 Get-VirtualDisk cmdlet 的输出。](../../relational-databases/performance/media/get-virtualdisk.png "Get-VirtualDisk")  
  
 现在你可查看服务器中可见的这一新卷。 现在你可对 SQL Server 事务日志使用此驱动器。  
  
 ![文件资源管理器窗口中“本电脑”页面的屏幕截图，其中显示了 Log_Space 驱动器。](../../relational-databases/performance/media/log-space-drive.png "Log_Space Drive")  
  
## <a name="see-also"></a>另请参阅  
 [Windows 10 中的 Windows 存储空间](https://windows.microsoft.com/windows-10/storage-spaces-windows-10)   
 [Windows 2012 R2 中的 Windows 存储空间](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v=ws.11))   
 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [查看或更改数据文件和日志文件的默认位置 (SQL Server Management Studio)](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md)  
  
