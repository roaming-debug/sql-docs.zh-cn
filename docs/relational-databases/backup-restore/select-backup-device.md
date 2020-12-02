---
title: 选择备份设备 | Microsoft Docs
description: 对于 SQL Server 还原，使用“选择备份设备”对话框来选择用于还原操作的逻辑备份设备。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.selectbackupdevice.f1
ms.assetid: 7887c9fd-15ce-4cc8-b069-845c1d09088c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3eb2dc44dde731d77ecea441532d45f8b7fa8b39
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125493"
---
# <a name="select-backup-device"></a>选择备份设备
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用 **“选择备份设备”** 对话框可以选择还原操作使用的逻辑备份设备。  
  
 逻辑备份设备是与物理设备对应的用户定义的逻辑设备，它是操作系统提供的磁带机或磁盘驱动器。  
  
> [!NOTE]  
>  如果备份使用多个备份设备，这些设备必须与单一类型的设备相对应。  
  
 **使用 SQL Server Management Studio 查看备份设备的内容**  
  
-   [查看备份磁带或文件的内容 (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
## <a name="options"></a>选项  
 **备份设备**  
 在该列表框中，选择要从中进行还原的逻辑备份设备的名称。  
  
 有关如何查看备份设备内容的信息，请参阅 [查看逻辑备份设备的属性和内容 (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)。  
  
## <a name="remarks"></a>备注  
 如果您在列表中看不到包含所查找备份的逻辑备份设备，则备份可能已直接写入一个或多个文件或磁带机中。 在此情况下，请取消 **“选择备份设备”** 对话框；并在 **“指定备份”** 对话框中的 **“备份介质”** 列表框中选择 **“文件”** 或 **“磁带”** 。  
  
## <a name="see-also"></a>另请参阅  
 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)  
  
  
