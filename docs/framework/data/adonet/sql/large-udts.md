---
title: "Большие, определяемые пользователем типы | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 420ae24e-762b-4e09-b4c3-2112c470ee49
caps.latest.revision: 6
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 6
---
# Большие, определяемые пользователем типы
Определяемые пользователем типы данных \(UDT\) позволяют разработчику расширить систему скалярных типов путем сохранения в базе данных SQL Server объектов среды CLR.  Определяемые пользователем типы могут содержать несколько элементов в отличие от традиционных псевдонимов типов данных, состоящих из одного системного типа данных SQL Server.  
  
> [!NOTE]
>  Чтобы воспользоваться расширенной поддержкой SqlClient определяемых пользователем типов данных большого размера, необходимо установить .NET Framework 3.5 с пакетом обновления 1 \(SP1\) или более поздней версии.  
  
 Ранее максимальный размер определяемых пользователем типов данных был ограничен 8 килобайтами.  В SQL Server 2008 это ограничение было снято для определяемых пользователем типов данных в формате <xref:Microsoft.SqlServer.Server.Format>.  
  
 Полную документацию по определяемым пользователем типам данных см. в электронной документации по SQL Server для используемой версии SQL Server.  
  
 **Электронная документация по SQL Server**  
  
1.  [Определяемые пользователями типы данных CLR](http://go.microsoft.com/fwlink/?LinkId=98366)  
  
## Загрузка схем определяемых пользователем типов данных с помощью метода GetSchema  
 Метод <xref:System.Data.SqlClient.SqlConnection.GetSchema%2A> класса <xref:System.Data.SqlClient.SqlConnection> возвращает сведения о схеме базы данных в <xref:System.Data.DataTable>.  Для получения дополнительной информации см. [Коллекции схем SQL Server](../../../../../docs/framework/data/adonet/sql-server-schema-collections.md).  
  
### Значения столбца GetSchemaTable для определяемых пользователем типов данных  
 Метод <xref:System.Data.SqlClient.SqlDataReader.GetSchemaTable%2A> класса <xref:System.Data.SqlClient.SqlDataReader> возвращает объект <xref:System.Data.DataTable> с описанием метаданных столбцов.  В приведенной ниже таблице описаны различия метаданных столбцов для определяемых пользователем типов данных в SQL Server 2005 и SQL Server 2008.  
  
|Столбец SqlDataReader|SQL Server 2005|SQL Server 2008 и более поздние версии|  
|---------------------------|---------------------|--------------------------------------------|  
|`ColumnSize`|Возможны разные варианты|Возможны разные варианты|  
|`NumericPrecision`|255|255|  
|`NumericScale`|255|255|  
|`DataType`|`Byte[]`|Экземпляр определяемого пользователем типа данных|  
|`ProviderSpecificDataType`|`SqlTypes.SqlBinary`|Экземпляр определяемого пользователем типа данных|  
|`ProviderType`|21 \(`SqlDbType.VarBinary`\)|29 \(`SqlDbType.Udt`\)|  
|`NonVersionedProviderType`|29 \(`SqlDbType.Udt`\)|29 \(`SqlDbType.Udt`\)|  
|`DataTypeName`|`SqlDbType.VarBinary`|Имя из трех частей, указанное как *Database.SchemaName.TypeName*.|  
|`IsLong`|Возможны разные варианты|Возможны разные варианты|  
  
## Вопросы, связанные с SqlDataReader  
 Объект <xref:System.Data.SqlClient.SqlDataReader>, начиная с SQL Server 2008, был расширен для поддержки загрузки значений определяемых пользователем типов данных большого размера.  Способ обработки значений определяемых пользователем типов данных большого размера объектом [SqlDataReader](assetId:///SqlDataReader?qualifyHint=False&amp;autoUpgrade=True) зависит от используемой версии SQL Server, а также от значения параметра `Type System Version` в строке подключения.  Для получения дополнительной информации см. <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>.  
  
 Следующие методы <xref:System.Data.SqlClient.SqlDataReader> возвращают <xref:System.Data.SqlTypes.SqlBinary> вместо определяемого пользователем типа, если в SQL Server 2005 установлен аргумент `Type System Version`:  
  
-   <xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificFieldType%2A>  
  
-   <xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificValue%2A>  
  
-   <xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificValues%2A>  ``  
  
-   <xref:System.Data.SqlClient.SqlDataReader.GetSqlValue%2A>  ``  
  
-   <xref:System.Data.SqlClient.SqlDataReader.GetSqlValues%2A>  
  
 Следующие методы возвращают массив `Byte[]` вместо определяемого пользователем типа, если в SQL Server 2005 установлен аргумент `Type System Version`:  
  
-   <xref:System.Data.SqlClient.SqlDataReader.GetValue%2A>  ``  
  
-   <xref:System.Data.SqlClient.SqlDataReader.GetValues%2A>  
  
 Обратите внимание, что для текущей версии ADO.NET преобразования не выполняются.  
  
## Указание параметров SqlParameters  
 Следующие свойства <xref:System.Data.SqlClient.SqlParameter> были расширены для работы с большими определяемыми пользователем типами.  
  
|Свойство SqlParameter|Описание|  
|---------------------------|--------------|  
|<xref:System.Data.SqlClient.SqlParameter.Value%2A>|Возвращает или задает объект, представляющий собой значение параметра.  Значение по умолчанию — null.  Свойство может принимать значения `SqlBinary`, `Byte[]` или управляемого объекта.|  
|<xref:System.Data.SqlClient.SqlParameter.SqlValue%2A>|Возвращает или задает объект, представляющий собой значение параметра.  Значение по умолчанию — null.  Свойство может принимать значения `SqlBinary`, `Byte[]` или управляемого объекта.|  
|<xref:System.Data.SqlClient.SqlParameter.Size%2A>|Возвращает или задает размер значения параметра для разрешения.  Значение по умолчанию — 0.  Свойство может быть целым числом, представляющим размер значения параметра.  Для определяемых пользователем типов данных большого размера оно может равняться действительному размеру определяемого пользователем типа или принимать значение \-1 \- для неизвестных.|  
  
## Пример извлечения данных  
 В приведенном ниже фрагменте кода демонстрируется извлечение данных определяемого пользователем типа большого размера.  Предполагается, что в переменной `connectionString` хранится допустимая строка подключения к базе данных SQL Server, а в переменной `commandString` \- допустимая инструкция SELECT, в которой столбец первичного ключа указан первым.  
  
```csharp  
using (SqlConnection connection = new SqlConnection(   
    connectionString, commandString))  
{  
  connection.Open();  
  SqlCommand command = new SqlCommand(commandString);  
  SqlDataReader reader = command.ExecuteReader();  
  while (reader.Read())  
  {  
    // Retrieve the value of the Primary Key column.  
    int id = reader.GetInt32(0);  
  
    // Retrieve the value of the UDT.  
    LargeUDT udt = (LargeUDT)reader[1];  
  
    // You can also use GetSqlValue and GetValue.  
    // LargeUDT udt = (LargeUDT)reader.GetSqlValue(1);  
    // LargeUDT udt = (LargeUDT)reader.GetValue(1);  
  
    Console.WriteLine(  
     "ID={0} LargeUDT={1}", id, udt);  
  }  
reader.close  
}  
```  
  
```vb  
Using connection As New SqlConnection( _  
    connectionString, commandString)  
    connection.Open()  
    Dim command As New SqlCommand(commandString, connection)  
    Dim reader As SqlDataReader  
    reader = command.ExecuteReader  
  
    While reader.Read()  
      ' Retrieve the value of the Primary Key column.  
      Dim id As Int32 = reader.GetInt32(0)  
  
      ' Retrieve the value of the UDT.  
      Dim udt As LargeUDT = CType(reader(1), LargeUDT)  
  
     ' You can also use GetSqlValue and GetValue.  
     ' Dim udt As LargeUDT = CType(reader.GetSqlValue(1), LargeUDT)  
     ' Dim udt As LargeUDT = CType(reader.GetValue(1), LargeUDT)  
  
      ' Print values.  
      Console.WriteLine("ID={0} LargeUDT={1}", id, udt)  
    End While  
    reader.Close()  
End Using  
  
```  
  
## См. также  
 [Настройка параметров и типы данных параметров](../../../../../docs/framework/data/adonet/configuring-parameters-and-parameter-data-types.md)   
 [Получение сведений о схеме базы данных](../../../../../docs/framework/data/adonet/retrieving-database-schema-information.md)   
 [Сопоставления типов данных SQL Server](../../../../../docs/framework/data/adonet/sql-server-data-type-mappings.md)   
 [Двоичные данные и данные большого размера SQL Server](../../../../../docs/framework/data/adonet/sql/sql-server-binary-and-large-value-data.md)   
 [Центр разработчиков, поставщики ADO.NET Managed Provider и набор данных](http://go.microsoft.com/fwlink/?LinkId=217917)