---
description: 处理 JScript 中的错误
title: 处理 JScript 中的错误 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- JScript
helpviewer_keywords:
- errors [ADO], JScript
- JScript error handling [ADO]
ms.assetid: 3de527e5-2e65-4ab0-9b7f-6d317c4478de
author: rothja
ms.author: jroth
ms.openlocfilehash: 5d88d7bd548ef475403f67ee80d2a1c0a7080446
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033097"
---
# <a name="handling-errors-in-jscript"></a>处理 JScript 中的错误
Microsoft® JScript®代码必须检查 **连接** 对象的 **错误** 集合的 **Count** 属性。 如果值大于0，则循环访问集合，并像在任何其他语言中那样打印这些值。  
  
```  
<!-- BeginErrorExampleJS -->  
<%@ Language=JScript %>  
<HTML>  
<HEAD>  
<title>Error Handling Example (JScript)</title>  
</HEAD>  
<BODY>  
<h1>Error Handling Example (JScript)</h1>  
<%  
   var cnn1 = Server.CreateObject("ADODB.Connection");  
   var errLoop = Server.CreateObject("ADODB.Error");  
   var strError = new String;  
  
   // Intentionally trigger an error.  
   cnn1.Open("nothing");  
  
   if (cnn1.Errors.Count > 0) {  
      // Enumerate Errors collection and display  
      // properties of each Error object.  
      for (var i = 1; i < cnn1.Errors.Count; i++) {  
         errLoop = cnn1.Errors(i);  
         strError = "Error #" & errLoop.Number + "<br>" +  
            "   " + errLoop.Description + "<br>" +  
            "   (Source: " & errLoop.Source & ")" + "<br>" +  
            "   (SQL State: " & errLoop.SQLState + ")" + "<br>" +  
            "   (NativeError: " & errLoop.NativeError + ")" + "<br>";  
         if (errLoop.HelpFile == "")  
            strError = strError +  
               "   No Help file available" +  
               "<br><br>";  
         else  
            strError = strError +  
               "   (HelpFile: " & errLoop.HelpFile & ")" & "<br>" +  
               "   (HelpContext: " & errLoop.HelpContext & ")" +  
               "<br><br>";  
         Response.Write("<p>" & strError & "</p>");  
      }  
   }  
%>  
  
</BODY>  
</HTML>  
<!-- EndErrorExampleJS -->  
```
