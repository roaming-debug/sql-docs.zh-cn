---
description: sp_OAMethod (Transact-SQL)
title: sp_OAMethod (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_OAMethod
- sp_OAMethod_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAMethod
ms.assetid: 1dfaebe2-c7cf-4041-a586-5d04faf2e25e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: da84a59353baffa92ac0fa30e55bfa4b248379f7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182409"
---
# <a name="sp_oamethod-transact-sql"></a>sp_OAMethod (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  调用一个 OLE 对象的方法。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_OAMethod objecttoken , methodname  
    [ , returnvalue OUTPUT ]   
    [ , [ @parametername = ] parameter [ OUTPUT ] [ ...n ] ]   
```  
  
## <a name="arguments"></a>参数  
 *objecttoken*  
 是先前使用 **sp_OACreate** 创建的 OLE 对象的对象标记。  
  
 *名称*  
 要调用的 OLE 对象的方法名。  
  
 _returnvalue_  **输出**  
 OLE 对象的方法的返回值。 如果指定此参数，则必须是相应数据类型的局部变量。  
  
 如果该方法返回单个值，请为 *returnvalue* 指定一个局部变量，该局部变量返回局部变量中的方法返回值，或不指定 *returnvalue*，这会将方法返回值作为单列单行结果集返回给客户端。  
  
 如果方法返回值是 OLE 对象，则 *returnvalue* 必须是数据类型为 **int** 的局部变量。对象标记存储在局部变量中，此对象标记可用于其他 OLE 自动化存储过程。  
  
 当方法返回值为数组时，如果指定了 *returnvalue* ，则将其设置为 NULL。  
  
 如果出现下列任意一种情况，则产生错误：  
  
-   已指定 *returnvalue* ，但该方法不返回值。  
  
-   方法返回二维以上的数组。  
  
-   方法返回一个数组作为输出参数。  
  
`[ _@parametername = ] parameter[ OUTPUT ]` 为方法参数。 如果已指定，则 *参数* 必须为适当数据类型的值。  
  
 若要获取 output 参数的返回值， *参数* 必须是相应数据类型的局部变量，并且必须指定 **output** 。 如果指定了常量参数，或未指定 **output** ，则将忽略输出参数的任何返回值。  
  
 如果指定，则 *parametername* 必须是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 命名参数的名称。 请注意， **@** _parametername_is 不是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 局部变量。 删除 at 符号 (**@**) ，并将 *parametername* 作为参数名传递给 OLE 对象。 指定了所有位置参数后，才能指定命名参数。  
  
 *n*  
 指示可以指定多个参数的占位符。  
  
> [!NOTE]
>  *\@ parametername* 可以是命名参数，因为它是指定方法的一部分并且传递给对象。 此存储过程的其他参数是按位置（而不是名称）指定的。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或非零数字（失败），是由 OLE 自动化对象返回的 HRESULT 整数值。  
  
 有关 HRESULT 返回代码的详细信息， [OLE 自动化返回代码和错误信息](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md)。  
  
## <a name="result-sets"></a>结果集  
 如果方法返回值是一维或二维数组，那么该数组将作为结果集返回给客户端：  
  
-   一维数组作为单行结果集返回给客户端，其中的列数与数组中的元素数相等。 换言之，该数组以（列）的形式返回。  
  
-   二维数组作为结果集返回给客户端，其中的列数与数组第一维中的元素数相同，行数与数组第二维中的元素数相同。 换言之，该数组以（列、行）的形式返回。  
  
 当属性返回值或方法返回值为数组时， **sp_OAGetProperty** 或 **sp_OAMethod** 会向客户端返回结果集。 （方法输出参数不能是数组。这些过程可扫描数组中的所有数据值，以确定用于结果集中各列的相应 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型和数据长度。 对于某个特定的列，这些过程将使用显示该列中的所有数据值所需要的数据类型和长度。  
  
 如果一列中的所有数据值具有相同的数据类型，此数据类型将用于整个列。 当列中的数据值为其他数据类型时，将基于下表选择整个列的数据类型。  
  
||int|float|money|datetime|varchar|nvarchar|  
|------|---------|-----------|-----------|--------------|-------------|--------------|  
|**int**|**int**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**float**|**float**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**money**|**money**|**money**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**datetime**|**varchar**|**varchar**|**varchar**|**datetime**|**varchar**|**nvarchar**|  
|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**nvarchar**|  
|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|  
  
## <a name="remarks"></a>备注  
 还可以使用 **sp_OAMethod** 获取属性值。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份或直接对此存储过程执行权限。 `Ole Automation Procedures` 必须 **启用** 配置才能使用与 OLE 自动化相关的任何系统过程。  
  
## <a name="examples"></a>示例  
  
### <a name="a-calling-a-method"></a>A. 调用一个方法  
 下面的示例调用 `Connect` 以前创建的 **SQLServer** 对象的方法。  
  
```  
EXEC @hr = sp_OAMethod @object, 'Connect', NULL, 'my_server',  
    'my_login', 'my_password';  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END;  
```  
  
### <a name="b-getting-a-property"></a>B. 获取一个属性  
 下面的示例获取 `HostName` 以前创建的 **SQLServer** 对象的属性 () 并将其存储在本地变量中。  
  
```  
DECLARE @property varchar(255);  
EXEC @hr = sp_OAMethod @object, 'HostName', @property OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END;  
PRINT @property;  
```  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的 OLE 自动化存储过程 ](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [OLE 自动化脚本示例](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
