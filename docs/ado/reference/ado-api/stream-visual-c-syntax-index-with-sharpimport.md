---
description: '流式处理 (Visual C++ 语法索引与 #import) '
title: 流 (#import) 的 Visual C++ 语法索引 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
dev_langs:
- C++
helpviewer_keywords:
- Stream collection [ADO]
ms.assetid: e59d0687-1f5a-45c5-9d0a-c1f27079495d
author: rothja
ms.author: jroth
ms.openlocfilehash: f030bf01a0fbfc69911713466255f7335e434a04
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056592"
---
# <a name="stream-visual-c-syntax-index-with-import"></a>流式处理 (Visual C++ 语法索引与 #import) 
## <a name="methods"></a>方法  
  
```  
HRESULT Cancel( );  
  
HRESULT Close( );  
  
HRESULT CopyTo( struct _Stream * DestStream, int CharNumber );  
  
HRESULT Flush( );  
  
HRESULT LoadFromFile( _bstr_t FileName );  
  
HRESULT Open( const _variant_t & Source, enum  
    ConnectModeEnum Mode, enum     StreamOpenOptionsEnum Options, _bstr_t  
    UserName, _bstr_t Password );  
  
_variant_t Read( long NumBytes );  
  
_bstr_t ReadText( long NumChars );  
  
HRESULT SaveToFile( _bstr_t FileName, enum SaveOptionsEnum  
    Options );  
  
HRESULT SetEOS( );  
  
HRESULT SkipLine( );  
  
HRESULT Write( const _variant_t & Buffer );  
  
HRESULT WriteText( _bstr_t Data, enum StreamWriteEnum  
    Options );  
```  
  
## <a name="properties"></a>属性  
  
```  
_bstr_t GetCharset( );  
void PutCharset( _bstr_t pbstrCharset );  
__declspec(property(get=GetCharset,put=PutCharset)) _bstr_t Charset;  
  
VARIANT_BOOL GetEOS( );  
__declspec(property(get=GetEOS)) VARIANT_BOOL EOS;  
  
enum LineSeparatorEnum GetLineSeparator( );  
void PutLineSeparator( enum LineSeparatorEnum pLS );  
__declspec(property(get=GetLineSeparator,put=PutLineSeparator)) enum  
    LineSeparatorEnum LineSeparator;  
  
enum ConnectModeEnum GetMode( );  
void PutMode( enum ConnectModeEnum pMode );  
__declspec(property(get=GetMode,put=PutMode)) enum ConnectModeEnum Mode;  
  
long GetPosition( );  
void PutPosition( long pPos );  
__declspec(property(get=GetPosition,put=PutPosition)) long Position;  
  
long GetSize( );  
__declspec(property(get=GetSize)) long Size;  
  
enum ObjectStateEnum GetState( );  
__declspec(property(get=GetState)) enum ObjectStateEnum State;  
  
enum StreamTypeEnum GetType( );  
void PutType( enum StreamTypeEnum ptype );  
__declspec(property(get=GetType,put=PutType)) enum StreamTypeEnum Type;  
```  
  
## <a name="see-also"></a>另请参阅  
 [流对象 (ADO)](./stream-object-ado.md)