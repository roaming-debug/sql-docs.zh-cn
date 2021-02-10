---
description: 用于 Internet 发布的 Microsoft OLE DB 提供程序概述
title: 用于 Internet 发布的 Microsoft OLE DB 提供程序 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB provider for Internet publishing [ADO]
- providers [ADO], OLE DB provider for Internet publishing
- Internet Publishing provider [ADO]
ms.assetid: 66a208d9-b580-4655-a41e-1d36e5b5bfca
author: rothja
ms.author: jroth
ms.openlocfilehash: e769310609ebae9c7df8cfe0d518acce058d32e7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029281"
---
# <a name="microsoft-ole-db-provider-for-internet-publishing-overview"></a>用于 Internet 发布的 Microsoft OLE DB 提供程序概述
用于 Internet 发布的 Microsoft OLE DB 提供程序允许 ADO 访问 Microsoft FrontPage 或 Microsoft Internet Information Server 所提供的资源。 资源包括 web 源文件（如 HTML 文件）或 Windows 2000 web 文件夹。

## <a name="connection-string-parameters"></a>连接字符串参数
 若要连接到该提供程序，请将 [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md)属性的 *provider* 参数设置为：

```vb
MSDAIPP.DSO
```

 还可以使用 [Provider](../../reference/ado-api/provider-property-ado.md) 属性设置或读取此值。

## <a name="typical-connection-string"></a>典型连接字符串
 此提供程序的典型连接字符串是：

```vb
"Provider=MSDAIPP.DSO;Data Source=ResourceURL;User ID=MyUserID;Password=MyPassword;"
```

 或

```vb
"URL=ResourceURL;User ID=MyUserID;Password=MyPassword;"
```

 字符串包含以下关键字：

|关键字|说明|
|-------------|-----------------|
|**提供程序**|指定用于 Internet 发布的 OLE DB 提供程序。|
|**数据源** 或- **URL**|指定在 Web 文件夹中发布的文件或目录的 URL。|
|**用户 ID**|指定用户名。|
|**密码**|指定用户密码。|

> [!NOTE]
>  如果要连接到支持 Windows 身份验证的数据源提供程序，应在连接字符串中指定 **Trusted_Connection = yes** 或 **集成安全性 = SSPI** 而不是用户 ID 和密码信息。

 如果将连接字符串中的 "URL =" 的 *ResourceURL* 值设置为无效值，则默认情况下，Internet 发布提供程序会引发一个对话框，提示输入有效的值。 这对于应用程序的中间层中的组件是不需要的行为，因为它会挂起程序的执行，直至对话框被清除，并且客户端似乎已冻结，因为它未收到来自组件的响应。

> [!NOTE]
>  如果为 MSDAIPP。通过使用 *提供程序* 连接字符串关键字或 **提供程序** 属性，将 DSO 显式指定为提供程序的值，而不能在连接字符串中使用 "URL ="。 如果这样做，则会发生错误。 相反，只需按主题 [使用 ADO 和用于 Internet 发布的 OLE DB 提供程序](../data/the-ole-db-provider-for-internet-publishing.md)来指定 URL。

## <a name="see-also"></a>另请参阅
 Internet 发布[方案](../data/internet-publishing-scenario.md) [OLE DB 用于 Internet 发布的提供程序](../data/the-ole-db-provider-for-internet-publishing.md)