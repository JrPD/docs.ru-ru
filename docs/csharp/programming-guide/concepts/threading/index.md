---
title: "Работа с потоками (C#)"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: 236d157d-37c0-4ee8-89fc-721e6c596325
caps.latest.revision: 4
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 45ffa38254717c9aee29c3922bf801f6a90c716e
ms.contentlocale: ru-ru
ms.lasthandoff: 07/28/2017

---
# <a name="threading-c"></a>Работа с потоками (C#)
Потоки позволяют программе на C# осуществлять параллельную обработку, то есть вы можете выполнять несколько операций одновременно. Например, потоки можно использовать для отслеживания пользовательского ввода, выполнения фоновых задач и одновременной обработки нескольких потоков ввода.  
  
 Потоки имеют следующие свойства.  
  
-   Потоки позволяют программе выполнять параллельную обработку.  
  
-   Пространство имен <xref:System.Threading> платформы .NET Framework упрощает использование потоков.  
  
-   Потоки используют одни и те же ресурсы приложения. Дополнительные сведения см. в разделе [Использование потоков и работа с потоками](https://msdn.microsoft.com/library/e1dx6b2h).  
  
 По умолчанию программа на языке C# имеет один поток. Но можно также создавать вспомогательные потоки и выполнять в них код параллельно основному. Эти потоки часто называются *рабочими потоками*.  
  
 Рабочие потоки можно использовать для выполнения трудоемких или срочных задач без прерывания основного потока. Рабочие потоки часто используются, например, в серверных приложениях, когда необходимо обработать входящий запрос, не дожидаясь завершения обработки предыдущего запроса. Кроме того, они используются для выполнения «фоновых» задач в настольных приложениях, что позволяет не занимать ресурсы основного потока, ответственного за работу пользовательского интерфейса.  
  
 Применение потоков позволяет решить проблемы с пропускной способностью и быстротой работы системы, но оно также может привести к проблемам с совместным использованием ресурсов, например к конфликтам и взаимоблокировкам. Использование нескольких потоков лучше всего подходит в тех случаях, когда разные задачи используют разные ресурсы, например дескрипторы файлов и сетевые подключения. Назначение нескольких потоков одному ресурсу, вероятнее всего, вызовет проблемы синхронизации и частую блокировку из-за ожидания, что сделает само использование нескольких потоков нецелесообразным.  
  
 Обычно рабочие потоки используются для выполнения трудоемких или срочных задач, которым не требуется большое количество ресурсов, занятых другими потоками. Естественно, некоторые используемые программой ресурсы должны быть доступны нескольким потокам. Для таких случаев в пространстве имен <xref:System.Threading> доступны классы по синхронизации потоков. К таким классам относятся <xref:System.Threading.Mutex>, <xref:System.Threading.Monitor>, <xref:System.Threading.Interlocked>, <xref:System.Threading.AutoResetEvent> и <xref:System.Threading.ManualResetEvent>.  
  
 Все классы или некоторые из них можно использовать для синхронизации работы нескольких потоков, но некоторые функции для потоков поддерживаются также и языком C#. Например [оператор lock](../../../../csharp/language-reference/keywords/lock-statement.md) дает возможность синхронизации за счет неявного использования класса <xref:System.Threading.Monitor>.  
  
> [!NOTE]
>  Начиная с [!INCLUDE[net_v40_long](~/includes/net-v40-long-md.md)], многопоточное программирование значительно упростилось благодаря классам <xref:System.Threading.Tasks.Parallel?displayProperty=fullName> и <xref:System.Threading.Tasks.Task?displayProperty=fullName>, [Parallel LINQ (PLINQ)](https://msdn.microsoft.com/library/dd460688), новым классам параллельной коллекции из пространства имен <xref:System.Collections.Concurrent?displayProperty=fullName> и новой модели программирования, которая вместо потоков использует концепцию задач. Дополнительные сведения см. в разделе [Параллельное программирование](https://msdn.microsoft.com/library/dd460693).  
  
## <a name="related-topics"></a>См. также  
  
|Заголовок|Описание|  
|-----------|-----------------|  
|[Многопоточные приложения(C#)](../../../../csharp/programming-guide/concepts/threading/multithreaded-applications.md)|Описание создания и использования потоков.|  
|[Параметры и возвращаемые значения для многопоточных процедур (C#)](../../../../csharp/programming-guide/concepts/threading/parameters-and-return-values-for-multithreaded-procedures.md)|Описание передачи и возвращения параметров в многопоточных приложениях.|  
|[Пошаговое руководство. Многопоточность с помощью компонента BackgroundWorker (C#)](../../../../csharp/programming-guide/concepts/threading/walkthrough-multithreading-with-the-backgroundworker-component.md)|Пример создания простого многопоточного приложения.|  
|[Синхронизация потоков (C#)](../../../../csharp/programming-guide/concepts/threading/thread-synchronization.md)|Описание управления взаимодействием потоков.|  
|[Таймеры потоков (C#)](../../../../csharp/programming-guide/concepts/threading/thread-timers.md)|Описание запуска процедур в отдельных потоках через фиксированные промежутки времени.|  
|[Группировка потоков в пул (C#)](../../../../csharp/programming-guide/concepts/threading/thread-pooling.md)|Описание использования пула рабочих потоков, управляемых системой.|  
|[Практическое руководство. Использование пула потоков (C#)](../../../../csharp/programming-guide/concepts/threading/how-to-use-a-thread-pool.md)|Демонстрация синхронизированного использования нескольких потоков в пуле.|  
|[Работа с потоками](https://msdn.microsoft.com/library/3e8s7xdd)|Описание реализации работы с потоками на платформе .NET Framework.|

