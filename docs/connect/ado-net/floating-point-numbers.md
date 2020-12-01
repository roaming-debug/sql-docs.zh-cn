---
title: 浮点数
description: 了解在 Microsoft SqlClient Data Provider for SQL Server 中使用浮点数时遇到的一些问题。
ms.date: 11/13/2020
ms.assetid: 73c218c6-1c44-4402-a167-4f6262629a91
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 12c9255e0f05d338d1c5cbe88019a9f288e4e8a5
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126334"
---
# <a name="floating-point-numbers"></a>浮点数

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

本主题介绍了开发人员在 Microsoft SqlClient Data Provider for SQL Server 中使用浮点数时经常遇到的一些问题。 这些问题是由计算机存储浮点数的方式所导致的，并不是特定提供程序（如 <xref:Microsoft.Data.SqlClient>）所特有的。

浮点数一般没有准确的二进制表示形式， 计算机只是存储数字的近似值。 不同情况下，可能会使用不同的二进制位数来表示同一数字。 当浮点数从一种表示形式转换为另一种表示形式时，该数字的最低有效位数可能稍有变化。 当数字从一种类型强制转换为另一种类型时，通常会发生转换。 无论转换是发生在数据库中表示数据库值的类型之间，还是发生在类型之间，都会产生差异。 由于这些变化，逻辑上相等的数字其最低有效位数可能会发生变化，从而导致它们具有不同的值。 数字中的精度位数可能比预期的更大或更小。 将数字的格式设置为字符串时，可能不会显示预期值。

若要最大限度地减少这些影响，应在 numeric 类型之间使用您可用的最近似匹配。 例如，如果你使用的是 SQL Server，那么当你将实类型的 Transact-SQL 值转换为浮点类型的值时，精确数值可能发生变化。 在 .NET Framework 中，将 <xref:System.Single> 转换为 <xref:System.Double> 还可能产生意外结果。 在这两种情况下，比较好的方法是使应用程序中的所有值都使用同一 numeric 类型。 您也可以使用固定精度的十进制类型，或者在使用浮点数之前将它们强制转换为固定精度的十进制类型。

若要使用相等比较来解决这些问题，请考虑对您的应用程序进行编码，以便忽略最低有效位数的变化。 例如，用一个数减去另一个数，而不是通过比较，来了解两个数字是否相等。 如果差异在可接受舍入范围内，您的应用程序可以将这些数视为同一个数。
