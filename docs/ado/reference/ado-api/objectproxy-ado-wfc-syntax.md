---
description: ObjectProxy（ADO - WFC 语法）
title: ObjectProxy (ADO-WFC 语法) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- ObjectProxy collection [ADO]
ms.assetid: f68f58bc-ad28-46cc-9fb3-099e1a678397
author: rothja
ms.author: jroth
ms.openlocfilehash: 9e136118be65296cf2a4c97b68afecda1cc6de30
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167017"
---
# <a name="objectproxy-ado---wfc-syntax"></a>ObjectProxy（ADO - WFC 语法）
**ObjectProxy** 对象表示服务器，由 "[空间](../rds-api/dataspace-object-rds.md)" 对象的 **createObject** 方法返回。 ObjectProxy 类具有一个方法， **调用**，它可以在服务器上调用方法，并返回由该调用导致的对象。  
  
 **包 .com. 数据**  
  
## <a name="methods"></a>方法  
  
### <a name="call-method-adowfc-syntax"></a>调用方法 (ADO/WFC 语法)   
 调用 ObjectProxy 所表示的服务器上的方法。 还可以选择将方法参数作为对象数组进行传递。  
  
#### <a name="syntax"></a>语法  
  
```  
public Object ObjectProxy.( String method )  
public Object ObjectProxy.( String method, Object[] args)  
```  
  
#### <a name="returns"></a>返回  
 对象  
 调用方法导致的对象。  
  
#### <a name="parameters"></a>参数  
 *ObjectProxy*  
 表示服务器的 **ObjectProxy** 对象。  
  
 *method*  
 一个字符串，其中包含要在服务器上调用的方法的名称。  
  
 *args*  
 可选。 对象的数组，这些对象是服务器上的方法的参数。 Java 数据类型会自动转换为适合在服务器上使用的数据类型。