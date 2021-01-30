---
description: 命令（ADO - WFC 语法）
title: 命令 (ADO-WFC 语法) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Command collection [ADO], ADO/WFC syntax
ms.assetid: 39d0aa06-03ac-4c9a-8400-83947756ef99
author: rothja
ms.author: jroth
ms.openlocfilehash: b1846915fad045029e27a6fb2f5ebbb32f5beec4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164667"
---
# <a name="command-ado---wfc-syntax"></a>命令（ADO - WFC 语法）
## <a name="package-commswfcdata"></a>包 .com. 数据  
  
### <a name="constructor"></a>构造函数  
  
```  
public Command()  
public Command(String commandtext)  
```  
  
### <a name="methods"></a>方法  
  
```  
public void cancel()  
public com.ms.wfc.data.Parameter createParameter(String  
    Name, int Type, int Direction, int Size, Object Value)  
public Recordset execute()  
public Recordset execute(Object[] parameters)  
public Recordset execute(Object[] parameters, int options)  
public int executeUpdate(Object[] parameters)  
public int executeUpdate(Object[] parameters, int options)  
public int executeUpdate()  
```  
  
 **ExecuteUpdate** 方法是一种特殊的 case 方法，该方法使用某些参数调用基础 ADO **execute** 方法。 **ExecuteUpdate** 方法不支持返回 **Recordset** 对象，因此，**执行** 方法的 *options* 参数使用 **AdoEnums.ExecuteOptions** 进行修改。 **Execute** 方法完成后，其更新的 *RecordsAffected* 参数会传递回 **executeUpdate** 方法，该方法最终返回为 **int**。  
  
### <a name="properties"></a>属性  
  
```  
public com.ms.wfc.data.Connection getActiveConnection()  
public void setActiveConnection(com.ms.wfc.data.Connection con)  
public void setActiveConnection(String conString)  
public String getCommandText()  
public void setCommandText(String command)  
public int getCommandTimeout()  
public void setCommandTimeout(int timeout)  
public int getCommandType()  
public void setCommandType(int type)  
public String getName()  
public void setName(String name)  
public boolean getPrepared()  
public void setPrepared(boolean prepared)  
public int getState()  
public com.ms.wfc.data.Parameter getParameter(int n)  
public com.ms.wfc.data.Parameter getParameter(String n)  
public com.ms.wfc.data.Parameters getParameters()  
public AdoProperties getProperties()  
```  
  
## <a name="see-also"></a>另请参阅  
 [命令对象 (ADO)](./command-object-ado.md)