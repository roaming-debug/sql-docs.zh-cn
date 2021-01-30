---
description: sp_OACreate (Transact-SQL)
title: sp_OACreate (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OACreate
- sp_OACreate_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OACreate
ms.assetid: eb84c0f1-26dd-48f9-9368-13ee4a30a27c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bd6d44aedf1328234cf51366c3e81bcfb2d6dc94
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191987"
---
# <a name="sp_oacreate-transact-sql"></a>sp_OACreate (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  创建 OLE 对象的实例。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_OACreate { progid | clsid } , objecttoken OUTPUT [ , context ]   
```  
  
## <a name="arguments"></a>参数  
 *progid*  
 要创建的 OLE 对象的编程标识符 (ProgID)。 此字符串描述 OLE 对象的类，其格式为： **'**_OLEComponent_**。**_对象_**'**  
  
 *OLEComponent* 是 ole 自动化服务器的组件名称， *Object* 是 ole 对象的名称。 指定的 OLE 对象必须有效并且必须支持 **IDispatch** 接口。  
  
 例如，SQLDMO。SQLServer 是 sql-dmo **SQLServer** 对象的 ProgID。 SQL-DMO 的组件名称为 SQLDMO， **sqlserver** 对象有效， (类似于 **SQLServer** 对象) 支持 **IDispatch** 的所有 sql-dmo 对象。  
  
 *clsid*  
 要创建的 OLE 对象的类标识符 (CLSID)。 此字符串描述了 OLE 对象的类，其格式为： **"{**_nnnnnnnn，nnnnnnnnnnnn_**}"**。 指定的 OLE 对象必须有效并且必须支持 **IDispatch** 接口。  
  
 例如，{00026BA1-0000-0000-C000-000000000046} 是 SQL-DMO **SQLServer** 对象的 CLSID。  
  
 _objecttoken_ **输出**  
 返回的对象标记，必须是数据类型为 **int** 的局部变量。此对象标记标识创建的 OLE 对象，并用于对其他 OLE 自动化存储过程的调用。  
  
 *上下文*  
 指定要运行新创建的 OLE 对象的执行上下文。 如果指定，则该值必须为下列值之一：  
  
 **1** = 仅) OLE 服务器的进程内 (。  
  
 **4** = 本地 ( .exe) 仅 OLE 服务器。  
  
 **5** = 同时允许进程内和本地 OLE 服务器  
  
 如果未指定，则默认值为 **5**。 此值作为对 **CoCreateInstance** 的调用的 *dwClsContext* 参数进行传递。  
  
 如果允许使用进程内 OLE 服务器 (通过使用上下文值 **1** 或 **5** ，或者不指定上下文值) ，则它可以访问拥有的内存和其他资源 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 进程内 OLE 服务器可能会破坏 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的内存或资源并导致不可预知的结果，如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 访问违规。  
  
 当你指定的上下文值为 **4** 时，本地 OLE 服务器无权访问任何 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源，并且不会损坏 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存或资源。  
  
> [!NOTE]  
>  此存储过程的参数按位置指定，而不是按名称指定。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或非零数字（失败），是由 OLE 自动化对象返回的 HRESULT 整数值。  
  
 有关 HRESULT 返回代码的详细信息，请参阅 [OLE 自动化返回代码和错误信息](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md)。  
  
## <a name="remarks"></a>备注  
 如果启用 OLE 自动化过程，则对 **sp_OACreate** 的调用将启动 OLE 自动化共享执行环境。 有关启用 OLE 自动化的详细信息，请参阅 [Ole Automation 过程服务器配置选项](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)。  
  
 已创建的 OLE 对象在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句批处理结束时会自动破坏。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份或直接对此存储过程执行权限。 `Ole Automation Procedures` 必须 **启用** 配置才能使用与 OLE 自动化相关的任何系统过程。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-progid"></a>A. 使用 ProgID  
 下面的示例通过使用 sql-dmo **SQLServer** 对象的 ProgID 来创建它。  
  
```  
DECLARE @object int;  
DECLARE @hr int;  
DECLARE @src varchar(255), @desc varchar(255);  
EXEC @hr = sp_OACreate 'SQLDMO.SQLServer', @object OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object, @src OUT, @desc OUT   
   raiserror('Error Creating COM Component 0x%x, %s, %s',16,1, @hr, @src, @desc)  
    RETURN  
END;  
GO  
```  
  
### <a name="b-using-clsid"></a>B. 使用 CLSID  
 下面的示例通过使用 sql-dmo **SQLServer** 对象的 CLSID 来创建它。  
  
```  
DECLARE @object int;  
DECLARE @hr int;  
DECLARE @src varchar(255), @desc varchar(255);  
EXEC @hr = sp_OACreate '{00026BA1-0000-0000-C000-000000000046}',  
    @object OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object, @src OUT, @desc OUT   
   raiserror('Error Creating COM Component 0x%x, %s, %s',16,1, @hr, @src, @desc)  
    RETURN  
END;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 OLE 自动化存储过程 ](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Ole 自动化过程服务器配置选项](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)   
 [OLE 自动化脚本示例](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
