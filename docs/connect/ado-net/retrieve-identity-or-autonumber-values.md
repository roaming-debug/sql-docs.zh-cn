---
title: 检索标识或自动编号值
description: 了解如何在 SQL Server 中检索主键的标识和自动编号值，以及如何在 ADO.NET 中合并新的标识值。
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: d6b7f9cb-81be-44e1-bb94-56137954876d
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 4ca2199686359235ff1d2c834b0b19a88296030d
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744343"
---
# <a name="retrieve-identity-or-autonumber-values"></a>检索标识或自动编号值

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

关系数据库中的主键是始终包含唯一值的列或列的组合。 知道主键值使您可以定位包含该值的行。 关系数据库引擎（如 SQL Server、Oracle 和 Microsoft Access/Jet）支持创建可指定为主键的自动递增列。 在向表中添加行时，服务器会生成这些值。 在 SQL Server 中可以设置列的标识属性，在 Oracle 中可以创建序列，在 Microsoft Access 中可以创建 AutoNumber 列。

通过将 <xref:System.Data.DataColumn> 属性设置为 True，也可以使用 <xref:System.Data.DataColumn.AutoIncrement%2A> 来生成自动递增值。 但是，如果多个客户端应用程序独立生成自动递增值，最后会使 <xref:System.Data.DataTable> 的单独实例中出现重复的值。 让服务器生成自动递增值，以允许每个用户为每个插入的行检索生成的值，从而消除潜在的冲突。

在调用 `Update` 的 `DataAdapter` 方法期间，数据库可以将数据以输出参数的形式或以与 INSERT 语句同一批执行的 SELECT 语句返回的结果集的第一个记录的形式，发回到您的 ADO.NET 应用程序。 用于 SQL Server 的 Microsoft SqlClient 数据提供程序可以检索这些值，并更新要更新的 <xref:System.Data.DataRow> 中的相应列。

> [!NOTE]
> 使用自动递增值的替代方法是使用 <xref:System.Guid.NewGuid%2A> 对象的 <xref:System.Guid> 方法在客户端计算机上生成一个 GUID（即全局唯一标识符），插入每个新行时可以将此标识符复制到服务器。 `NewGuid` 方法使用算法生成一个 16 字节的二进制值，该算法不生成重复值的概率很高。 在 SQL Server 数据库中，GUID 存储在 `uniqueidentifier` 列中，SQL Server 可以使用 Transact-SQL `NEWID()` 函数自动生成此列。 将 GUID 用作主键会对性能产生负面影响。 SQL Server 引入对 `NEWSEQUENTIALID()` 函数的支持，此函数生成一个连续 GUID，虽然不能保证此 GUID 全局唯一，却可以更有效地进行索引。

## <a name="retrieve-sql-server-identity-column-values"></a>检索 SQL Server 标识列值

使用 Microsoft SQL Server 时，可以创建通过输出参数返回插入行标识值的存储过程。 下表说明 SQL Server 中可用来检索标识列值的三个 Transact-SQL 函数。

|函数|说明|
|--------------|-----------------|
|SCOPE_IDENTITY|返回当前执行范围内的最后一个标识值。 对于多数方案，建议使用 SCOPE_IDENTITY。|
|@@IDENTITY|包含当前会话中任何表生成的最后一个标识值。 @@IDENTITY 会受到触发器的影响，可能不会返回预期的标识值。|
|IDENT_CURRENT|返回在任何会话中以及任何范围内为特定表生成的最后一个标识值。|

下面的存储过程演示如何向“类别”表中插入行，并使用输出参数返回由 Transact-SQL SCOPE_IDENTITY() 函数生成的新标识值。

```sql
CREATE PROCEDURE dbo.InsertCategory
  @CategoryName nvarchar(15),
  @Identity int OUT
AS
INSERT INTO Categories (CategoryName) VALUES(@CategoryName)
SET @Identity = SCOPE_IDENTITY()
```

然后可以将此存储过程指定为 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> 对象的 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 的源。 <xref:Microsoft.Data.SqlClient.SqlCommand.CommandType%2A> 的 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> 属性必须设置为 <xref:System.Data.CommandType.StoredProcedure>。 可以通过创建一个 <xref:Microsoft.Data.SqlClient.SqlParameter> 为 <xref:System.Data.ParameterDirection> 的 <xref:System.Data.ParameterDirection.Output> 来检索标识输出。 如果将插入命令的 <xref:Microsoft.Data.SqlClient.SqlCommand.UpdatedRowSource%2A> 属性设置为 `UpdateRowSource.OutputParameters` 或 `UpdateRowSource.Both`，则在处理 `InsertCommand` 时，会返回自动递增的标识值并将其置于当前行的“CategoryID”列中。

