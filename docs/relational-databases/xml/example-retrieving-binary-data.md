---
title: 示例：检索二进制数据 | Microsoft Docs
description: 查看 SQL 查询的示例，该查询使用具有 FOR XML 子句的 RAW 和 BINARY BASE64 选项检索二进制数据。
ms.custom: ''
ms.date: 04/03/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- RAW mode, retrieving binary data example
ms.assetid: 5cea5d49-58ac-403a-a933-c4fd91de400b
author: RothJa
ms.author: jroth
ms.openlocfilehash: fedec9e351b9c8910db0bee51c1e2c5dc8569655
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97402974"
---
# <a name="example-retrieving-binary-data"></a>示例：检索二进制数据

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

下面的查询返回在 **varbinary(max)** 类型列中存储的产品照片。 在此查询中指定了 `BINARY BASE64` 选项，以便以 base64 编码格式返回二进制数据。

## <a name="example"></a>示例

```sql
USE AdventureWorks2012;
GO
SELECT ProductPhotoID, ThumbNailPhoto
FROM Production.ProductPhoto
WHERE ProductPhotoID=1
FOR XML RAW, BINARY BASE64 ;
GO
```

预期的结果如下：

```console
<row ProductModelID="1" ThumbNailPhoto="base64 encoded binary data"/>
```

## <a name="see-also"></a>另请参阅

[将 RAW 模式与 FOR XML 一起使用](../../relational-databases/xml/use-raw-mode-with-for-xml.md)
