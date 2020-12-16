---
title: Transact-SQL 中的 OLE 自动化对象 | Microsoft Docs
description: 了解通过存储过程执行的 OLE 自动化对象如何在 SQL Server 数据库引擎实例的地址空间中运行。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
helpviewer_keywords:
- triggers [SQL Server], OLE Automation
- batches [SQL Server], OLE Automation
- OLE Automation [SQL Server]
- OLE Automation [SQL Server], about OLE Automation
ms.assetid: a887d956-4cd0-400a-aa96-00d7abd7c44b
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1e20fc91b1276a2a7bd8e263c18d6a9da1786ba9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482948"
---
# <a name="ole-automation-objects-in-transact-sql"></a>Transact-SQL 中的 OLE 自动化对象
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  [!INCLUDE[tsql](../../includes/tsql-md.md)] 包括一些系统存储过程，这些存储过程允许在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理、存储过程和触发器中引用 OLE 自动化对象。 这些系统存储过程作为扩展存储过程运行，而且通过存储过程执行的 OLE 自动化对象在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例的地址空间中的运行方式与扩展存储过程的运行方式相同。  
  
 OLE 自动化存储过程使 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理能够引用 SQL-DMO 对象和自定义 OLE 自动化对象，例如公开 **IDispatch** 接口的对象。 使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 创建的自定义进程内 OLE 服务器必须有一个用于“Class_Initialize”和“Class_Terminate”子例程的错误处理程序（使用“On Error GoTo”语句指定） 。 **Class_Initialize** 子例程和 **Class_Terminate** 子例程中未处理的错误可能导致不可预知的错误，例如 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的访问冲突。 建议其他子例程也有错误处理程序。  
  
 当使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中的 OLE 自动化对象时的第一个步骤就是调用 **sp_OACreate** 系统存储过程在 [!INCLUDE[ssDE](../../includes/ssde-md.md)]实例的地址空间中创建对象实例。  
  
 创建了对象实例后，调用下列存储过程以使用与该对象相关的属性、方法和错误信息：  
  
-   **sp_OAGetProperty** ，获取属性值。  
  
-   **sp_OASetProperty** ，设置属性值。  
  
-   **sp_OAMethod** ，调用方法。  
  
-   **sp_OAGetErrorInfo** ，获取最新的错误信息。  
  
 当不再需要对象时，调用 **sp_OADestroy** 释放使用 **sp_OACreate** 创建的对象实例。  
  
 OLE 自动化对象通过属性值和方法返回数据。 **sp_OAGetProperty** 和 **sp_OAMethod** 以结果集的形式返回这些数据值。  
  
 OLE 自动化对象的作用域是一个批处理。 对该对象的所有引用都必须包含在单个批处理、存储过程或触发器中。  
  
 引用对象时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动化对象支持从引用的对象遍历至它包含的其他对象。 例如，在使用 SQL-DMO **SQLServer** 对象时，可以引用该服务器上包含的数据库和表。  
  
## <a name="related-content"></a>相关内容  
 [对象层次结构语法 (Transact-SQL)](../../relational-databases/system-stored-procedures/object-hierarchy-syntax-transact-sql.md)  
  
 [外围应用配置器](../../relational-databases/security/surface-area-configuration.md)  
  
 [Ole Automation Procedures 服务器配置选项](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)  
  
 [sp_OACreate (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-oacreate-transact-sql.md)  
  
 [sp_OAGetProperty (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-oagetproperty-transact-sql.md)  
  
 [sp_OASetProperty (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-oasetproperty-transact-sql.md)  
  
 [sp_OAMethod (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-oamethod-transact-sql.md)  
  
 [sp_OAGetErrorInfo (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-oageterrorinfo-transact-sql.md)  
  
 [sp_OADestroy (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-oadestroy-transact-sql.md)  
  
  
