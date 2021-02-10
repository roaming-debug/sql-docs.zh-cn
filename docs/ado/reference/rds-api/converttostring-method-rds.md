---
description: ConvertToString 方法 (RDS)
title: ConvertToString 方法 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- ConvertToString method [ADO]
ms.assetid: b3f36bc8-6f69-49b0-83cd-2ccd3afebfbe
author: rothja
ms.author: jroth
ms.openlocfilehash: ce472fa758c479a02a43ce8ddd640219e1f7919e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049507"
---
# <a name="converttostring-method-rds"></a>ConvertToString 方法 (RDS)
将 [记录集](../ado-api/recordset-object-ado.md) 转换为表示记录集数据的 MIME 字符串。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataFactory.ConvertToString(Recordset)  
```  
  
#### <a name="parameters"></a>参数  
 *DataFactory*  
 表示 [RDSServer. DataFactory](./datafactory-object-rdsserver.md) 对象的对象变量。  
  
 *Recordset*  
 表示 **Recordset** 对象的对象变量。  
  
## <a name="remarks"></a>备注  
 使用 .asp 文件，使用 **ConvertToString** 将 **记录集** 嵌入到服务器上生成的 HTML 页面，以将其传输到客户端计算机。  
  
 **ConvertToString** 首先将 **记录集** 加载到游标服务表中，然后以 MIME 格式生成流。  
  
 在客户端上，远程数据服务可以将 MIME 字符串转换回完全正常运行的 **记录集**。 它非常适用于处理少于400行的数据，每行宽度不超过1024字节。 不应将其用于通过 HTTP 流式处理 BLOB 数据和大型结果集。 不会对字符串执行任何网络压缩，因此，与远程数据服务所定义和部署的、通过 HTTP 传输协议格式定义和部署的线路优化 tablegram 格式相比，很大的数据集需要相当长的时间进行传输。  
  
> [!NOTE]
>  如果使用 Active Server Pages 将生成的 MIME 字符串嵌入到客户端 HTML 页中，请注意早于版本2.0 的 VBScript 版本将字符串大小限制为32K。 如果超过此限制，将返回错误。 通过 .asp 文件使用 MIME 嵌入时，使查询范围保持相对较小。 若要解决此问题，请从 Microsoft Windows 脚本技术网站下载最新版本的 VBScript。  
  
## <a name="applies-to"></a>应用于  
 [DataFactory 对象 (RDSServer)](./datafactory-object-rdsserver.md)  
  
## <a name="see-also"></a>另请参阅  
 [ConvertToString 方法示例 (VB) ](../ado-api/converttostring-method-example-vb.md)   
 [ConvertToString 方法示例 (VBScript)](./converttostring-method-example-vbscript.md)