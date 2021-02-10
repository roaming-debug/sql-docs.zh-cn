---
description: DataSpace（ADO - WFC 语法）
title: " (ADO-WFC 语法) 的空间Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- DataSpace collection [ADO], ADO/WFC syntax
ms.assetid: 950d45d8-07de-467b-b255-f9a7b997204c
author: rothja
ms.author: jroth
ms.openlocfilehash: 40207634f4111852bc280823826c108c49fde070
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034437"
---
# <a name="dataspace-ado---wfc-syntax"></a>DataSpace（ADO - WFC 语法）
**空间** 类的 **createObject** 方法指定业务对象处理客户端应用程序请求 (*progid*) 和通信协议和服务器 (*连接*) 。 **createObject** 返回表示服务器的 [ObjectProxy](../../../ado/reference/ado-api/objectproxy-ado-wfc-syntax.md) 对象。  
  
## <a name="package-commswfcdata"></a>包 .com. 数据  
  
### <a name="constructor"></a>构造函数  
  
```  
public DataSpace()  
```  
  
### <a name="methods"></a>方法  
  
```  
public static ObjectProxy DataSpace.createObject(String  
    progid, String connection)  
```  
  
### <a name="properties"></a>属性  
  
```  
public static int getInternetTimeout()  
public static void setInternetTimeout(int plInetTimeout)  
```  
  
## <a name="see-also"></a>另请参阅  
 [DataSpace 对象 (RDS)](../../../ado/reference/rds-api/dataspace-object-rds.md)
