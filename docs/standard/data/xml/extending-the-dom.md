---
title: "Расширение модели DOM | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
ms.assetid: b5489c96-4afd-439a-a25d-fc82eb4a148d
caps.latest.revision: 5
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 4
---
# Расширение модели DOM
Платформа Microsoft .NET Framework имеет базовый набор классов, обеспечивающих реализацию модели DOM.  Объект <xref:System.Xml.XmlNode> и его производные классы предоставляют методы и свойства, позволяющие переходить по содержимому и структуре XML\-документа, запрашивать и изменять это содержимое и структуру.  
  
 Если содержимое XML загружается в память с помощью модели DOM, созданные узлы содержат такие сведения, как имя узла, тип узла и так далее.  Могут быть случаи, когда необходимы конкретные данные об узле, которые не предоставляют базовые классы.  Например, может потребоваться узнать номер строки и позицию узла.  В этом случае можно произвести новые классы из существующих классов DOM, создав для них дополнительные возможности.  
  
 Есть две общие рекомендации для создания новых классов из существующих.  
  
-   Рекомендуется никогда не создавать производных классов от класса <xref:System.Xml.XmlNode>.  Вместо этого рекомендуется наследовать классы от класса, соответствующего типу интересующего вас узла.  Например, если необходимо возвращать дополнительную информацию об узлах атрибутов, можно унаследовать класс <xref:System.Xml.XmlAttribute>.  
  
-   За исключением методов создания узлов, при переопределении функции рекомендуется всегда вызывать базовую версию функции, а затем добавлять дополнительную обработку.  
  
## Создание собственных экземпляров узлов  
 Класс <xref:System.Xml.XmlDocument> содержит методы создания узлов.  Эти методы вызываются для создания узлов при загрузке XML\-файла.  Эти методы можно переопределять, чтобы собственные экземпляры узлов создавались при загрузке документа.  Например, при расширении класса <xref:System.Xml.XmlElement> был бы унаследован класс <xref:System.Xml.XmlDocument> и переопределен метод <xref:System.Xml.XmlDocument.CreateElement%2A>.  
  
 В следующем примере показано, как переопределить метод <xref:System.Xml.XmlDocument.CreateElement%2A>, чтобы возвратить собственную реализацию класса <xref:System.Xml.XmlElement>.  
  
```vb  
Class LineInfoDocument  
    Inherits XmlDocument  
        Public Overrides Function CreateElement(prefix As String, localname As String, nsURI As String) As XmlElement  
        Dim elem As New LineInfoElement(prefix, localname, nsURI, Me)  
        Return elem  
    End Function 'CreateElement  
End Class 'LineInfoDocument  
```  
  
```csharp  
class LineInfoDocument : XmlDocument {  
  public override XmlElement CreateElement(string prefix, string localname, string nsURI) {  
  LineInfoElement elem = new LineInfoElement(prefix, localname, nsURI, this);  
  return elem;  
  }  
```  
  
## Расширение класса  
 Чтобы расширить класс, необходимо создать собственный класс на основе одного из существующих классов модели DOM.  После этого можно будет переопределить любые виртуальные методы или свойства в базовом классе или добавить собственные.  
  
 В следующем примере создается новый класс, который реализует класс <xref:System.Xml.XmlElement> и интерфейс <xref:System.Xml.IXmlLineInfo>.  Определяются дополнительные методы и свойства, позволяющие пользователям получать сведения о строке.  
  
```vb  
Class LineInfoElement  
   Inherits XmlElement  
   Implements IXmlLineInfo  
   Private lineNumber As Integer = 0  
   Private linePosition As Integer = 0  
  
   Friend Sub New(prefix As String, localname As String, nsURI As String, doc As XmlDocument)  
      MyBase.New(prefix, localname, nsURI, doc)  
      CType(doc, LineInfoDocument).IncrementElementCount()  
   End Sub 'New  
  
   Public Sub SetLineInfo(linenum As Integer, linepos As Integer)  
      lineNumber = linenum  
      linePosition = linepos  
   End Sub 'SetLineInfo  
  
   Public ReadOnly Property LineNumber() As Integer  
      Get  
         Return lineNumber  
      End Get  
   End Property  
  
   Public ReadOnly Property LinePosition() As Integer  
      Get  
         Return linePosition  
      End Get  
   End Property  
  
   Public Function HasLineInfo() As Boolean  
      Return True  
   End Function 'HasLineInfo  
End Class 'LineInfoElement ' End LineInfoElement class.  
```  
  
```csharp  
class LineInfoElement : XmlElement, IXmlLineInfo {  
   int lineNumber = 0;  
   int linePosition = 0;  
   internal LineInfoElement( string prefix, string localname, string nsURI, XmlDocument doc ) : base( prefix, localname, nsURI, doc ) {  
       ( (LineInfoDocument)doc ).IncrementElementCount();  
  }     
  public void SetLineInfo( int linenum, int linepos ) {  
      lineNumber = linenum;  
      linePosition = linepos;  
  }  
  public int LineNumber {  
     get {  
       return lineNumber;  
     }  
  }  
  public int LinePosition {  
      get {  
        return linePosition;  
      }  
  }  
  public bool HasLineInfo() {   
    return true;   
  }  
} // End LineInfoElement class.  
```  
  
