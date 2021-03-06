---
title: "xml:lang Handling in XAML | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-wpf"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "XAML [XAML Services], xml:lang attribute"
  - "xml:lang attribute [XAML Services]"
  - "RFC 3066 standard [XAML Services]"
  - "standards [XAML Services], RFC 3066"
ms.assetid: 7aac0078-a1c5-41f8-b8b0-975510d9dca0
caps.latest.revision: 16
author: "wadepickett"
ms.author: "wpickett"
manager: "wpickett"
caps.handback.revision: 15
---
# xml:lang Handling in XAML
Атрибут `xml:lang` — это определенный [!INCLUDE[TLA2#tla_xml](../../../includes/tla2sharptla-xml-md.md)] атрибут, который объявляет язык и региональные параметры для элемента в XML. Такое же значение атрибута сохраняется в XAML; однако действуют некоторые дополнительные факторы.  
  
## Использование атрибута XAML  
  
```  
<object xml:lang="rfc3066lang" />  
```  
  
## Значения XAML  
  
|||  
|-|-|  
|*rfc3066lang*|Строка, которая является производной от стандарта [RFC 3066](http://go.microsoft.com/fwlink/?LinkId=132454) и определяет язык или язык\-регион. В последнем варианте язык и регион разделяются дефисом. Дополнительные сведения о значениях и формате см. в разделе <xref:System.Windows.Markup.XmlLanguage>.|  
  
## Заметки  
 Определение атрибута `xml:lang` в [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] является производным от `xml:lang`, определенного в [!INCLUDE[TLA#tla_w3c](../../../includes/tlasharptla-w3c-md.md)] как "специальный атрибут" для [!INCLUDE[TLA2#tla_xml](../../../includes/tla2sharptla-xml-md.md)]. Сведения о языке и региональных параметрах потенциально обрабатываются элементами по\-разному, в зависимости от реализации этих элементов; однако обработка [!INCLUDE[TLA2#tla_xaml](../../../includes/tla2sharptla-xaml-md.md)] атрибута `xml:lang` по умолчанию отсутствует.  
  
 Значение по умолчанию атрибута `xml:lang` представляет собой пустую строку на уровне атрибута.  
  
 Действие атрибута `xml:lang` и значение этого атрибута обычно сохраняются в дочерних элементах при интерпретации системами, которые работают со значениями `xml:lang`.  
  
 При интерпретации модулем записи XAML служб XAML .NET Framework значение `xml:lang` может создать объекты <xref:System.Windows.Markup.XmlLanguage> или <xref:System.Globalization.CultureInfo> в базовом объектном представлении; однако это поведение зависит от того, является ли значение, указанное для `xml:lang`, допустимой конструкцией для этих классов.  
  
 Инфраструктуры могут создавать связи между определенными инфраструктурой свойствами и значением `xml:lang` в XML, применяя <xref:System.Windows.Markup.XmlLangPropertyAttribute> к свойству.  
  
## Узлы использования WPF  
 Для элементов, которые являются производными классами от <xref:System.Windows.FrameworkElement> или <xref:System.Windows.FrameworkContentElement>, можно использовать эквивалентное свойство зависимостей <xref:System.Windows.FrameworkElement.Language%2A> вместо атрибута `xml:lang`. По умолчанию свойство <xref:System.Windows.FrameworkElement.Language%2A> использует "en\-US", если не установлено иное, посредством этого свойства или путем обработки атрибута `xml:lang`.  
  
## См. также  
 [Глобализация для WPF](../../../ocs/framework/wpf/advanced/globalization-for-wpf.md)