---
title: "Возвращающие табличное значение параметры | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 370c16d5-db7b-43e3-945b-ccaab35b739b
caps.latest.revision: 5
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 5
---
# Возвращающие табличное значение параметры
Возвращающие табличное значение параметры обеспечивают легкий способ упаковки строк данных из клиентского приложения в [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)], не требуя многочисленных циклов приема\-передачи или специальной логики на стороне сервера для обработки данных.  Возвращающие табличное значение параметры можно использовать для инкапсуляции строк данных в клиентском приложении и отправки данных на сервер с помощью одной параметризированной команды.  Входящие строки данных сохраняются в табличной переменной, с которой затем можно работать, используя язык [!INCLUDE[tsql](../../../../../includes/tsql-md.md)].  
  
 Доступ к значениям столбца в возвращающих табличное значение параметрах обеспечивается с помощью стандартных инструкций SELECT [!INCLUDE[tsql](../../../../../includes/tsql-md.md)].  Возвращающие табличное значение параметры строго типизированы, а проверка их структуры выполняется автоматически.  Объем, занимаемый возвращающими табличное значение параметрами, ограничен только размерами памяти на сервере.  
  
> [!NOTE]
>  В возвращающий табличное значение параметр нельзя вернуть данные.  Возвращающие табличное значение параметры являются исключительно входными. Ключевое слово OUTPUT не поддерживается.  
  
 Дополнительные сведения о возвращающих табличное значение параметрах см. в приведенных ниже ресурсах.  
  