如果插入命令执行的是同时包括 INSERT 语句和可返回新标识值的 SELECT 语句的批处理，则可以通过将插入命令的 `UpdatedRowSource` 属性设置为 `UpdateRowSource.FirstReturnedRecord` 来检索这个新值。

[!code-csharp[DataWorks SqlClient.RetrieveIdentityStoredProcedure#1](~/../sqlclient/doc/samples/SqlDataAdapter_RetrieveIdentityStoredProcedure.cs#1)]

## <a name="merge-new-identity-values"></a>合并新标识值

常见的方案是调用 `GetChanges` 的 `DataTable` 方法来创建仅包含已更改行的副本，并在调用 `Update` 的 `DataAdapter` 方法时使用这个新副本。 这在需要将已更改的行封送到执行更新的单独组件时十分有用。 更新后，此副本会包含随后必须合并回原始 `DataTable` 中的新标识值。 新标识值很可能会不同于 `DataTable` 中的原始值。 若要完成合并，必须保留副本中“AutoIncrement”列的原始值，以便能够在原始 `DataTable` 中定位和更新现有行，而不是追加包含新标识值的新行。 但在默认情况下，在调用 `Update` 的 `DataAdapter` 方法后，这些原始值会丢失，这是因为会对每个已更新的 `AcceptChanges` 隐式调用 `DataRow`。

在 `DataColumn` 更新期间，可以采用两种方法保留 `DataRow` 中 `DataAdapter` 的原始值：

- 第一种保留原始值的方法是将 `AcceptChangesDuringUpdate` 的 `DataAdapter` 属性设置为 `false`。 此配置会影响要更新的 `DataTable` 中的每个 `DataRow`。 有关更多信息及代码示例，请参阅 <xref:System.Data.Common.DataAdapter.AcceptChangesDuringUpdate%2A>。

- 第二种方法是在 `RowUpdated` 的 `DataAdapter` 事件处理程序中编写代码，将 <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> 设置为 <xref:System.Data.UpdateStatus.SkipCurrentRow>。 `DataRow` 将会更新，但每个 `DataColumn` 的原始值会保留。 使用此方法可以对某些行保留原始值而不对其他行保留原始值。 例如，通过先检查 <xref:System.Data.Common.RowUpdatedEventArgs.StatementType%2A> 然后仅针对 <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> 为 <xref:System.Data.UpdateStatus.SkipCurrentRow> 的行将 `StatementType` 设置为 `Insert`，代码可以为已添加的行保留原始值，而不为已编辑或已删除的行保留原始值。

当使用这些方法中的任一方法在 `DataAdapter` 更新期间保留 `DataRow` 中的原始值时，用于 SQL Server 的 Microsoft SqlClient 数据适配器将执行一系列操作，将 `DataRow` 的当前值设置为输出参数或结果集的第一个返回行返回的新值，同时仍然保留每个 `DataColumn` 中的原始值。 首先调用 `AcceptChanges` 的 `DataRow` 方法将当前值保留为原始值，然后分配新值。 完成这些操作后，`DataRows` 属性设置为 <xref:System.Data.DataRow.RowState%2A> 的 <xref:System.Data.DataRowState.Added> 将会使其 `RowState` 属性设置为 <xref:System.Data.DataRowState.Modified>，这可能是不需要的行为。

如何将命令结果应用于每个要更新的 <xref:System.Data.DataRow> 由每个 <xref:System.Data.Common.DbCommand.UpdatedRowSource%2A> 的 <xref:System.Data.Common.DbCommand> 属性确定。 此属性设置为 `UpdateRowSource` 枚举中的一个值。

下表说明 `UpdateRowSource` 枚举值如何影响已更新行的 <xref:System.Data.DataRow.RowState%2A> 属性。

|成员名称|描述|
|-----------------|-----------------|
|<xref:System.Data.UpdateRowSource.Both>|调用 `AcceptChanges` 并将输出参数值和/或任何返回的数据集的第一行中的值放在要更新的 `DataRow` 中。 如果没有要应用的值，则 `RowState` 将为 <xref:System.Data.DataRowState.Unchanged>。|
|<xref:System.Data.UpdateRowSource.FirstReturnedRecord>|如果返回一个行，则调用 `AcceptChanges` 并将此行映射到 `DataTable` 中已更改的行，同时将 `RowState` 设置为 `Modified`。 如果未返回任何行，则不调用 `AcceptChanges`，`RowState` 将保持为 `Added`。|
|<xref:System.Data.UpdateRowSource.None>|忽略任何返回的参数或行。 不对 `AcceptChanges` 进行调用，`RowState` 将保持为 `Added`。|
|<xref:System.Data.UpdateRowSource.OutputParameters>|调用 `AcceptChanges` 并将任何输出参数映射到 `DataTable` 中已更改的行，同时将 `RowState` 设置为 `Modified`。 如果没有输出参数，则 `RowState` 将为 `Unchanged`。|

### <a name="example"></a>示例

此示例演示从 `DataTable` 中提取已更改的行，然后使用 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 更新数据源并检索新标识列值。 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> 执行两个 Transact-SQL 语句：第一个是 INSERT 语句，第二个是使用 SCOPE_IDENTITY 函数检索标识值的 SELECT 语句。

```sql
INSERT INTO dbo.Shippers (CompanyName)
VALUES (@CompanyName);
SELECT ShipperID, CompanyName FROM dbo.Shippers
WHERE ShipperID = SCOPE_IDENTITY();
```

插入命令的 `UpdatedRowSource` 属性设置为 `UpdateRowSource.FirstReturnedRow` 并且 <xref:System.Data.MissingSchemaAction> 的 `DataAdapter` 属性设置为 `MissingSchemaAction.AddWithKey`。 填充 `DataTable` 并且代码向 `DataTable` 添加一个新行。 然后将已更改的行提取到一个新的 `DataTable` 中，将后者传递到 `DataAdapter`，它将随后更新服务器。

[!code-csharp[DataWorks SqlClient.MergeIdentity#1](~/../sqlclient/doc/samples/SqlDataAdapter_MergeIdentity.cs#1)]

`OnRowUpdated` 事件处理程序检查 <xref:System.Data.Common.RowUpdatedEventArgs.StatementType%2A> 的 <xref:Microsoft.Data.SqlClient.SqlRowUpdatedEventArgs> 以确定行是否为插入项。 如果是，则将 <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> 属性设置为 <xref:System.Data.UpdateStatus.SkipCurrentRow>。 该行将被更新，但会保留该行中的原始值。 在过程的正文中，调用 <xref:System.Data.DataSet.Merge%2A> 方法以将新标识值合并到原始 `DataTable` 中，最后调用 `AcceptChanges`。

[!code-csharp[DataWorks SqlClient.MergeIdentity#2](~/../sqlclient/doc/samples/SqlDataAdapter_MergeIdentity.cs#2)]

### <a name="retrieve-identity-values"></a>检索标识值

当列中的值必须唯一时，我们经常将该列设置为标识。 而且有时我们需要新数据的标识值。 此示例演示了如何检索标识值：

- 创建存储过程以插入数据和返回标识值。

- 执行命令以插入新数据并显示结果。

- 使用 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 以插入新数据并显示结果。

在你编译并运行该示例之前，你必须使用以下脚本创建示例数据库：

```sql
USE [master]
GO

CREATE DATABASE [MySchool]
GO

USE [MySchool]
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE procedure [dbo].[CourseExtInfo] @CourseId int
as
select c.CourseID,c.Title,c.Credits,d.Name as DepartmentName
from Course as c left outer join Department as d on c.DepartmentID=d.DepartmentID
where c.CourseID=@CourseId

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
create procedure [dbo].[DepartmentInfo] @DepartmentId int,@CourseCount int output
as
select @CourseCount=Count(c.CourseID)
from course as c
where c.DepartmentID=@DepartmentId

select d.DepartmentID,d.Name,d.Budget,d.StartDate,d.Administrator
from Department as d
where d.DepartmentID=@DepartmentId

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
Create PROCEDURE [dbo].[GetDepartmentsOfSpecifiedYear]
@Year int,@BudgetSum money output
AS
BEGIN
        SELECT @BudgetSum=SUM([Budget])
  FROM [MySchool].[dbo].[Department]
  Where YEAR([StartDate])=@Year

SELECT [DepartmentID]
      ,[Name]
      ,[Budget]
      ,[StartDate]
      ,[Administrator]
  FROM [MySchool].[dbo].[Department]
  Where YEAR([StartDate])=@Year

END
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE PROCEDURE [dbo].[GradeOfStudent]
-- Add the parameters for the stored procedure here
@CourseTitle nvarchar(100),@FirstName nvarchar(50),
@LastName nvarchar(50),@Grade decimal(3,2) output
AS
BEGIN
select @Grade=Max(Grade)
from [dbo].[StudentGrade] as s join [dbo].[Course] as c on
s.CourseID=c.CourseID join [dbo].[Person] as p on s.StudentID=p.PersonID
where c.Title=@CourseTitle and p.FirstName=@FirstName
and p.LastName= @LastName
END
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE PROCEDURE [dbo].[InsertPerson]
-- Add the parameters for the stored procedure here
@FirstName nvarchar(50),@LastName nvarchar(50),
@PersonID int output
AS
BEGIN
    insert [dbo].[Person](LastName,FirstName) Values(@LastName,@FirstName)

    set @PersonID=SCOPE_IDENTITY()
END
Go

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Course]([CourseID] [nvarchar](10) NOT NULL,
[Year] [smallint] NOT NULL,
[Title] [nvarchar](100) NOT NULL,
[Credits] [int] NOT NULL,
[DepartmentID] [int] NOT NULL,
 CONSTRAINT [PK_Course] PRIMARY KEY CLUSTERED
(
[CourseID] ASC,
[Year] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Department]([DepartmentID] [int] IDENTITY(1,1) NOT NULL,
[Name] [nvarchar](50) NOT NULL,
[Budget] [money] NOT NULL,
[StartDate] [datetime] NOT NULL,
[Administrator] [int] NULL,
 CONSTRAINT [PK_Department] PRIMARY KEY CLUSTERED
(
[DepartmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[Person]([PersonID] [int] IDENTITY(1,1) NOT NULL,
[LastName] [nvarchar](50) NOT NULL,
[FirstName] [nvarchar](50) NOT NULL,
[HireDate] [datetime] NULL,
[EnrollmentDate] [datetime] NULL,
[Picture] [varbinary](max) NULL,
 CONSTRAINT [PK_School.Student] PRIMARY KEY CLUSTERED
(
[PersonID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[StudentGrade]([EnrollmentID] [int] IDENTITY(1,1) NOT NULL,
[CourseID] [nvarchar](10) NOT NULL,
[StudentID] [int] NOT NULL,
[Grade] [decimal](3, 2) NOT NULL,
 CONSTRAINT [PK_StudentGrade] PRIMARY KEY CLUSTERED
(
[EnrollmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
create view [dbo].[EnglishCourse]
as
select c.CourseID,c.Title,c.Credits,c.DepartmentID
from Course as c join Department as d on c.DepartmentID=d.DepartmentID
where d.Name=N'English'

GO
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1045', 2012, N'Calculus', 4, 7)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1061', 2012, N'Physics', 4, 1)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2021', 2012, N'Composition', 3, 2)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2042', 2012, N'Literature', 4, 2)
SET IDENTITY_INSERT [dbo].[Department] ON

INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (1, N'Engineering', 350000.0000, CAST(0x0000999C00000000 AS DateTime), 2)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (2, N'English', 120000.0000, CAST(0x0000999C00000000 AS DateTime), 6)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (4, N'Economics', 200000.0000, CAST(0x0000999C00000000 AS DateTime), 4)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (7, N'Mathematics', 250024.0000, CAST(0x0000999C00000000 AS DateTime), 3)
SET IDENTITY_INSERT [dbo].[Department] OFF
SET IDENTITY_INSERT [dbo].[Person] ON

INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (1, N'Hu', N'Nan', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (2, N'Norman', N'Laura', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (3, N'Olivotto', N'Nino', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (4, N'Anand', N'Arturo', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (5, N'Jai', N'Damien', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (6, N'Holt', N'Roger', CAST(0x000097F100000000 AS DateTime), NULL)
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (7, N'Martin', N'Randall', CAST(0x00008B1A00000000 AS DateTime), NULL)
SET IDENTITY_INSERT [dbo].[Person] OFF
SET IDENTITY_INSERT [dbo].[StudentGrade] ON

INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (1, N'C1045', 1, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (2, N'C1045', 2, CAST(3.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (3, N'C1045', 3, CAST(2.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (4, N'C1045', 4, CAST(4.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (5, N'C1045', 5, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (6, N'C1061', 1, CAST(4.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (7, N'C1061', 3, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (8, N'C1061', 4, CAST(2.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (9, N'C1061', 5, CAST(1.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (10, N'C2021', 1, CAST(2.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (11, N'C2021', 2, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (12, N'C2021', 4, CAST(3.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (13, N'C2021', 5, CAST(3.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (14, N'C2042', 1, CAST(2.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (15, N'C2042', 2, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (16, N'C2042', 3, CAST(4.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (17, N'C2042', 5, CAST(3.00 AS Decimal(3, 2)))
SET IDENTITY_INSERT [dbo].[StudentGrade] OFF
ALTER TABLE [dbo].[Course]  WITH CHECK ADD  CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
REFERENCES [dbo].[Department] ([DepartmentID])
GO
ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department]
GO
ALTER TABLE [dbo].[StudentGrade]  WITH CHECK ADD  CONSTRAINT [FK_StudentGrade_Student] FOREIGN KEY([StudentID])
REFERENCES [dbo].[Person] ([PersonID])
GO
ALTER TABLE [dbo].[StudentGrade] CHECK CONSTRAINT [FK_StudentGrade_Student]
GO
```

代码列表如下：

[!code-csharp[SqlClient_RetrieveIdentity#1](~/../sqlclient/doc/samples/SqlClient_RetrieveIdentity.cs#1)]

## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-modifying-data.md)
- [DataAdapter 和 DataReader](dataadapters-datareaders.md)
- [通过 DataAdapter 更新数据源](update-data-sources-with-dataadapters.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
