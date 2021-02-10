---
description: 编辑现有记录
title: 编辑现有记录 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- editing data [ADO], existing records
ms.assetid: 17ce1263-5897-452a-9ea5-c7f96b33df65
author: rothja
ms.author: jroth
ms.openlocfilehash: 031d819e2c8eed63b9b8ff42aa58fb8e998a4273
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037497"
---
# <a name="editing-existing-records"></a>编辑现有记录
若要编辑现有记录，请转到要编辑的行，然后更改要更改的字段的 **值** 属性。 有关 **字段** 对象的 **Value** 属性的详细信息，请参阅 [检查数据](./examining-data.md)。 根据游标类型，您将使用 **Update** 或 **UpdateBatch** 将更改发送回数据源。 有关详细信息，请参阅 [更新和保留数据](./updating-and-persisting-data.md)。  
  
 通常，将存储过程与命令对象结合使用来执行更新，以及执行其他操作，这种方法通常更高效，因为存储过程不需要创建游标。 有关游标的详细信息，请参阅 [了解游标和锁定](./understanding-cursors-and-locks.md)。