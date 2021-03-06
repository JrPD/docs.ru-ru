---
title: "Элемент &lt;issuerChannelBehaviors&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: f7378673-8e9b-45b2-98d1-cf5dccdd8c40
caps.latest.revision: 8
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 8
---
# Элемент &lt;issuerChannelBehaviors&gt;
Содержит коллекцию поведений конечной точки клиента [!INCLUDE[indigo1](../../../../../includes/indigo1-md.md)] \(определенных в конфигурации\) для использования при взаимодействии с заданными службами маркеров безопасности.  К определенным поведениям не относятся элементы [\<clientCredentials\>](../../../../../docs/framework/configure-apps/file-schema/wcf/clientcredentials.md).  
  
## Синтаксис  
  
```  
  
<issuerChannelBehaviors>  
      <add behaviorConfiguraton="string"  
                issuerAddress="string" />  
</issuerChannelBehaviors>  
```  
  
## Атрибуты и элементы  
 В следующих разделах описаны атрибуты, дочерние и родительские элементы.  
  
### Атрибуты  
 Отсутствует.  
  
### Дочерние элементы  
  
|Элемент|Описание|  
|-------------|--------------|  
|[\<добавление;\>](../../../../../docs/framework/configure-apps/file-schema/wcf/add-of-issuerchannelbehaviors.md)|Добавляет поведение в коллекцию.|  
  
### Родительские элементы  
  
|Элемент|Описание|  
|-------------|--------------|  
|[\<issuedToken\>](../../../../../docs/framework/configure-apps/file-schema/wcf/issuedtoken.md)|Задает пользовательский маркер, используемый для проверки подлинности клиента при подключении к службе.|  
  
## Заметки  
 Этот элемент используется, когда для связи со службой необходимо любое поведение \(кроме поведений, в которые включаются элементы `<clientCredentials>`\).  Например, если необходимо включение элемента поведения [\<dataContractSerializer\>](../../../../../docs/framework/configure-apps/file-schema/wcf/datacontractserializer-element.md).  
  
## См. также  
 <xref:System.ServiceModel.Configuration.IssuedTokenClientElement.IssuerChannelBehaviors%2A>   
 <xref:System.ServiceModel.Configuration.IssuedTokenClientBehaviorsElement>   
 <xref:System.ServiceModel.Configuration.IssuedTokenClientBehaviorsElementCollection>   
 <xref:System.ServiceModel.Security.IssuedTokenClientCredential.IssuerChannelBehaviors%2A>   
 [Идентификация и проверка подлинности службы](../../../../../docs/framework/wcf/feature-details/service-identity-and-authentication.md)   
 [Поведения безопасности](../../../../../docs/framework/wcf/feature-details/security-behaviors-in-wcf.md)   
 [Федерация и выданные маркеры](../../../../../docs/framework/wcf/feature-details/federation-and-issued-tokens.md)   
 [Защита служб и клиентов](../../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)   
 [Обеспечение безопасности клиентов](../../../../../docs/framework/wcf/securing-clients.md)   
 [Как создавать федеративный клиент](../../../../../docs/framework/wcf/feature-details/how-to-create-a-federated-client.md)   
 [Как настраивать локальный издатель](../../../../../docs/framework/wcf/feature-details/how-to-configure-a-local-issuer.md)   
 [Федерация и выданные маркеры](../../../../../docs/framework/wcf/feature-details/federation-and-issued-tokens.md)