---
description: MSSQLSERVER_2533
title: MSSQLSERVER_2533 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2533 (Database Engine error)
ms.assetid: 0418352c-0ab2-4dc7-b8b9-5c3bad94560c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9001611265f16f0afac6de6f42e18cfb7f964357
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188950"
---
# <a name="mssqlserver_2533"></a>MSSQLSERVER_2533
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|2533|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|DBCC_PAGE_WAS_NOT_SEEN|  
|消息正文|表错误:看不到分配给对象 ID O_ID、索引 ID I_ID、分区 ID PN_ID、分配单元 ID A_ID（类型为 TYPE）的页 P_ID。 该页可能无效，或者页头中可能包含错误的分配单元 ID。|  
  
## <a name="explanation"></a>说明  
页已分配给分配单元 ID *A_ID*，但此分配单元 ID 不在页头中。 页头具有其他的分配单元 ID。 如果页头中的分配单元 ID 用于有效的对象，则该页可能具有匹配的 MSSQLEngine_2534 错误。  
  
## <a name="user-action"></a>用户操作  
  
### <a name="look-for-hardware-failure"></a>查找硬件故障  
运行硬件诊断并更正任何问题。 也可以通过检查 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 系统和应用程序日志以及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志来查看是否存在由硬件故障导致的错误。 修复日志中包含的所有与硬件相关的问题。  
  
如果持续遇到数据损坏问题，请尝试分别换下不同的硬件组件以确定问题所在。 进行检查以确保系统未启用磁盘控制器上的写缓存。 如果怀疑写入缓存是问题起因，请与硬件供应商联系。  
  
最后，您可能会发现，切换到全新的硬件系统是解决问题的极佳途径。 此切换操作可能包括重新格式化磁盘驱动器和重新安装操作系统。  
  
### <a name="restore-from-backup"></a>从备份还原  
如果出现的问题与硬件无关，并且您确信有可用的干净备份，请从备份中还原数据库。  
  
### <a name="run-dbcc-checkdb"></a>运行 DBCC CHECKDB  
如果不存在干净的可用备份，请运行不带 REPAIR 子句的 DBCC CHECKDB 以确定损坏程度。 建议使用 DBCC CHECKDB 的 REPAIR 子句。 接下来，请运行带适当 REPAIR 子句的 DBCC CHECKDB 修复损坏的数据。  
  
> [!CAUTION]  
> 如果您不确定运行带有 REPAIR 子句的 DBCC CHECKDB 会对数据造成何种影响，请在运行该语句前与您的主要支持提供商联系。  
  
如果运行具有 REPAIR 子句的 DBCC CHECKDB 无法解决存在的问题，请与主要支持提供商联系。  
  
### <a name="results-of-running-repair-options"></a>运行 REPAIR 选项的结果  
REPAIR 将释放页。 如果页来自行内数据分配单元，则将重新生成索引（如果存在索引）。  
  
> [!CAUTION]  
> 此修复可能会导致数据丢失。  
  
## <a name="see-also"></a>另请参阅  
[MSSQLSERVER_2534](~/relational-databases/errors-events/mssqlserver-2534-database-engine-error.md)  
  
