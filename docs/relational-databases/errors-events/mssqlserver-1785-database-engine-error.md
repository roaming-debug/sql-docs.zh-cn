---
description: MSSQLSERVER_1785
title: MSSQLSERVER_1785
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1785 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 6adb44b26ca0af5ab00476c680c924875cd50d2d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348826"
---
# <a name="mssqlserver_1785"></a>MSSQLSERVER_1785
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|1785|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|CRTFKINVTOPO|
|消息正文|将 FOREIGN KEY 约束 '%.ls' 引入表 '%. ls' 可能会导致循环或多重级联路径。 请指定 ON DELETE NO ACTION 或 ON UPDATE NO ACTION，或修改其他 FOREIGN KEY 约束。|
||

## <a name="explanation"></a>说明

之所以会收到此错误消息，是因为在 SQL Server 中，一个表不能多次出现在由 `DELETE` 或 `UPDATE` 语句启动的所有级联引用操作列表中。 级联引用操作树必须在级联引用操作树上只有一个指向特定表的路径。

将向用户报告如下错误消息：

> 服务器:消息 1785，级别 16，状态 1，行 1 将 FOREIGN KEY 约束“fk_two”引入表“table2”可能会导致循环或多重级联路径。 请指定 ON DELETE NO ACTION 或 ON UPDATE NO ACTION，或修改其他 FOREIGN KEY 约束。 服务器:消息 1750，级别 16，状态 1，行 1 无法创建约束。 请参阅前面的错误

## <a name="user-action"></a>用户操作

若要解决此问题，请创建一个外键，以便在级联引用操作列表中创建指向一个表的单个路径。

可以通过多种方式强制引用完整性。 声明性引用完整性 (DRI) 是最基本的方法，但也是最不灵活的方法。 如果需要更多灵活性，但仍需要高度完整性，则可以改用触发器。

## <a name="more-information"></a>详细信息

下面的示例代码是生成错误消息的外键创建尝试示例：

```sql
USE tempdb
GO

CREATE TABLE table1 (user_ID INTEGER NOT NULL PRIMARY KEY, user_name
CHAR(50) NOT NULL)
GO

CREATE TABLE table2 (author_ID INTEGER NOT NULL PRIMARY KEY, author_name
CHAR(50) NOT NULL, lastModifiedBy INTEGER NOT NULL, addedby INTEGER NOT NULL)
GO

ALTER TABLE table2 ADD CONSTRAINT fk_one FOREIGN KEY (lastModifiedby)
REFERENCES table1 (user_ID) ON DELETE CASCADE ON UPDATE cascade
GO

ALTER TABLE table2 ADD CONSTRAINT fk_two FOREIGN KEY (addedby)
REFERENCES table1(user_ID) ON DELETE NO ACTION ON UPDATE cascade
GO
--this fails with the error because it provides a second cascading path to table2.

ALTER TABLE table2 ADD CONSTRAINT fk_two FOREIGN KEY (addedby)
REFERENCES table1 (user_ID) ON DELETE NO ACTION ON UPDATE NO ACTION
GO
-- this works.
```

### <a name="see-also"></a>另请参阅

[级联引用完整性](../tables/primary-and-foreign-key-constraints.md#referential-integrity)