### Пример  
 Следующий пример считает количество элементов в XML\-документе.  
  
```vb  
Imports System  
Imports System.Xml  
Imports System.IO  
  
 _  
  
Class LineInfoDocument  
   Inherits XmlDocument  
  
   Private elementCount As Integer  
  
   Friend Sub New()  
      elementCount = 0  
   End Sub 'New  
  
   Public Overrides Function CreateElement(prefix As String, localname As String, nsURI As String) As XmlElement  
      Dim elem As New LineInfoElement(prefix, localname, nsURI, Me)  
      Return elem  
   End Function 'CreateElement  
  
   Public Sub IncrementElementCount()  
      elementCount += 1  
   End Sub 'IncrementElementCount  
  
   Public Function GetCount() As Integer  
      Return elementCount  
   End Function 'GetCount  
End Class 'LineInfoDocument  
 _ 'End LineInfoDocument class.  
  
Class LineInfoElement  
   Inherits XmlElement  
  
   Friend Sub New(prefix As String, localname As String, nsURI As String, doc As XmlDocument)  
      MyBase.New(prefix, localname, nsURI, doc)  
      CType(doc, LineInfoDocument).IncrementElementCount()  
   End Sub 'New  
End Class 'LineInfoElement  
 _ 'End LineInfoElement class.   
  
Public Class Test  
  
   Private filename As [String] = "book.xml"  
  
   Public Shared Sub Main()  
  
      Dim doc As New LineInfoDocument()  
      doc.Load(filename)  
      Console.WriteLine("Number of elements in {0}: {1}", filename, doc.GetCount())  
   End Sub 'Main   
End Class 'Test  
```  
  
```csharp  
using System;  
using System.Xml;  
using System.IO;  
  
class LineInfoDocument : XmlDocument {  
  
  int elementCount;  
  internal LineInfoDocument():base() {  
    elementCount = 0;  
  }  
  
  public override XmlElement CreateElement( string prefix, string localname, string nsURI) {  
    LineInfoElement elem = new LineInfoElement(prefix, localname, nsURI, this );  
    return elem;  
  }  
  
  public void IncrementElementCount() {  
     elementCount++;  
  }  
  
  public int GetCount() {  
     return elementCount;  
  }  
} // End LineInfoDocument class.  
  
class LineInfoElement:XmlElement {  
  
    internal LineInfoElement( string prefix, string localname, string nsURI, XmlDocument doc ):base( prefix,localname,nsURI, doc ){  
      ((LineInfoDocument)doc).IncrementElementCount();  
    }     
} // End LineInfoElement class.  
  
public class Test {  
  
  const String filename = "book.xml";  
  public static void Main() {  
  
     LineInfoDocument doc =new LineInfoDocument();  
     doc.Load(filename);      
     Console.WriteLine("Number of elements in {0}: {1}", filename, doc.GetCount());  
  
  }  
}   
```  
  
##### Ввод  
 book.xml  
  
```  
<!--sample XML fragment-->  
<book genre='novel' ISBN='1-861001-57-5' misc='sale-item'>  
  <title>The Handmaid's Tale</title>  
  <price>14.95</price>  
</book>  
```  
  
##### Вывод  
  
```  
Number of elements in book.xml: 3  
```  
  
 Пример, показывающий, как расширять классы модели XML DOM \(System.Xml\), см. в www.gotdotnet.com\/userfiles\/XMLDom\/extendDOM.zip.  
  
## Обработчик события узла  
 Реализация модели DOM в платформе .NET Framework также имеет систему событий, которая позволяет принимать и обрабатывать события при изменении узлов в XML\-документе.  При помощи классов <xref:System.Xml.XmlNodeChangedEventHandler> и <xref:System.Xml.XmlNodeChangedEventArgs> можно отслеживать события `NodeChanged`, `NodeChanging`, `NodeInserted`, `NodeInserting`, `NodeRemoved` и `NodeRemoving`.  
  
 В производных классах процесс обработки событий протекает точно так же, как в исходных классах модели DOM.  
  
 Дополнительные сведения об обработке событий узлов см. в разделах [События](../../../../docs/standard/events/index.md) и [Делегат XmlNodeChangedEventHandler](frlrfSystemXmlXmlNodeChangedEventHandlerClassTopic).  
  
## Атрибуты по умолчанию и метод CreateElement  
 При переопределении метода <xref:System.Xml.XmlDocument.CreateElement%2A> в производном классе, если во время изменения документа создаются новые элементы, атрибуты по умолчанию не добавляются.  Это верно только при выполнении изменений.  Так как метод <xref:System.Xml.XmlDocument.CreateElement%2A> отвечает за добавление атрибутов по умолчанию в класс <xref:System.Xml.XmlDocument>, необходимо запрограммировать эту функциональность в методе <xref:System.Xml.XmlDocument.CreateElement%2A>.  При загрузке объекта <xref:System.Xml.XmlDocument>, который содержит атрибуты по умолчанию, они будут обработаны правильно.  Дополнительные сведения об атрибутах по умолчанию см. в разделе [Создание новых атрибутов для элементов в модели DOM](../../../../docs/standard/data/xml/creating-new-attributes-for-elements-in-the-dom.md).  
  
## См. также  
 [Модель DOM для XML](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)