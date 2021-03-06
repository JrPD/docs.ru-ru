---
title: "Сборки и глобальный кэш сборок (Visual Basic) | Документация Майкрософт"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
ms.assetid: fcf78ff1-f1ab-4a5d-b6d8-00d2046b6c80
caps.latest.revision: 3
author: dotnet-bot
ms.author: dotnetcontent
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 0b712132becfe47d50d1c06c0e8fd9940b8035e9
ms.lasthandoff: 03/13/2017

---
# <a name="assemblies-and-the-global-assembly-cache-visual-basic"></a>Сборки и глобальный кэш сборок (Visual Basic)
Сборки представляют собой базовый элемент развертывания, управления версиями, повторного использования, назначения областей активации и прав доступа для приложения на основе платформы .NET. Сборки создаются в форме исполняемого файла (.exe) или файла динамической библиотеки (.dll) и являются составными частями .NET Framework. Они предоставляют сведения для среды CLR, которые нужны для распознавания реализаций типов. Сборку можно представить как коллекцию типов и ресурсов, которые предназначены для совместной работы и формируют логическую единицу функциональности.  
  
 Сборки могут содержать один или несколько модулей. Например, в крупных проектах несколько разработчиков могут создавать отдельные модули, которые вместе образуют единую сборку. Дополнительные сведения о модулях см. в статье [How to: Build a Multifile Assembly](https://msdn.microsoft.com/library/226t7yxe) (Практическое руководство. Создание многофайловой сборки).  
  
 Сборки имеют следующие свойства.  
  
-   Сборки реализованы как файлы .exe или .dll.  
  
-   Сборку можно совместно использовать в нескольких приложениях, поместив ее в глобальный кэш сборок. Сборки должны получить строгие имена, чтобы их можно было включить в глобальный кэш сборок. Подробнее см. в статье [Сборки со строгими именами](https://msdn.microsoft.com/library/wd40t7ad).  
  
-   Сборки загружаются в память только в том случае, если они реально используются. Если сборка не используется, она не загружается. Благодаря этому свойству сборки могут быть эффективным средством для управления ресурсами в крупных проектах.  
  
-   Сведения о сборке можно получить программным путем с помощью отражения. Дополнительные сведения см. в статье [Отражение (Visual Basic)](../../../../visual-basic/programming-guide/concepts/reflection.md).  
  
-   Если вы хотите загрузить сборку только для проверки, используйте метод <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A>.  
  
## <a name="assembly-manifest"></a>Манифест сборки  
 Каждая сборка содержит *манифест сборки*. Манифест сборки выполняет роль оглавления и содержит следующую информацию.  
  
-   Идентификатор сборки (ее имя и версию).  
  
-   Таблицу всех файлов, входящих в сборку. Сюда будут включаться другие ваши сборки, от которых зависит файл .exe или .dll, а также растровые изображения или файлы Readme.  
  
-   *Список ссылок сборки*, то есть список всех внешних зависимостей — библиотек или других файлов, созданных другими разработчиками, которые требуются для работы вашего приложения. Ссылки на сборки содержат ссылки на глобальные и частные объекты. Глобальные объекты находятся в глобальном кэше сборок. Эта область, как системный каталог System32, доступна для всех других приложений. Пространство имен <xref:Microsoft.VisualBasic?displayProperty=fullName> можно привести в качестве примера сборки, размещенной в глобальном кэше сборок. Закрытые объекты должны находиться в каталоге установки приложения или в одном из его подкаталогов.  
  
 Поскольку сборки содержат сведения о содержимом, управлении версиями и зависимостях, правильность работы созданных вами приложений на Visual Basic не зависит от значений в реестре Windows. Сборки снижают риск конфликта библиотек DLL, а также повышают надежность и простоту развертывания приложений. Во многих случаях для установки приложения на базе .NET достаточно просто скопировать его файлы на целевой компьютер.  
  
 Подробнее см. в статье [Манифест сборки](https://msdn.microsoft.com/library/1w45z383).  
  
## <a name="adding-a-reference-to-an-assembly"></a>Добавление ссылки на сборку  
 Чтобы использовать сборку, нужно добавить ссылку на нее. После этого с помощью [инструкции Imports](../../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md) вы выбираете пространство имен элементов, которые хотите использовать. Когда вы добавите ссылку на сборку и импортируете ее, в вашем приложении станут доступны все предоставленные в сборке классы, свойства, методы и другие члены пространства имен, как если бы их код являлся частью файла с исходным кодом вашего приложения.  
  
## <a name="creating-an-assembly"></a>Создание сборки  
 Скомпилируйте приложение, запустите сборку из командной строки с использованием компилятора командной строки. Дополнительные сведения о сборке сборок из командной строки см. в статье [Building from the Command Line](../../../../visual-basic/reference/command-line-compiler/building-from-the-command-line.md) (Сборка из командной строки).  
  
> [!NOTE]
>  Чтобы создать сборку в Visual Studio, выберите пункт **Сборка** из меню **Сборка**.  
  
## <a name="see-also"></a>См. также  
 [Сборки в среде CLR](https://msdn.microsoft.com/library/k3677y81)   
 [Friend Assemblies (Visual Basic)](friend-assemblies.md)  (Дружественные сборки (Visual Basic))  
 [How to: Share an Assembly with Other Applications (Visual Basic)](how-to-share-an-assembly-with-other-applications.md)  (Практическое руководство. Совместное использование сборки с другими приложениями (Visual Basic))  
 [How to: Load and Unload Assemblies (Visual Basic)](how-to-load-and-unload-assemblies.md)  (Практическое руководство. Загрузка и выгрузка сборок (Visual Basic))  
 [How to: Determine If a File Is an Assembly (Visual Basic)](how-to-determine-if-a-file-is-an-assembly.md)  (Практическое руководство. Как определить, является ли файл сборкой (Visual Basic))  
 [How to: Create and Use Assemblies Using the Command Line (Visual Basic)](how-to-create-and-use-assemblies-using-the-command-line.md)  (Практическое руководство. Создание и использование сборок с помощью командной строки (Visual Basic))  
 [Walkthrough: Embedding Types from Managed Assemblies in Visual Studio (Visual Basic)](walkthrough-embedding-types-from-managed-assemblies-in-vs.md)  (Пошаговое руководство. Внедрение типов из управляемых сборок в Visual Studio (Visual Basic))  
 [Walkthrough: Embedding Type Information from Microsoft Office Assemblies in Visual Studio (Visual Basic)](walkthrough-embedding-type-information-from-microsoft-office-assemblies-in-vs.md) (Пошаговое руководство. Внедрение данных о типах из сборок Microsoft Office в Visual Studio (Visual Basic))
