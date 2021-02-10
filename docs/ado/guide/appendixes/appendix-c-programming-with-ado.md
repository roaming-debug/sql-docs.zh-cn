---
description: 附录 C：在开发环境中用 ADO 编程
title: 附录 C：用 ADO 编程 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, programming
ms.assetid: 40af6e70-2a37-480f-aadc-92095d450af7
author: rothja
ms.author: jroth
ms.openlocfilehash: dbed6b90aaf097bba303ab65de0b3d2c00baa85f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029451"
---
# <a name="appendix-c-programming-with-ado-in-development-environments"></a>附录 C：在开发环境中用 ADO 编程
ADO 是可与许多编程语言（包括 Microsoft Visual Basic、VBScript、JScript 和 Visual C++）结合使用的 COM 自动化接口组件。 其中的每个工具和其他应用程序（如 Microsoft Office 和 Microsoft SQL Server）都安装了 ADO 版本。

 ADO 的库是 msado15.dll 的，程序 ID (ProgID) 前缀为 "ADODB.RECORDSET"。 例如，若要显式引用 ADO [记录集](../../reference/ado-api/recordset-object-ado.md)，请使用 `ADODB.Recordset` 。

 有关在各种开发环境中用 ADO 进行编程的详细信息，请参阅以下主题：

-   [配合使用 ADO 与 Microsoft Visual Basic](./using-ado-with-microsoft-visual-basic.md)

-   [配合使用 ADO 与脚本语言](./using-ado-with-scripting-languages.md)

-   [配合使用 ADO 与 Microsoft Visual C++](./using-ado-with-microsoft-visual-c.md)

## <a name="see-also"></a>另请参阅
 [ADO API 参考](../../reference/ado-api/ado-api-reference.md)[附录 D： ADO 示例](./appendix-d-ado-samples.md)[配置 RDS](../remote-data-service/configuring-rds.md) [Microsoft ActiveX 数据对象 (ado) ](../../microsoft-activex-data-objects-ado.md) [附录 A：提供方](./appendix-a-providers.md) [ADO 历史记录](../ado-history.md)