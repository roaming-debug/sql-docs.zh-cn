---
description: 实现全文搜索
title: 实现全文搜索
ms.custom: seo-dt-2019
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- full-text search [SMO]
ms.assetid: 9ce9ad9c-f671-4760-90b5-e0c8ca051473
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 828b677210b00dae3a09d4f73b67fb73f0470edd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352506"
---
# <a name="implementing-full-text-search"></a>实现全文搜索
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  全文搜索对于每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例都可用且在 SMO 中由 <xref:Microsoft.SqlServer.Management.Smo.Server.FullTextService%2A> 对象来表示。 <xref:Microsoft.SqlServer.Management.Smo.FullTextService>对象驻留在 **服务器** 对象下。 它用于管理 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 全文搜索服务的配置选项。 <xref:Microsoft.SqlServer.Management.Smo.FullTextCatalogCollection> 对象属于 <xref:Microsoft.SqlServer.Management.Smo.Database> 对象，并且它是表示为数据库定义的全文目录的 <xref:Microsoft.SqlServer.Management.Smo.FullTextCatalog> 对象的集合。 与普通索引不同，只能为每个表定义一个全文索引。 此索引由 <xref:Microsoft.SqlServer.Management.Smo.FullTextIndexColumn> 对象中的 <xref:Microsoft.SqlServer.Management.Smo.Table> 对象表示。  
  
 若要创建全文搜索服务，必须为数据库定义一个全文目录，并为数据库的其中一个表定义一个全文搜索索引。  
  
 首先，通过调用 <xref:Microsoft.SqlServer.Management.Smo.FullTextCatalog> 构造函数并指定目录名称，为数据库创建全文目录。 其次，通过调用该构造函数并指定要为其创建全文索引的表，创建全文索引。 接着，通过使用 <xref:Microsoft.SqlServer.Management.Smo.FullTextIndexColumn> 对象并提供表中列的名称，可以为全文索引添加索引列。 然后，为已创建的目录设置 <xref:Microsoft.SqlServer.Management.Smo.FullTextIndex.CatalogName%2A> 属性。 最后，调用 <xref:Microsoft.SqlServer.Management.Smo.FullTextIndex.Create%2A> 方法并为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例创建全文索引。  
  
## <a name="example"></a>示例  
 若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  
  
## <a name="creating-a-full-text-search-service-in-visual-basic"></a>在 Visual Basic 中创建全文搜索服务  
 此代码示例为 [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] 示例数据库中的 `ProductCategory` 表创建全文搜索目录。 然后，它会为 `ProductCategory` 表中的 Name 列创建全文搜索索引。 全文搜索索引要求已为该列定义唯一索引。  
  
```  
' compile with:   
' /r:Microsoft.SqlServer.SqlEnum.dll   
' /r:Microsoft.SqlServer.Smo.dll   
' /r:Microsoft.SqlServer.ConnectionInfo.dll   
' /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll  
  
Imports Microsoft.SqlServer.Management.Smo  
Imports Microsoft.SqlServer.Management.Sdk.Sfc  
Imports Microsoft.SqlServer.Management.Common  
  
Public Class A  
   Public Shared Sub Main()  
      ' Connect to the local, default instance of SQL Server.  
      Dim srv As Server = Nothing  
      srv = New Server()  
  
      ' Reference the AdventureWorks database.  
      Dim db As Database = Nothing  
      db = srv.Databases("AdventureWorks")  
  
      ' Reference the ProductCategory table.  
      Dim tb As Table = Nothing  
      tb = db.Tables("ProductCategory", "Production")  
  
      ' Define a FullTextCatalog object variable by specifying the parent database and name arguments in the constructor.  
      Dim ftc As FullTextCatalog = Nothing  
      ftc = New FullTextCatalog(db, "Test_Catalog")  
      ftc.IsDefault = True  
  
      ' Create the Full-Text Search catalog on the instance of SQL Server.  
      ftc.Create()  
  
      ' Define a FullTextIndex object varaible by supplying the parent table argument in the constructor.  
      Dim fti As FullTextIndex = Nothing  
      fti = New FullTextIndex(tb)  
  
      ' Define a FullTextIndexColumn object variable by supplying the parent index and column name arguments in the constructor.  
      Dim ftic As FullTextIndexColumn = Nothing  
      ftic = New FullTextIndexColumn(fti, "Name")  
  
      ' Add the indexed column to the index.  
      fti.IndexedColumns.Add(ftic)  
      fti.ChangeTracking = ChangeTracking.Automatic  
  
      ' Specify the unique index on the table that is required by the Full Text Search index.  
      fti.UniqueIndexName = "AK_ProductCategory_Name"  
  
      ' Specify the catalog associated with the index.  
      fti.CatalogName = "Test_Catalog"  
  
      ' Create the Full Text Search index on the instance of SQL Server.  
      fti.Create()  
   End Sub  
End Class  
```  
  
