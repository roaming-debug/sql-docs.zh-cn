---
description: executeUpdate 方法 (java.lang.String)
title: executeUpdate 方法 (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 02/07/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.executeUpdate (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 91ecb1cd-001d-4ac9-9ae8-5db05c3c2959
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 39ef10b030e96ca7d419977c56e9ca6388fb8e0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038557"
---
# <a name="executeupdate-method-javalangstring"></a>executeUpdate 方法 (java.lang.String)

运行给定的 SQL 语句，可以是 INSERT、UPDATE、MERGE 或 DELETE 语句；或不返回任何内容的 SQL 语句，例如 SQL DDL 语句。

## <a name="syntax"></a>语法

```
public final int executeUpdate(java.lang.String sql)
```

#### <a name="parameters"></a>参数
*sql*

包含 SQL 语句的 String。

## <a name="return-value"></a>返回值
一个指示受影响的行数的 int，如果使用 DDL 语句，则为 0。

## <a name="exceptions"></a>例外
[SQLServerException](./sqlserverexception-class.md)

## <a name="remarks"></a>备注
此 executeUpdate 方法是由 java.sql.PreparedStatement 接口中的 executeUpdate 方法指定的。

调用此方法将导致异常，因为在创建 SQLServerPreparedStatement 对象时指定了该对象的 SQL 语句。

## <a name="see-also"></a>另请参阅

[executeUpdate 方法 &#40;SQLServerPreparedStatement&#41;](./executeupdate-method-sqlserverpreparedstatement.md)

[SQLServerPreparedStatement 成员](./sqlserverpreparedstatement-members.md)

[SQLServerPreparedStatement 类](./sqlserverpreparedstatement-class.md)
