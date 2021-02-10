---
description: 用于 Internet 发布的 OLE DB 提供程序
title: 用于 Internet 发布的 OLE DB 提供程序 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB provider for Internet publishing [ADO]
- ADO, Internet publishing
- publishing to Internet [ADO]
- Internet publishing [ADO]
- providers [ADO], OLE DB provider for Internet publishing
ms.assetid: 4869aafa-7401-4ce1-93ce-45406a60274f
author: rothja
ms.author: jroth
ms.openlocfilehash: 9282bdc81d71ecd938f3d115678769915f8e3367
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032399"
---
# <a name="the-ole-db-provider-for-internet-publishing"></a>用于 Internet 发布的 OLE DB 提供程序
ADO [记录](../../../ado/reference/ado-api/record-object-ado.md) 和 [流](../../../ado/reference/ado-api/stream-object-ado.md) 对象可与 microsoft OLE DB 提供商一起使用，以便 Internet 发布 (internet 发布提供程序) 访问和操作资源，如 Microsoft FrontPage 提供的 Web 文件夹或文件。 使用 ADO，你可以将 **记录**、 **流** 或 [记录集](../../../ado/reference/ado-api/recordset-object-ado.md) 的源指定为 URL。 然后，可以上传、下载、移动、复制和删除资源，或直接操作资源属性。  
  
 有关将 **记录** 和 **流** 与 internet 发布提供程序一起使用的示例代码，请参阅 [internet 发布方案](../../../ado/guide/data/internet-publishing-scenario.md)。  
  
 Internet 发布提供程序随 Microsoft Windows 2000 一起安装。 更早版本的 Internet 发布提供程序也可用于 Microsoft Office 2000 和 Microsoft Internet Explorer 5.0。  
  
 可以通过三种方式将 ADO 连接到 Internet 发布提供程序：  
  
-   在连接字符串中指定 "URL ="。 例如：  
  
    ```  
    objConn.Open "URL=https://servername"  
    ```  
  
-   为连接字符串的 *Provider* 关键字指定 Msdaipp。 例如：  
  
    ```  
    objConn.Open "provider=MSDAIPP.DSO;data source=https://servername"  
    ```  
  
-   为[连接](../../../ado/reference/ado-api/connection-object-ado.md)对象的[Provider](../../../ado/reference/ado-api/provider-property-ado.md)属性指定 Msdaipp。 例如：  
  
    ```  
    objConn.Provider = "MSDAIPP.DSO"  
    objConn.Open "https://servername"  
    ```  
  
> [!NOTE]
>  如果 Msdaipp 是使用 *提供程序* 连接字符串关键字或 **提供程序** 属性显式指定为提供程序的值，则不能在连接字符串中使用 "URL ="。 如果这样做，则会发生错误。 相反，只需按前面所示指定 URL。  
  
 有关 Internet 发布提供程序的更多特定信息，请参阅 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)，或与用于 internet 发布的 OLE DB 提供程序的源应用程序一起提供的提供程序文档： Windows 2000、Office 2000 或 internet Explorer 5.0。
