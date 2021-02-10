---
description: 记录集（ADO - WFC 语法）
title: 记录集 (ADO-WFC 语法) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Recordset collection [ADO], ADO/WFX syntax
ms.assetid: bd1f571e-007f-432e-ada1-5c3e436c1a22
author: rothja
ms.author: jroth
ms.openlocfilehash: 8ddfe5c0b536ecf9d9876a7d282df7dab1d3835b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051788"
---
# <a name="recordset-ado---wfc-syntax"></a>记录集（ADO - WFC 语法）
## <a name="package-commswfcdata"></a>包 .com. 数据  
  
### <a name="constructors"></a>构造函数  
  
```  
public Recordset()  
public Recordset(Object r)  
```  
  
### <a name="methods"></a>方法  
  
```  
public void addNew(Object[] fieldList, Object[] valueList)  
public void addNew(Object[] valueList)  
public void addNew()  
public void cancel()  
public void cancelBatch(int affectRecords)  
public void cancelBatch()  
public void cancelUpdate()  
public Object clone()  
public Object clone(int lockType)  
public void close()   
public int compareBookmarks(Object bookmark1, Object bookmark2)  
public void delete(int affectRecords)  
public void delete()  
public void find(String criteria)  
public void find(String criteria, int SkipRecords)  
public void find(String criteria, int SkipRecords, int searchDirection)  
public void find(String criteria, int SkipRecords, int searchDirection, Object bmkStart)  
public Object[][] getRows(int Rows, Object bmkStart, Object[] fieldList)  
public void move(int numRecords)  
public void move(int numRecords, Object bmkStart)  
public void moveFirst()  
public void moveLast()  
public void moveNext()  
public void movePrevious()  
public Recordset nextRecordset()  
public Recordset nextRecordset(int[] recordsAffected)  
public void open()  
public void open(Object source)  
public void open(Object source, Object activeConnection)  
public void open(Object source, Object activeConnection, int cursorType)  
public void open(Object source, Object activeConnection, int cursorType,   
                  int lockType)  
public void open(Object source, Object activeConnection, int cursorType,   
                  int lockType, int options)  
public void requery()  
public void requery(int options)  
public void resync()  
public void resync(int affectRecords, int resyncValues)  
public void save(String fileName)  
public void save(String fileName, int persistFormat)  
public boolean supports(int cursorOptions)  
public void update()  
public void update(Object[] valueList)  
public void update(Object[] fieldList, Object[] valueList)  
public void updateBatch()  
public void updateBatch(int affectRecords)  
```  
  
### <a name="properties"></a>属性  
  
```  
public int getAbsolutePage()  
public void setAbsolutePage(int page)  
public int getAbsolutePosition()  
public void setAbsolutePosition(int pos)  
public Command getActiveCommand()  
public Connection getActiveConnection()  
public void setActiveConnection(String conn)  
public void setActiveConnection(com.ms.wfc.data.Connection c)  
public boolean getBOF()  
public boolean getEOF()  
public Object getBookmark()  
public void setBookmark(Object bmk)  
public int getCacheSize()  
public void setCacheSize(int size)  
public int getCursorLocation()  
public void setCursorLocation(int cursorLoc)  
public int getCursorType()  
public void setCursorType(int cursorType)  
public String getDataMember()  
public void setDataMember(String pbstrDataMember)  
public Iunknown getDataSource()  
public void setDataSource(IUnknown dataSource)  
public int getEditMode()  
public Object getFilter()  
public void setFilter(Object filter)  
public int getLockType()  
public void setLockType(int lockType)  
public int getMarshalOptions()  
public void setMarshalOptions(int options)  
public int getMaxRecords()  
public void setMaxRecords(int maxRecords)  
public int getPageCount()  
public int getPageSize()  
public void setPageSize(int pageSize)  
public int getRecordCount()  
public String getSort()  
public void setSort(String criteria)  
public String getSource()  
public void setSource(String query)  
public void setSource(com.ms.wfc.data.Command command)  
public int getState()  
public int getStatus()  
public boolean getStayInSync()  
public void setStayInSync(boolean pbStayInSync)  
public com.ms.wfc.data.Field getField(int n)  
public com.ms.wfc.data.Field getField(String n)  
public com.ms.wfc.data.Fields getFields()  
public AdoProperties getProperties()  
```  
  
### <a name="events"></a>事件  
 有关 ADO/WFC 事件的详细信息，请参阅 [按语言的 Ado 事件实例化](../../guide/data/ado-event-instantiation-by-language.md)。  
  
```  
public void addOnEndOfRecordset(RecordsetEventHandler handler)  
public void removeOnEndOfRecordset(RecordsetEventHandler handler)  
public void addOnFetchComplete(RecordsetEventHandler handler)  
public void removeOnFetchComplete(RecordsetEventHandler handler)  
public void addOnFetchProgress(RecordsetEventHandler handler)  
public void removeOnFetchProgress(RecordsetEventHandler handler)  
public void addOnFieldChangeComplete(RecordsetEventHandler handler)  
public void removeOnFieldChangeComplete(RecordsetEventHandler handler)  
public void addOnMoveComplete(RecordsetEventHandler handler)  
public void removeOnMoveComplete(RecordsetEventHandler handler)  
public void addOnRecordChangeComplete(RecordsetEventHandler handler)  
public void removeOnRecordChangeComplete(RecordsetEventHandler handler)  
public void addOnRecordsetChangeComplete(RecordsetEventHandler handler)  
public void removeOnRecordsetChangeComplete(RecordsetEventHandler handler)  
public void addOnWillChangeField(RecordsetEventHandler handler)  
public void removeOnWillChangeField(RecordsetEventHandler handler)  
public void addOnWillChangeRecord(RecordsetEventHandler handler)  
public void removeOnWillChangeRecord(RecordsetEventHandler handler)  
public void addOnWillChangeRecordset(RecordsetEventHandler handler)  
public void removeOnWillChangeRecordset(RecordsetEventHandler handler)  
public void addOnWillMove(RecordsetEventHandler handler)  
public void removeOnWillMove(RecordsetEventHandler handler)  
```  
  
## <a name="see-also"></a>另请参阅  
 [记录集对象 (ADO)](./recordset-object-ado.md)