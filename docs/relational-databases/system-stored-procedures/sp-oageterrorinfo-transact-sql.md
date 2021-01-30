---
description: sp_OAGetErrorInfo (Transact-SQL)
title: sp_OAGetErrorInfo (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OAGetErrorInfo_TSQL
- sp_OAGetErrorInfo
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAGetErrorInfo
ms.assetid: ceecea08-456f-4819-85d9-ecc9647d7187
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d55126c7feb362b910e83034a66e4d691f1be84c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191975"
---
# <a name="sp_oageterrorinfo-transact-sql"></a>sp_OAGetErrorInfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  获取 OLE 自动化错误信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_OAGetErrorInfo [ objecttoken ]  
    [ , source OUTPUT ]   
    [ , description OUTPUT ]   
    [ , helpfile OUTPUT ]   
    [ , helpid OUTPUT ]   
```  
  
## <a name="arguments"></a>参数  
 *objecttoken*  
 是先前使用 **sp_OACreate** 创建的 OLE 对象的对象标记，或为 NULL。 如果指定了 *objecttoken* ，则返回该对象的错误消息。 如果指定为 NULL，则返回整个批处理的错误信息。  
  
 _源_**输出**  
 错误信息的源。 如果指定，则它必须是本地 **char**、 **nchar**、 **varchar** 或 **nvarchar** 变量。 必要时将截断返回值以适合局部变量的要求。  
  
 _说明_**输出**  
 错误的说明。 如果指定，则它必须是本地 **char**、 **nchar**、 **varchar** 或 **nvarchar** 变量。 必要时将截断返回值以适合局部变量的要求。  
  
 _帮助_ 的 **输出**  
 OLE 对象的帮助文件。 如果指定，则它必须是本地 **char**、 **nchar**、 **varchar** 或 **nvarchar** 变量。 必要时将截断返回值以适合局部变量的要求。  
  
 _helpid_ **输出**  
 帮助文件的上下文 ID。 如果指定，则它必须为本地 **int** 变量。  
  
> [!NOTE]  
>  此存储过程的参数按位置（而不是按名称）指定。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或非零数字（失败），是由 OLE 自动化对象返回的 HRESULT 整数值。  
  
 有关 HRESULT 返回代码的详细信息，请参阅 [OLE 自动化返回代码和错误信息](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md)。  
  
## <a name="result-sets"></a>结果集  
 如果未指定输出参数，错误信息将作为结果集返回给客户端。  
  
|列名|数据类型|说明|  
|------------------|---------------|-----------------|  
|**错误**|**二进制 (4)**|错误号的二进制表示形式。|  
|**Source**|**nvarchar (nn)**|错误的源。|  
|**说明**|**nvarchar (nn)**|错误的说明。|  
|**帮助**|**nvarchar (nn)**|错误源的帮助文件。|  
|**HelpID**|**int**|错误源帮助文件中的帮助上下文 ID。|  
  
## <a name="remarks"></a>备注  
 对 OLE 自动化存储过程的每个调用 (除 **sp_OAGetErrorInfo**) 重置错误信息;因此， **sp_OAGetErrorInfo** 仅获取最新 OLE 自动化存储过程调用的错误信息。 请注意，因为 **sp_OAGetErrorInfo** 不会重置错误信息，所以可多次调用它来获取相同的错误信息。  
  
 下表中列出了 OLE 自动化错误及其常见原因。  
  
|错误及 HRESULT|常见原因|  
|-----------------------|------------------|  
|**变量类型错误 (0x80020008)**|[!INCLUDE[tsql](../../includes/tsql-md.md)]作为方法参数传递的值的数据类型与该方法参数的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 数据类型不匹配，或者 NULL 值作为方法参数传递。|  
|**未知名称 (0x8002006)**|找不到指定对象的指定属性名或方法名。|  
|**类字符串无效 (0x800401f3)**|没有将指定的 ProgID 或 CLSID 没有注册为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的 OLE 对象。 自定义 OLE 自动化服务器必须先进行注册，然后才能使用 **sp_OACreate** 对其进行实例化。 为此，可以使用用于进程内 ( .dll) 服务器的 Regsvr32.exe 实用工具，或本地 () 服务器的 **/REGSERVER** 命令行开关。|  
|**服务器执行失败 (0x80080005)**|指定的 OLE 对象已注册为本地 OLE 服务器（.exe 文件），但无法找到或启动该 .exe 文件。|  
|**无法找到指定的模块 (0x8007007e)**|指定的 OLE 对象已注册为进程内 OLE 服务器（.dll 文件），但无法找到或半截该 .dll 文件。|  
|**类型不匹配 (0x80020005)**|用于存储返回的属性值或者方法返回值的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 局部变量的数据类型与属性或方法返回值的 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 数据类型不匹配。 或者，要求属性或方法返回值，但该属性或方法未返回值。|  
|**Sp_OACreate 的 "context" 参数的数据类型或值无效。 (0x8004275B)**|上下文参数值应为下列值之一：1、4 或 5。|  
  
 有关处理 HRESULT 返回代码的详细信息，请参阅 [OLE 自动化返回代码和错误信息](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md)。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份或直接对此存储过程执行权限。 `Ole Automation Procedures` 必须 **启用** 配置才能使用与 OLE 自动化相关的任何系统过程。  
  
## <a name="examples"></a>示例  
 以下示例将显示 OLE 自动化错误信息。  
  
```  
DECLARE @output varchar(255);  
DECLARE @hr int;  
DECLARE @source varchar(255);  
DECLARE @description varchar(255);  
PRINT 'OLE Automation Error Information';  
EXEC @hr = sp_OAGetErrorInfo @object, @source OUT, @description OUT;  
IF @hr = 0  
BEGIN  
    SELECT @output = '  Source: ' + @source  
    PRINT @output  
    SELECT @output = '  Description: ' + @description  
    PRINT @output  
END  
ELSE  
BEGIN  
    PRINT '  sp_OAGetErrorInfo failed.'  
    RETURN  
END;  
```  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 OLE 自动化存储过程 ](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [OLE 自动化脚本示例](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
