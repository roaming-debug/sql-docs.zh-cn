---
description: 将记录添加到记录集
title: 添加记录 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- AddNew method [ADO]
- ADO, editing data
- editing data [ADO], AddNew method
- editing data [ADO], adding data
ms.assetid: dd34669e-6f06-403b-9241-1c85c82aecc2
author: rothja
ms.author: jroth
ms.openlocfilehash: b39a0920d9816837b6d737df00e6bb4560358356
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033397"
---
# <a name="adding-records-to-a-recordset"></a>将记录添加到记录集
使用 **AddNew** 方法可在现有 **记录集中** 创建和初始化新记录。 您可以使用 **支持** 的 **CursorOptionEnum** 值为 **adAddNew** 的方法，验证是否可以将记录添加到当前 **记录集** 对象。

 调用 **AddNew** 方法之后，新记录将成为当前记录，并在调用 **Update** 方法后保持最新。 如果 **记录集** 对象不支持书签，则在移动到另一记录后，您可能无法访问新记录。 因此，根据游标类型，您可能需要调用 **Requery** 方法以使新记录可访问。

 如果在编辑当前记录或添加新记录时调用 **AddNew** ，ADO 将调用 **Update** 方法来保存所有更改，然后创建新记录。

 本部分包含以下主题。

-   [使用 AddNew 添加记录](./adding-records-using-addnew.md)

-   [添加多个字段](./adding-multiple-fields.md)

-   [确定编辑模式](./determining-edit-mode.md)

-   [以即时和批处理模式使用 AddNew](./using-addnew-in-immediate-and-batch-modes.md)