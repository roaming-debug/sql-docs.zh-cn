---
description: xp_logevent (Transact-SQL)
title: xp_logevent (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_logevent
- xp_logevent_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- xp_logevent
ms.assetid: 7b379ad0-5b12-4d2e-9c52-62465df1fdbd
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2a3a1aa4f71f05860975e9eadc60fa49c65a1951
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99124621"
---
# <a name="xp_logevent-transact-sql"></a>xp_logevent (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将用户定义的消息记录到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日志文件和 Windows 事件查看器中。 xp_logevent 可用于发送警报，而无需向客户端发送消息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
xp_logevent { error_number , 'message' } [ , 'severity' ]  
```  
  
## <a name="arguments"></a>参数  
 error_number  
 用户定义错误号，该值大于 50,000。 最大值为 2147483647 (2^31 - 1)。  
  
 **"** *message* **"**  
 最多 2048 个字符的字符串。  
  
 **"** *严重性* **"**  
 以下三个字符串之一：INFORMATIONAL、WARNING 或 ERROR。 *严重性* 为可选，默认值为 "信息"。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 对于所包含的代码示例，xp_logevent 返回下列错误消息：  
  
 `The command(s) completed successfully.`  
  
## <a name="remarks"></a>备注  
 从 [!INCLUDE[tsql](../../includes/tsql-md.md)] 过程、触发器、批处理等发送消息时，请使用 RAISERROR 语句而不是 xp_logevent。 xp_logevent 不会调用客户端的消息处理程序，也不设置 @ @ERROR 。 若要将消息写入 Windows 事件查看器以及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志文件中，请执行 RAISERROR 语句。  
  
## <a name="permissions"></a>权限  
 需要 master 数据库中的 db_owner 固定数据库角色的成员身份，或者 sysadmin 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 以下示例将消息以及传递给消息的变量记录到 Windows 事件查看器中。  
  
```  
DECLARE @@TABNAME varchar(30), @@USERNAME varchar(30), @@MESSAGE varchar(255);  
SET @@TABNAME = 'customers';  
SET @@USERNAME = USER_NAME();  
SELECT @@MESSAGE = 'The table ' + @@TABNAME + ' is not owned by the user   
   ' + @@USERNAME + '.';  
  
USE master;  
EXEC xp_logevent 60000, @@MESSAGE, informational;  
```  
  
## <a name="see-also"></a>另请参阅  
 [PRINT (Transact-SQL)](../../t-sql/language-elements/print-transact-sql.md)   
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [&#40;Transact-sql&#41;的常规扩展存储过程 ](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)  
  
  