## <a name="creating-a-full-text-search-service-in-visual-c"></a>在 Visual C# 中创建全文搜索服务  
 此代码示例为 [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] 示例数据库中的 `ProductCategory` 表创建全文搜索目录。 然后，它会为 `ProductCategory` 表中的 Name 列创建全文搜索索引。 全文搜索索引要求已为该列定义唯一索引。  
  
```  
// compile with:   
// /r:Microsoft.SqlServer.SqlEnum.dll   
// /r:Microsoft.SqlServer.Smo.dll   
// /r:Microsoft.SqlServer.ConnectionInfo.dll   
// /r:Microsoft.SqlServer.Management.Sdk.Sfc.dll   
  
using Microsoft.SqlServer.Management.Smo;  
using Microsoft.SqlServer.Management.Sdk.Sfc;  
using Microsoft.SqlServer.Management.Common;  
  
public class A {  
   public static void Main() {  
      // Connect to the local, default instance of SQL Server.  
      Server srv = default(Server);  
      srv = new Server();  
  
      // Reference the AdventureWorks database.  
      Database db = default(Database);  
      db = srv.Databases ["AdventureWorks"];  
  
      // Reference the ProductCategory table.  
      Table tb = default(Table);  
      tb = db.Tables["ProductCategory", "Production"];  
  
      // Define a FullTextCatalog object variable by specifying the parent database and name arguments in the constructor.  
      FullTextCatalog ftc = default(FullTextCatalog);  
      ftc = new FullTextCatalog(db, "Test_Catalog");  
      ftc.IsDefault = true;  
  
      // Create the Full-Text Search catalog on the instance of SQL Server.  
      ftc.Create();  
  
      // Define a FullTextIndex object varaible by supplying the parent table argument in the constructor.  
      FullTextIndex fti = default(FullTextIndex);  
      fti = new FullTextIndex(tb);  
  
      // Define a FullTextIndexColumn object variable by supplying the parent index and column name arguments in the constructor.  
      FullTextIndexColumn ftic = default(FullTextIndexColumn);  
      ftic = new FullTextIndexColumn(fti, "Name");  
  
      // Add the indexed column to the index.  
      fti.IndexedColumns.Add(ftic);  
      fti.ChangeTracking = ChangeTracking.Automatic;  
  
      // Specify the unique index on the table that is required by the Full Text Search index.  
      fti.UniqueIndexName = "AK_ProductCategory_Name";  
  
      // Specify the catalog associated with the index.  
      fti.CatalogName = "Test_Catalog";  
  
      // Create the Full Text Search index on the instance of SQL Server.  
      fti.Create();  
   }  
}  
```  
  
## <a name="creating-a-full-text-search-service-in-powershell"></a>在 PowerShell 中创建全文搜索服务  
 此代码示例为 [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] 示例数据库中的 `ProductCategory` 表创建全文搜索目录。 然后，它会为 `ProductCategory` 表中的 Name 列创建全文搜索索引。 全文搜索索引要求已为该列定义唯一索引。  
  
```  
# Example of implementing a full text search on the default instance.  
# Set the path context to the local, default instance of SQL Server and database tables  
  
CD \sql\localhost\default\databases  
$db = get-item AdventureWorks2012  
  
CD AdventureWorks\tables  
  
#Get a reference to the table  
$tb = get-item Production.ProductCategory  
  
# Define a FullTextCatalog object variable by specifying the parent database and name arguments in the constructor.  
  
$ftc = New-Object -TypeName Microsoft.SqlServer.Management.SMO.FullTextCatalog -argumentlist $db, "Test_Catalog2"  
$ftc.IsDefault = $true  
  
# Create the Full Text Search catalog on the instance of SQL Server.  
$ftc.Create()  
  
# Define a FullTextIndex object variable by supplying the parent table argument in the constructor.  
$fti = New-Object -TypeName Microsoft.SqlServer.Management.SMO.FullTextIndex -argumentlist $tb  
  
#  Define a FullTextIndexColumn object variable by supplying the parent index   
#  and column name arguments in the constructor.  
  
$ftic = New-Object -TypeName Microsoft.SqlServer.Management.SMO.FullTextIndexColumn -argumentlist $fti, "Name"  
  
# Add the indexed column to the index.  
$fti.IndexedColumns.Add($ftic)  
  
# Set change tracking  
$fti.ChangeTracking = [Microsoft.SqlServer.Management.SMO.ChangeTracking]::Automatic  
  
# Specify the unique index on the table that is required by the Full Text Search index.  
$fti.UniqueIndexName = "AK_ProductCategory_Name"  
  
# Specify the catalog associated with the index.  
$fti.CatalogName = "Test_Catalog2"  
  
# Create the Full Text Search Index  
$fti.Create()  
```  
  
  
