---
description: DefaultDatabase 属性
title: DefaultDatabase 属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::DefaultDatabase
helpviewer_keywords:
- DefaultDatabase property
ms.assetid: 41e8a8dd-e69c-4a09-8736-93502e01961c
author: rothja
ms.author: jroth
ms.openlocfilehash: 8cff37ae3bc0940a62e2acc9af3ac6c25d1379a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167587"
---
# <a name="defaultdatabase-property"></a>DefaultDatabase 属性
指示 [连接](../../../ado/reference/ado-api/connection-object-ado.md) 对象的默认数据库。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 设置或返回一个 **字符串** 值，该值的计算结果为提供程序提供的数据库的名称。  
  
## <a name="remarks"></a>备注  
 使用 **DefaultDatabase** 属性可以设置或返回特定 **连接** 对象上默认数据库的名称。  
  
 如果有默认数据库，SQL 字符串可以使用非限定语法来访问该数据库中的对象。 若要访问 **DefaultDatabase** 属性中指定的数据库中的对象，必须使用所需的数据库名称限定对象名称。 在连接时，提供程序将向 **DefaultDatabase** 属性写入默认数据库信息。 某些提供程序仅允许每个连接一个数据库，在这种情况下，不能更改 **DefaultDatabase** 属性。  
  
 某些数据源和提供程序可能不支持此功能，并且可能会返回错误或空字符串。  
  
> [!NOTE]
>  **远程数据服务使用情况** 此属性在客户端 **连接** 对象上不可用。  
  
## <a name="applies-to"></a>应用于  
 [连接对象 (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [Provider 和 DefaultDatabase Properties (VB) ](../../../ado/reference/ado-api/provider-and-defaultdatabase-properties-example-vb.md)   
 [Provider 和 DefaultDatabase 属性示例 (VC++)](../../../ado/reference/ado-api/provider-and-defaultdatabase-properties-example-vc.md)   
