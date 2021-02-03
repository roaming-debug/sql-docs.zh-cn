---
description: MSSQLSERVER_41342
title: MSSQLSERVER_41342 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 41342 (Database Engine error)
ms.assetid: 28270d98-c543-4e7d-b40c-2200e38dce1c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 638f0758a28a7a2a03a9f9abcbbfb82181948548
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179563"
---
# <a name="mssqlserver_41342"></a>MSSQLSERVER_41342
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|事件 ID|41342|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|HK_HW_NOT_SUPPORTED|  
|消息正文|系统上的处理器型号不支持创建 *construct*。 较早的处理器通常会出现此错误。|  
  
## <a name="explanation"></a>说明  
内存优化的表要求处理器型号支持对 128 位值执行原子比较和交换操作，这要求装配说明 CMPXCHG16B。 一些较旧的 AMD 处理器型号不支持 CMPXCHG16B 指令。 此外，默认情况下，某些虚拟化环境不启用此指令。  
  
## <a name="user-action"></a>用户操作  
升级您的处理器。 如果处理器支持该指令并且正在虚拟机中运行 SQL Server，则更改配置以便支持指令 CMPXCHG16B。  
  
## <a name="see-also"></a>另请参阅  
[内存中 OLTP（内存中优化）](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
