---
title: 还原数据库（“文件”页）| Microsoft Docs
description: 在 SQL Server 中还原数据库时，使用“还原数据库”对话框的“文件”页可管理数据库中要还原的特定文件。
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restoredb.files.f1
- sql13.swb.restoredb.files.f1 in the code
ms.assetid: 714c36ea-a9f9-43a4-99f9-a6f73d1baf8e
author: cawrites
ms.author: chadam
ms.openlocfilehash: d94a143eae321f7ea01f97a54b351d72f8142ad6
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129150"
---
# <a name="restore-database-files-page"></a>还原数据库（“文件”页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用 **“还原数据库”** 对话框的 **“文件”** 页可以管理数据库中您选择要还原的特定文件。  
  
## <a name="options"></a>选项  
  
### <a name="restore-database-files-as"></a>将数据库文件还原为  
 用来向还原的文件分配新文件路径并进行管理。  
  
 **将所有文件重新定位到文件夹**  
 重新定位还原的文件。  
  
|选项|说明|  
|------------|-----------------|  
|**数据文件的文件夹**|输入或搜索应将还原的数据文件重新定位到的数据文件的文件夹名称。|  
|**日志文件的文件夹**|输入或搜索应将还原的日志文件文件重新定位到的日志文件的文件夹。|  
  
 **逻辑文件名**  
 对于每个要还原的数据库文件显示一行。  
  
 **文件类型**  
 显示文件类型。  
  
 **原始文件名**  
 显示已还原文件的原始文件路径。  
  
 **还原为**  
 列出用于另存还原文件的文件名。 输入或搜索适当的文件名。  
  
## <a name="see-also"></a>另请参阅  
 [还原数据库（“常规”页）](../../relational-databases/backup-restore/restore-database-general-page.md)   
 [还原数据库（“选项”页）](../../relational-databases/backup-restore/restore-database-options-page.md)   
 [RESTORE 参数 (Transact-SQL)](../../t-sql/statements/restore-statements-arguments-transact-sql.md)   
 [为磁带驱动器定义逻辑备份设备 (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
