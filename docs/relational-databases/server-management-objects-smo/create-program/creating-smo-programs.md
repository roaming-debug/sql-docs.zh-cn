---
description: 创建 SMO 程序
title: 正在创建 SMO 程序 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- Visual Basic [SMO]
- Visual C# [SMO]
- programming [SMO]
- SQL Server Management Objects, programming
- SMO [SQL Server], programming
ms.assetid: 7d2f0bcf-f1ae-45b8-bc3f-7aea4fae7e45
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 00ae0e27bb4d90c286ef1b199c200ab5b900f706
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467258"
---
# <a name="creating-smo-programs"></a>创建 SMO 程序
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理对象 (SMO) 对象的常规编程包括了所有对象共享的共有领域，例如运行方法、设置属性和操作集合。  
  
|主题|说明|  
|-----------|-----------------|  
|[连接到 SQL Server 实例](../../../relational-databases/server-management-objects-smo/create-program/connecting-to-an-instance-of-sql-server.md)|与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例建立连接的最简单 SMO 程序。 演示 Windows 身份验证和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证。 此外还包括了一些示例，演示如何连接到本地和远程 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。|  
|[断开与 SQL Server 实例的连接](../../../relational-databases/server-management-objects-smo/create-program/disconnecting-from-an-instance-of-sql-server.md)|一个演示如何断开与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的连接的程序。|  
|[调用方法](../../../relational-databases/server-management-objects-smo/create-program/calling-methods.md)|本节介绍调用方法的常见途径。 演示如何使用参数以及如何处理在 <xref:System.Data.DataTable> 对象中返回的数据的表。 还包括有关如何调用对象构造函数以及如何调用 **Clone** 方法的示例。|  
|[设置属性 - SMO](../../../relational-databases/server-management-objects-smo/create-program/setting-properties-smo.md)|本节介绍如何设置不同类型的属性。 演示如何设置和获取对象属性。 此外还包括一些示例，演示如何在创建对象时设置对象属性，以及如何遍历对象的所有属性。|  
|[使用集合](../../../relational-databases/server-management-objects-smo/create-program/using-collections.md)|各种讨论用于对象集合的方法的程序。 演示如何使用集合引用对象。 此外还包括一个示例，演示如何遍历集合成员。|  
|[处理 SMO 事件](../../../relational-databases/server-management-objects-smo/create-program/handling-smo-events.md)|本节介绍如何设置和处理 SMO 中的事件。 包含一个示例，演示如何设置事件处理程序和如何设置事件订阅。|  
|[处理 SMO 异常](../../../relational-databases/server-management-objects-smo/create-program/handling-smo-exceptions.md)|本节介绍如何捕获 SMO 中的异常。 包括一些示例，演示如何捕获异常以及如何显示内部异常。|  
|[使用数据类型](../../../relational-databases/server-management-objects-smo/create-program/working-with-data-types.md)|本节介绍如何使用不同的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。 介绍如何使用对象构造函数中的规范构造数据类型。 此外还包括一个示例，演示如何使用默认构造函数创建数据类型。|  
|[使用事务](../../../relational-databases/server-management-objects-smo/create-program/using-transactions.md)|本节介绍如何在 SMO 中实现事务处理。 包括一个示例，演示如何在 SMO 程序中使用事务。|  
|[使用捕获模式](../../../relational-databases/server-management-objects-smo/create-program/using-capture-mode.md)|本节介绍如何记录 SMO 程序的输出。 该示例将 SMO 程序记录为发送到 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 实例的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 语句以便稍后执行。|  
  
  
