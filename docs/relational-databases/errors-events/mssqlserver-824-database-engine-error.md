---
description: MSSQLSERVER_824
title: MSSQLSERVER_824 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 824 (Database Engine error)
ms.assetid: 2aa22246-2712-4fdb-9744-36e7e6f3175e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 031a478a281538b096d0900ad80fa34741182269
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180301"
---
# <a name="mssqlserver_824"></a>MSSQLSERVER_824
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|824|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|B_HARDSSERR|  
|消息正文|SQL Server 检测到基于一致性的逻辑 I/O 错误: %ls。 在文件 '%ls' 中、偏移量为 %#016I64x 的位置对数据库 ID %d 中的页 %S_PGID 执行 %S_MSG 期间，发生了该错误。  SQL Server 错误日志或系统事件日志中的其他消息可能提供了更详细信息。|  
  
## <a name="symptom"></a>症状  


如果读取或写入数据库页后逻辑一致性检查失败，你可能会在 SQL Server 错误日志或 Windows 应用程序事件日志中看到以下错误消息：
 
``` 
2009-11-02 15:46:42.90 spid51      Error: 824, Severity: 24, State: 2.
2009-11-02 15:46:42.90 spid51      SQL Server detected a logical consistency-based I/O error: incorrect pageid (expected 1:43686; actual 0:0). It occurred during a read of page (1:43686) in database ID 23 at offset 0x0000001554c000 in file 'H:\MSSQL.SQL2008\MSSQL\DATA\my_db.mdf'.  Additional messages in the SQL Server error log or system event log may provide more detail. This is a severe error condition that threatens database integrity and must be corrected immediately. Complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.
```
 
如果应用程序在查询或修改数据时遇到此消息，则会将错误消息返回给应用程序并终止数据库连接。 
  
## <a name="cause"></a>原因
此错误表明 Windows 报告已从磁盘成功读取页，但 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 检测到页中存在错误。 此错误与错误 823 类似，只是 Windows 检测不到此错误。此错误通常表示 I/O 子系统出现了问题，如磁盘驱动器故障、磁盘固件问题、设备驱动程序故障等。 有关 I/O 错误的详细信息，请参阅 [Microsoft SQL Server I/O 基础知识，第 2 章](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10))。  

SQL Server 使用 Windows API（例如 ReadFile、WriteFile、ReadFileScatter、WriteFileGather）执行 I/O 操作。 执行这些 I/O 操作后，SQL Server 检查所有与这些 API 调用相关联的错误条件。 如果这些 API 调用因操作系统错误而失败，则 SQL Server 报告错误 823。 在某些情况下，Windows API 调用确实成功了，但 I/O 操作传输的数据可能已经遇到逻辑一致性问题。 这些逻辑一致性问题通过错误 824 报告。
 
824 错误包含下列信息：

- I/O 操作面向的数据库文件
- 在其中尝试执行 I/O 操作的文件的偏移
- 此文件所属的数据库
- I/O 操作中涉及的页码
- 操作是读取还是写入操作
- 有关失败的逻辑一致性检查的详细信息（用于此检查的检查类型、实际值和预期值）
 
这些逻辑一致性检查是 SQL Server 执行的附加完整性检查，用于确保在整个 I/O 操作期间维护数据传输中涉及的数据的某些关键方面。 检查内容包括校验和、残缺页、短传输、页面 ID 错误、读取过时、页面审核失败。 根据数据库和服务器级别的不同配置选项，执行的检查的性质有所不同。 
 
824 错误消息通常指示基础存储系统或硬件出现问题，或 I/O 请求路径中的驱动程序出现问题。 如果文件系统中存在不一致，或者数据库文件已损坏，则可能遇到此错误。

## <a name="resolution"></a>解决方法  

如果遇到错误 824，可以尝试以下解决方法： 

- 查看 msdb 中的 [suspect_pages](../backup-restore/manage-the-suspect-pages-table-sql-server.md) 表，检查（同一数据库或不同数据库中的）其他页是否遇到此问题。
- 使用 DBCC CHECKDB 命令检查位于同一卷（824 消息中所报告的卷）上的数据库的一致性。 如果发现 DBCC CHECKDB 命令不一致，请使用知识库文章[如何解决 DBCC CHECKB 报告的数据库一致性错误](https://support.microsoft.com/help/2015748/how-to-troubleshoot-database-consistency-errors-reported-by-dbcc-check)中的指南。
- 如果遇到这些 824 错误的数据库没有启用“PAGE_VERIFY CHECKSUM”数据库选项，请立即执行此操作。 824 错误可能由于校验和失败以外的其他原因发生，但 CHECKSUM 提供了在页写入磁盘后验证其一致性的最佳选项。
- 查看 Windows 事件日志中是否存在从操作系统或存储设备/设备驱动程序中报告的错误或消息。 如果它们与此错误有某种关联，请先解决这些错误。 例如，除了 824 消息以外，你还可能会注意到事件日志中的磁盘源报告的“驱动程序检测到 \Device\Harddisk4\DR4 上的控制器错误”之类的事件。 在这种情况下，你须评估此设备上是否存在此文件，然后先更正这些磁盘错误。
- 使用 [SQLIOSim](https://support.microsoft.com/help/231619/how-to-use-the-sqliosim-utility-to-simulate-sql-server-activity-on-a-d) 实用工具确定这些 824 错误是否可在常规 SQL Server I/O 请求之外重现。 SQLIOSim 随 SQL Server 2008 进行装配，因此不需要在此版本或更高版本上单独下载。
- 与硬件供应商或设备制造商合作，确保：
   - 硬件设备和配置符合 [SQL Server 的 I/O 要求](https://support.microsoft.com/help/967576/microsoft-sql-server-database-engine-input-output-requirements)。
   - I/O 路径中所有设备的设备驱动程序和其他支持软件组件都是最新的。
- 如果硬件供应商或设备制造商为你提供了一些诊断实用工具，请用这些实用工具评估 I/O 系统的运行状况。
- 评估在遇到问题的这些 I/O 请求的路径中是否存在筛选器驱动程序。
   - 检查这些筛选器驱动程序是否有任何更新
   - 能否删除或禁用这些筛选器驱动程序，以观察导致 824 错误的问题是否消失
- 如果出现的问题与硬件无关，并且已知的干净备份可用，则请从备份中还原数据库。  

## <a name="see-also"></a>另请参阅  
[管理 suspect_pages 表 (SQL Server)](~/relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
