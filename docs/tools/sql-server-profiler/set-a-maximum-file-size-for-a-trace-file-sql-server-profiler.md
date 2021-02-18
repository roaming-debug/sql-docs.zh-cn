---
title: 设置跟踪文件的最大文件大小
titleSuffix: SQL Server Profiler
description: 了解如何在 SQL Server Profiler 中限制跟踪文件的大小，以及如何指定该文件达到最大大小时是否滚动更新。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: e86dc4ce-5aa3-4c0d-acb5-c9e8871ed963
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 094df80b40338af09e7c4d9084ec3715a91a61c5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345798"
---
# <a name="set-a-maximum-file-size-for-a-trace-file-sql-server-profiler"></a>设置跟踪文件的最大文件大小 (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用以下步骤可设置跟踪文件的最大文件大小。  
  
### <a name="to-set-a-maximum-file-size-for-a-trace-file"></a>设置跟踪文件的最大文件大小  
  
1.  在 **“文件”** 菜单上，单击 **“新建跟踪”** ，然后连接到 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例。  
  
     将出现“跟踪属性”对话框。 **“跟踪属性”** 对话框。  
  
    > [!NOTE]  
    >  如果选择“建立连接后立即开始跟踪”，则不会显示“跟踪属性”对话框，而是开始跟踪。 若要关闭此设置，请在“工具”菜单上，单击“选项”，然后清除“建立连接后立即开始跟踪”复选框。  
  
2.  在 **“跟踪名称”** 框中，键入跟踪的名称。  
  
3.  在“模板名称”列表中，选择跟踪模板。  
  
4.  选择 **“保存到文件”** ，然后指定存储跟踪信息的文件。  
  
5.  在 **“设置最大文件大小”** 复选框中，指定跟踪的最大文件大小。 当文件大小达到此最大值时，跟踪文件就不再记录到此文件中。 如果选中“启用文件滚动更新”（默认情况下选中此选项），则出现以下情况：  
  
     如果使用文件滚动更新选项，则在达到最大文件大小时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会关闭当前文件并创建一个新文件。 新文件的名称与上一个文件相同，但是在其名称后追加了一个指明顺序的整数。例如，如果原始跟踪文件命名为 filename_1.trc，则下一个跟踪文件就为 filename_2.trc，依此类推。 如果分配给新的滚动更新文件的名称已经被现有文件使用，则现有文件将被覆盖，除非它是只读的。 将跟踪数据保存到文件时，默认情况下启用文件滚动选项。  
  
     如果启用了文件滚动更新选项，则在使用其他某种方法停止跟踪之前，跟踪将一直继续。 若要停止在达到文件大小限制后进行跟踪，请禁用文件滚动更新选项。  
  
    > [!NOTE]  
    >  FAT32 文件系统将文件大小限制为略微小于 4 GB。 当跟踪文件达到该大小时，跟踪将会失败，并出现错误“磁盘空间不足”。 若要创建更大的文件，请使用 NTFS 文件系统。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
