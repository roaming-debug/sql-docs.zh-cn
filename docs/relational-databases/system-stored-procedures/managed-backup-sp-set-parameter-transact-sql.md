---
description: " (transact-sql 的 managed_backup.sp_set_parameter managed_backup) "
title: managed_backup managed_backup.sp_set_parameter (transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_set_parameter_TSQL
- sp_set_parameter
- smart_admin.sp_set_parameter
- smart_admin.sp_set_parameter_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_set_parameter
- smart_admin.sp_set_parameter
ms.assetid: bd8ae5fd-1337-4b7f-b0a4-153cbca9fa5f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a1f8722dd358a09103f3e75fd294eedb00019a01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205273"
---
# <a name="managed_backupsp_set_parameter-transact-sql"></a> (transact-sql 的 managed_backup.sp_set_parameter managed_backup) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  设置指定的智能管理系统参数的值。  
  
 可用的参数与 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 关联。 这些参数用于设置电子邮件通知、启用特定的扩展事件以及启用基于用户设置的策略的管理策略。 必须指定参数名称和参数值对。  

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sql  
EXEC managed_backup.sp_set_parameter   
    [@parameter_name = ] {'SSMBackup2WANotificationEmailIds' | 'SSMBackup2WAEnableUserDefinedPolicy' | 'SSMBackup2WADebugXevent' | 'FileRetentionDebugXevent' | 'StorageOperationDebugXevent'}  
    ,[@parameter_value = ] 'parameter_value'  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> 参数  
 @parameter_name  
 要设置值的参数的名称。 @parameter_name 为 NVARCHAR (128) 。 可用的参数名称为 **SSMBackup2WANotificationEmailIds**、 **SSMBackup2WADebugXevent**、 **SSMBackup2WAEnableUserDefinedPolicy**、 **FileRetentionDebugXevent** 和 **StorageOperationDebugXevent**。  
  
 @parameter_value  
 要设置的参数的值。 @parameter 值为 NVARCHAR (128) 。  允许下列参数名称/值对：  
  
-   @parameter_name = "SSMBackup2WANotificationEmailIds"： @parameter_value  = "email"  
  
-   @parameter_name = ' SSMBackup2WAEnableUserDefinedPolicy '： @parameter_value  = {' true ' |"false"}  
  
-   @parameter_name = ' SSMBackup2WADebugXevent '： @parameter_value  = {' true ' |"false"}  
  
-   @parameter_name = ' FileRetentionDebugXevent '： @parameter_value  = {' true ' |"false"}  
  
-   @parameter_name = ' StorageOperationDebugXevent ' = {' true ' |"false"}  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="best-practices"></a>最佳实践  
 可选部分，介绍用户在执行语句或例程时应了解的最佳实践。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 需要对 managed_backup 的 **sp_set_parameter** 存储过程的 **EXECUTE** 权限。  
  
## <a name="examples"></a>示例  
 以下示例启用操作并调试扩展事件。  
  
```  
-- to enable operational events  
Use msdb;  
Go  
         EXEC managed_backup.sp_set_parameter 'FileRetentionOperationalXevent', 'True'  
--  to enable debug events  
Use msdb;  
Go  
         EXEC managed_backup.sp_set_parameter 'FileRetentionDebugXevent', 'True'  
  
```  
  
 以下示例启用错误和警告的电子邮件通知，并设置用于将通知发送到的 emailID：  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_set_parameter @parameter_name = 'SSMBackup2WANotificationEmailIds', @parameter_value = '<email address>'  
  
```  
  
  
