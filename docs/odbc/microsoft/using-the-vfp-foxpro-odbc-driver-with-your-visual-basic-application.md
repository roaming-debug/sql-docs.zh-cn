---
description: 配合使用 VFP FoxPro ODBC 驱动程序和 Visual Basic 应用程序
title: 将 VFP FoxPro ODBC 驱动程序用于 Visual Basic 应用程序 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- Visual FoxPro ODBC driver [ODBC], visual basic applications
- Visual Basic applications [ODBC]
- FoxPro ODBC driver [ODBC], visual basic applications
- Visual FoxPro data [ODBC], visual basic applications
ms.assetid: 5223ca23-5df6-4ebc-aa3b-70682ff27a8c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4bf121725c30f5f01ef4314c21f1dffbf34fcce8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070392"
---
# <a name="using-the-vfp-foxpro-odbc-driver-with-your-visual-basic-application"></a>配合使用 VFP FoxPro ODBC 驱动程序和 Visual Basic 应用程序
Microsoft® Visual Basic®应用程序可以通过创建连接到 Visual FoxPro 数据源的数据控件与 Visual FoxPro 数据通信。  
  
#### <a name="to-connect-to-visual-foxpro-data-using-the-data-control-in-visual-basic"></a>使用 Visual Basic 中的数据控件连接到 Visual FoxPro 数据  
  
1.  创建一个名为 "test" 的数据源，该数据源连接到 Visual FoxPro 中包含的 TasTrade 示例数据库。 默认的 Visual FoxPro 安装将 TasTrade 示例数据库放置在该位置：  
  
    ```  
    c:\vfp\samples\mainsamp\data\tastrade.dbc  
    ```  
  
2.  在 Visual Basic 中，创建一个新窗体，然后在其上放置一个文本框和一个数据控件。  
  
3.  按如下所示更改数据控件的连接属性：  
  
    ```  
    ODBC;DATABASE=tastrade;DSN=test  
    ```  
  
4.  将 "项内容属性" 属性更改为以下内容：  
  
    ```  
    2 - Snapshot  
    ```  
  
5.  将 RecordSource 属性更改为以下内容：  
  
    ```  
    customer  
    ```  
  
6.  将文本框的 "DataSource" 属性更改为 "数据控件" 的默认名称，如下所示：  
  
    ```  
    data1  
    ```  
  
7.  将文本框的 DataField 属性更改为以下内容：  
  
    ```  
    customer_id  
    ```  
  
8.  运行窗体，并使用数据控件从 Visual FoxPro TasTrade 示例数据库中跳过客户 id 字段。
