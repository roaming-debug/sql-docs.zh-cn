---
description: 连接（ADO for Visual C++ 语法）
title: Visual C++ 语法) 的连接 (ADO |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
dev_langs:
- C++
helpviewer_keywords:
- Connection collection [ADO], ADO for Visual C++ syntax
ms.assetid: cb5e1e15-c5b4-44ab-892f-bf1ae601d0a5
author: rothja
ms.author: jroth
ms.openlocfilehash: aaffd8563a7135837ccd0760fdab5f529fe065a4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034777"
---
# <a name="connection-ado-for-visual-c-syntax"></a>连接（ADO for Visual C++ 语法）
## <a name="methods"></a>方法  
  
```  
BeginTrans(long *TransactionLevel)  
CommitTrans(void)  
RollbackTrans(void)  
Cancel(void)  
Close(void)  
Execute(BSTR CommandText, VARIANT *RecordsAffected, long Options, _ADORecordset **ppiRset)  
Open(BSTR ConnectionString, BSTR UserID, BSTR Password, long Options)  
OpenSchema(SchemaEnum Schema, VARIANT Restrictions, VARIANT SchemaID, _ADORecordset **pprset)  
```  
  
## <a name="properties"></a>属性  
  
```  
get_Attributes(long *plAttr)  
put_Attributes(long lAttr)  
get_CommandTimeout(LONG *plTimeout)  
put_CommandTimeout(LONG lTimeout)  
get_ConnectionString(BSTR *pbstr)  
put_ConnectionString(BSTR bstr)  
get_ConnectionTimeout(LONG *plTimeout)  
put_ConnectionTimeout(LONG lTimeout)  
get_CursorLocation(CursorLocationEnum *plCursorLoc)  
put_CursorLocation(CursorLocationEnum lCursorLoc)  
get_DefaultDatabase(BSTR *pbstr)  
put_DefaultDatabase(BSTR bstr)  
get_IsolationLevel(IsolationLevelEnum *Level)  
put_IsolationLevel(IsolationLevelEnum Level)  
get_Mode(ConnectModeEnum *plMode)  
put_Mode(ConnectModeEnum lMode)  
get_Provider(BSTR *pbstr)  
put_Provider(BSTR Provider)  
get_State(LONG *plObjState)  
get_Version(BSTR *pbstr)  
get_Errors(ADOErrors **ppvObject)  
```  
  
## <a name="events"></a>事件  
  
```  
BeginTransComplete(LONG TransactionLevel, ADOError *pError, EventStatusEnum *adStatus, _ADOConnection *pConnection)  
CommitTransComplete(ADOError *pError, EventStatusEnum *adStatus, _ADOConnection *pConnection)  
ConnectComplete(ADOError *pError, EventStatusEnum *adStatus, _ADOConnection *pConnection)  
Disconnect(EventStatusEnum *adStatus, _ADOConnection *pConnection)  
ExecuteComplete(LONG RecordsAffected, ADOError *pError, EventStatusEnum *adStatus, _ADOCommand *pCommand, _ADORecordset *pRecordset, _ADOConnection *pConnection)  
InfoMessage(ADOError *pError, EventStatusEnum *adStatus, _ADOConnection *pConnection)  
RollbackTransComplete(ADOError *pError, EventStatusEnum *adStatus, _ADOConnection *pConnection)  
WillConnect(BSTR *ConnectionString, BSTR *UserID, BSTR *Password, long *Options, EventStatusEnum *adStatus, _ADOConnection *pConnection)  
WillExecute(BSTR *Source, CursorTypeEnum *CursorType, LockTypeEnum *LockType, long *Options, EventStatusEnum *adStatus, _ADOCommand *pCommand, _ADORecordset *pRecordset, _ADOConnection *pConnection)  
```  
  
## <a name="see-also"></a>另请参阅  
 [连接对象 (ADO)](./connection-object-ado.md)