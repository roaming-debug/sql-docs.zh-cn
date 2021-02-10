---
description: XactAttributeEnum
title: XactAttributeEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- XactAttributeEnum
helpviewer_keywords:
- XactAttributeEnum enumeration [ADO]
ms.assetid: e7dcecd3-7dc7-445c-b922-f700c3067fbc
author: rothja
ms.author: jroth
ms.openlocfilehash: e741ec0e5458218ddb6dc3a46c3a098acf482611
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056022"
---
# <a name="xactattributeenum"></a>XactAttributeEnum
指定 [连接](./connection-object-ado.md) 对象的事务特性。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adXactAbortRetaining**|262144|通过调用 [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) 以自动启动新事务来执行保留中止。 并非所有提供程序都支持此行为。|  
|**adXactCommitRetaining**|131072|通过调用 [CommitTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) 以自动启动新事务来执行保留提交。 并非所有提供程序都支持此行为。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums.XactAttribute.ABORTRETAINING|  
|AdoEnums.XactAttribute.COMMITRETAINING|  
  
## <a name="applies-to"></a>应用于  
 [Attributes 属性 (ADO)](./attributes-property-ado.md)