---
description: “读取可重复”隔离级别的死锁
title: 具有读取可重复隔离级别的死锁 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- deadlocks in RDS [ADO]
- read repeatable in RDS [ADO]
ms.assetid: 29f3683f-12f3-4304-8a54-fe133c25a423
author: rothja
ms.author: jroth
ms.openlocfilehash: 05b43a72ede823a3a755edfdaa48be1e767b3e74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036497"
---
# <a name="deadlocks-with-read-repeatable-isolation-level"></a>“读取可重复”隔离级别的死锁
如果自定义业务对象使用可重复读取的隔离级别来访问 SQL Server，且在同一事务中发送查询和更新的两个客户端同时调用业务对象，则可能会发生死锁。 远程数据服务旨在允许其中一个进程超时以释放死锁，但该客户端的更新将失败。  
  
 使用 [Cursor 服务](../appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md)**命令 "超时** 动态" 属性来修改超时的长度。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="see-also"></a>另请参阅  
 [RDS 基础知识](./rds-fundamentals.md)