|Ресурс|Описание|  
|------------|--------------|  
|[Возвращающие табличное значение параметры \(компонент Database Engine\)](http://go.microsoft.com/fwlink/?LinkId=98363) в электронной документации [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)]|Описывается создание и использование возвращающих табличное значение параметров.|  
|[Определяемые пользователем табличные типы](http://go.microsoft.com/fwlink/?LinkId=98364) в электронной документации [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)]|Описывается использование определяемых пользователем табличных типов, предназначенных для объявления возвращающих табличное значение параметров.|  
|Раздел [Microsoft SQL Server Database Engine](http://go.microsoft.com/fwlink/?LinkId=120507) на веб\-сайте [CodePlex](http://go.microsoft.com/fwlink/?LinkID=67739)|Содержит образцы, в которых демонстрируется использование возможностей и функций [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)].|  
  
## Передача нескольких строк в предыдущие версии SQL Server  
 До появления в [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)] 2008 параметров, возвращающих табличное значение, были ограниченны возможности передачи нескольких строк данных в хранимую процедуру или параметризованную команду SQL.  При передаче нескольких строк на сервер разработчик может воспользоваться одним из описанных ниже вариантов.  
  
-   Использование ряда отдельных параметров, представляющих значения в нескольких столбцах и строках данных.  Объем данных, который можно передать с помощью этого метода, ограничен максимально допустимым количеством параметров.  В процедурах [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)] можно использовать максимум 2100 параметров.  Для сборки отдельных значений в табличную переменную или временную таблицу для дальнейшей обработки необходимо реализовать логику на стороне сервера.  
  
-   Объединение нескольких значений данных в строки с разделителями или XML\-документы и последующая передача этих текстовых значений в процедуру или инструкцию.  При этом в процедуру или инструкцию необходимо включить логику для проверки структур данных и разделения значений.  
  
-   Создание ряда отдельных инструкций SQL для изменения данных в нескольких строках, например инструкций, создаваемых при вызове метода `Update` класса <xref:System.Data.SqlClient.SqlDataAdapter>.  Изменения можно отправить на сервер по отдельности или объединенными в группы.  Тем не менее, даже при отправке в пакетах из нескольких инструкций, каждая инструкция выполняется на сервере отдельно.  
  
-   Чтобы загрузить несколько строк данных в таблицу, воспользуйтесь служебной программой `bcp` или объектом <xref:System.Data.SqlClient.SqlBulkCopy>.  Хотя этот метод весьма эффективен, в нем не поддерживается обработка данных на стороне сервера, если данные не загружены во временную таблицу или табличную переменную.  
  
## Создание типов параметров, возвращающих табличное значение  
 Возвращающие табличное значение параметры основаны на строго типизированных табличных структурах, заданных с помощью инструкций [!INCLUDE[tsql](../../../../../includes/tsql-md.md)] CREATE TYPE.  Перед использованием в клиентских приложениях возвращающих табличное значение параметров в [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)] необходимо создать табличный тип и определить структуру.  Дополнительные сведения о создании типов таблиц см. в разделе [Определяемые пользователем табличные типы](http://go.microsoft.com/fwlink/?LinkId=98364) в электронной документации [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)].  
  
 Приведенная ниже инструкция создает табличный тип с именем CategoryTableType, которая состоит из столбцов CategoryID и CategoryName.  
  
```  
CREATE TYPE dbo.CategoryTableType AS TABLE  
    ( CategoryID int, CategoryName nvarchar(50) )  
```  
  
 После создания табличного типа можно объявить возвращающие табличное значение параметры, основанные на этом типе.  В приведенном ниже фрагменте кода [!INCLUDE[tsql](../../../../../includes/tsql-md.md)] демонстрируется объявление возвращающего табличное значение параметра в определении хранимой процедуры.  Обратите внимание, что для объявления возвращающего табличное значение параметра необходимо использовать ключевое слово READONLY.  
  
```  
CREATE PROCEDURE usp_UpdateCategories   
    (@tvpNewCategories dbo.CategoryTableType READONLY)  
```  
  
## Изменение данных с помощью возвращающих табличное значение параметров \(Transact\-SQL\)  
 Возвращающие табличное значение параметры можно использовать для изменения данных на основе наборов, при котором в одной инструкции затрагивается несколько строк.  Например, можно выбрать все строки возвращающего табличное значение параметра и вставить их в таблицу базы данных или создать инструкцию UPDATE, соединив возвращающий табличное значение параметр с таблицей, которую необходимо обновить.  
  
 В приведенной ниже инструкции [!INCLUDE[tsql](../../../../../includes/tsql-md.md)] UPDATE демонстрируется соединение возвращающего табличное значение параметра с таблицей Categories.  При использовании возвращающего табличное значение параметра в операторе JOIN предложения FROM необходимо создать для этого параметра псевдоним; как показано здесь, для возвращающего табличное значение параметра используется псевдоним «ec»:  
  
```  
UPDATE dbo.Categories  
    SET Categories.CategoryName = ec.CategoryName  
    FROM dbo.Categories INNER JOIN @tvpEditedCategories AS ec  
    ON dbo.Categories.CategoryID = ec.CategoryID;  
```  
  
 В данном примере кода [!INCLUDE[tsql](../../../../../includes/tsql-md.md)] демонстрируется выбор строк из возвращающего табличное значение параметра для выполнения инструкции INSERT в одной операции на основе набора данных.  
  
```  
INSERT INTO dbo.Categories (CategoryID, CategoryName)  
    SELECT nc.CategoryID, nc.CategoryName FROM @tvpNewCategories AS nc;  
```  
  
## Ограничения возвращающих табличное значение параметров  
 На использование возвращающих табличное значение параметров накладывается ряд указанных ниже ограничений.  
  
-   Возвращающие табличное значение параметры нельзя передавать [определяемым пользователем функциям CLR](http://msdn.microsoft.com/library/ms131077.aspx).  
  
-   Возвращающие табличное значение параметры можно индексировать только для поддержки ограничений UNIQUE и PRIMARY KEY.  [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)] не ведет статистику по возвращающим табличное значение параметрам.  
  
-   В коде [!INCLUDE[tsql](../../../../../includes/tsql-md.md)] возвращающие табличное значение параметры предназначены только для чтения.  Значения столбцов в строках возвращающего табличное значение параметра нельзя обновить; также нельзя вставлять и удалять строки.  Чтобы изменить данные, передаваемые в хранимую процедуру или параметризованную инструкцию через возвращающий табличное значение параметр, необходимо вставить данные во временную таблицу или табличную переменную.  
  
-   Для изменения структуры возвращающего табличное значение параметра нельзя использовать инструкции ALTER TABLE.  
  
## Пример настройки параметра SqlParameter  
 Поставщик <xref:System.Data.SqlClient> поддерживает заполнение возвращающих табличное значение параметров из объектов <xref:System.Data.DataTable>, <xref:System.Data.Common.DbDataReader> или System.Collections.Generic.IEnumerable\<<xref:Microsoft.SqlServer.Server.SqlDataRecord>\> \(<xref:System.Collections.Generic.IEnumerable%601>?qualifyHint=False&autoUpgrade=True).  Необходимо указать имя типа возвращающего табличное значение параметра с помощью свойства <xref:System.Data.SqlClient.SqlParameter.TypeName%2A> объекта <xref:System.Data.SqlClient.SqlParameter>.  Значение `TypeName` должно совпадать с именем совместимого типа, созданного ранее на сервере.  В приведенном ниже фрагменте кода демонстрируется настройка объекта <xref:System.Data.SqlClient.SqlParameter> для вставки данных.  
  
```csharp  
// Configure the command and parameter.  
SqlCommand insertCommand = new SqlCommand(  
    sqlInsert, connection);  
SqlParameter tvpParam = insertCommand.Parameters.AddWithValue(  
    "@tvpNewCategories", addedCategories);  
tvpParam.SqlDbType = SqlDbType.Structured;  
tvpParam.TypeName = "dbo.CategoryTableType";  
```  
  
```vb  
' Configure the command and parameter.  
Dim insertCommand As New SqlCommand(sqlInsert, connection)  
Dim tvpParam As SqlParameter = _  
   insertCommand.Parameters.AddWithValue( _  
  "@tvpNewCategories", addedCategories)  
tvpParam.SqlDbType = SqlDbType.Structured  
tvpParam.TypeName = "dbo.CategoryTableType"  
  
```  
  
 Также для передачи строк данных в возвращающий табличное значение параметр можно использовать любой производный от <xref:System.Data.Common.DbDataReader> объект, как показано в этом фрагменте.  
  
```csharp  
// Configure the SqlCommand and table-valued parameter.  
SqlCommand insertCommand = new SqlCommand(  
  "usp_InsertCategories", connection);  
insertCommand.CommandType = CommandType.StoredProcedure;  
SqlParameter tvpParam =   
   insertCommand.Parameters.AddWithValue(  
   "@tvpNewCategories", dataReader);  
tvpParam.SqlDbType = SqlDbType.Structured;  
```  
  
```vb  
' Configure the SqlCommand and table-valued parameter.  
Dim insertCommand As New SqlCommand("usp_InsertCategories", connection)  
insertCommand.CommandType = CommandType.StoredProcedure  
Dim tvpParam As SqlParameter = _  
  insertCommand.Parameters.AddWithValue("@tvpNewCategories", _  
  dataReader)  
tvpParam.SqlDbType = SqlDbType.Structured  
  
```  
  
## Передача возвращающего табличное значение параметра в хранимую процедуру  
 В данном примере демонстрируется передача возвращающего табличное значение параметра в хранимую процедуру.  Добавленные строки извлекаются в коде в новый объект <xref:System.Data.DataTable> с помощью метода <xref:System.Data.DataTable.GetChanges%2A>.  Затем в коде задается команда <xref:System.Data.SqlClient.SqlCommand>, присваивающая свойству <xref:System.Data.SqlClient.SqlCommand.CommandType%2A> значение <xref:System.Data.CommandType>.  Объект <xref:System.Data.SqlClient.SqlParameter> заполняется с помощью метода <xref:System.Data.SqlClient.SqlParameterCollection.AddWithValue%2A>, а свойству <xref:System.Data.SqlClient.SqlParameter.SqlDbType%2A> присваивается значение `Structured`.  Затем с помощью метода выполняется <xref:System.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> команда <xref:System.Data.SqlClient.SqlCommand>.  
  
```csharp  
// Assumes connection is an open SqlConnection object.  
using (connection)  
{  
// Create a DataTable with the modified rows.  
DataTable addedCategories =  
  CategoriesDataTable.GetChanges(DataRowState.Added);  
  
// Configure the SqlCommand and SqlParameter.  
SqlCommand insertCommand = new SqlCommand(  
    "usp_InsertCategories", connection);  
insertCommand.CommandType = CommandType.StoredProcedure;  
SqlParameter tvpParam = insertCommand.Parameters.AddWithValue(  
    "@tvpNewCategories", addedCategories);  
tvpParam.SqlDbType = SqlDbType.Structured;  
  
// Execute the command.  
insertCommand.ExecuteNonQuery();  
}  
```  
  
```vb  
' Assumes connection is an open SqlConnection object.  
Using connection  
   '  Create a DataTable with the modified rows.  
   Dim addedCategories As DataTable = _  
     CategoriesDataTable.GetChanges(DataRowState.Added)  
  
  ' Configure the SqlCommand and SqlParameter.  
   Dim insertCommand As New SqlCommand( _  
     "usp_InsertCategories", connection)  
   insertCommand.CommandType = CommandType.StoredProcedure  
   Dim tvpParam As SqlParameter = _  
     insertCommand.Parameters.AddWithValue( _  
     "@tvpNewCategories", addedCategories)  
   tvpParam.SqlDbType = SqlDbType.Structured  
  
   '  Execute the command.  
   insertCommand.ExecuteNonQuery()  
End Using  
```  
  
### Передача возвращающего табличное значение параметра в параметризованную инструкцию SQL  
 В приведенном ниже примере кода демонстрируется вставка данных в таблицу dbo.Categories с помощью инструкции INSERT с вложенным запросом SELECT, для которого в качестве источника данных указан возвращающий табличное значение параметр.  При передаче в параметризованную инструкцию SQL возвращающего табличное значение параметра необходимо указать для этого параметра имя типа с помощью нового свойства <xref:System.Data.SqlClient.SqlParameter.TypeName%2A> класса <xref:System.Data.SqlClient.SqlParameter>.  Значение `TypeName` должно совпадать с именем совместимого типа, созданного ранее на сервере.  Для ссылки на структуру типов, заданную в dbo.CategoryTableType, в данном примере кода используется свойство `TypeName`.  
  
> [!NOTE]
>  При указании значения для столбца идентификаторов в возвращающем табличное значение параметре необходимо выполнить для сеанса инструкцию SET IDENTITY\_INSERT.  
  
```csharp  
// Assumes connection is an open SqlConnection.  
using (connection)  
{  
// Create a DataTable with the modified rows.  
DataTable addedCategories = CategoriesDataTable.GetChanges(  
    DataRowState.Added);  
  
// Define the INSERT-SELECT statement.  
string sqlInsert =   
    "INSERT INTO dbo.Categories (CategoryID, CategoryName)"  
    + " SELECT nc.CategoryID, nc.CategoryName"  
    + " FROM @tvpNewCategories AS nc;"  
  
// Configure the command and parameter.  
SqlCommand insertCommand = new SqlCommand(  
    sqlInsert, connection);  
SqlParameter tvpParam = insertCommand.Parameters.AddWithValue(  
    "@tvpNewCategories", addedCategories);  
tvpParam.SqlDbType = SqlDbType.Structured;  
tvpParam.TypeName = "dbo.CategoryTableType";  
  
// Execute the command.  
insertCommand.ExecuteNonQuery();  
}  
```  
  
```vb  
' Assumes connection is an open SqlConnection.  
Using connection  
  ' Create a DataTable with the modified rows.  
  Dim addedCategories As DataTable = _  
    CategoriesDataTable.GetChanges(DataRowState.Added)  
  
  ' Define the INSERT-SELECT statement.  
  Dim sqlInsert As String = _  
  "INSERT INTO dbo.Categories (CategoryID, CategoryName)" _  
  & " SELECT nc.CategoryID, nc.CategoryName" _  
  & " FROM @tvpNewCategories AS nc;"  
  
  ' Configure the command and parameter.  
  Dim insertCommand As New SqlCommand(sqlInsert, connection)  
  Dim tvpParam As SqlParameter = _  
     insertCommand.Parameters.AddWithValue( _  
    "@tvpNewCategories", addedCategories)  
  tvpParam.SqlDbType = SqlDbType.Structured  
  tvpParam.TypeName = "dbo.CategoryTableType"  
  
  ' Execute the query  
  insertCommand.ExecuteNonQuery()  
End Using  
```  
  
## Потоковая передача строк с помощью объекта DataReader  
 Также для потоковой передачи строк данных в возвращающий табличное значение параметр можно использовать любой производный от <xref:System.Data.Common.DbDataReader> класс.  В приведенном ниже фрагменте кода демонстрируется загрузка данных из базы данных Oracle с помощью <xref:System.Data.OracleClient.OracleCommand> и <xref:System.Data.OracleClient.OracleDataReader>.  Затем в коде настраивается команда <xref:System.Data.SqlClient.SqlCommand>, предназначенная для вызова хранимой процедуры с одним входным параметром.  Свойству <xref:System.Data.SqlClient.SqlParameter.SqlDbType%2A> класса <xref:System.Data.SqlClient.SqlParameter> присваивается значение `Structured`.  Метод <xref:System.Data.SqlClient.SqlParameterCollection.AddWithValue%2A> передает результирующий набор `OracleDataReader` в хранимую процедуру в виде возвращающего табличное значение параметра.  
  
```csharp  
// Assumes connection is an open SqlConnection.  
// Retrieve data from Oracle.  
OracleCommand selectCommand = new OracleCommand(  
   "Select CategoryID, CategoryName FROM Categories;",  
   oracleConnection);  
OracleDataReader oracleReader = selectCommand.ExecuteReader(  
   CommandBehavior.CloseConnection);  
  
 // Configure the SqlCommand and table-valued parameter.  
 SqlCommand insertCommand = new SqlCommand(  
   "usp_InsertCategories", connection);  
 insertCommand.CommandType = CommandType.StoredProcedure;  
 SqlParameter tvpParam =   
    insertCommand.Parameters.AddWithValue(  
    "@tvpNewCategories", oracleReader);  
 tvpParam.SqlDbType = SqlDbType.Structured;  
  
 // Execute the command.  
 insertCommand.ExecuteNonQuery();  
```  
  
```vb  
' Assumes connection is an open SqlConnection.  
' Retrieve data from Oracle.  
Dim selectCommand As New OracleCommand( _  
  "Select CategoryID, CategoryName FROM Categories;", _  
  oracleConnection)  
Dim oracleReader As OracleDataReader = _  
  selectCommand.ExecuteReader(CommandBehavior.CloseConnection)  
  
' Configure SqlCommand and table-valued parameter.  
Dim insertCommand As New SqlCommand("usp_InsertCategories", connection)  
insertCommand.CommandType = CommandType.StoredProcedure  
Dim tvpParam As SqlParameter = _  
  insertCommand.Parameters.AddWithValue("@tvpNewCategories", _  
  oracleReader)  
tvpParam.SqlDbType = SqlDbType.Structured  
  
' Execute the command.  
insertCommand.ExecuteNonQuery()  
```  
  
## См. также  
 [Настройка параметров и типы данных параметров](../../../../../docs/framework/data/adonet/configuring-parameters-and-parameter-data-types.md)   
 [Команды и параметры](../../../../../docs/framework/data/adonet/commands-and-parameters.md)   
 [Параметры DataAdapter](../../../../../docs/framework/data/adonet/dataadapter-parameters.md)   
 [Операции с данными SQL Server в ADO.NET](../../../../../docs/framework/data/adonet/sql/sql-server-data-operations.md)   
 [Центр разработчиков, поставщики ADO.NET Managed Provider и набор данных](http://go.microsoft.com/fwlink/?LinkId=217917)