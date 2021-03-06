---
title: "System.ServiceModel.Channels.HttpsClientCertificateNotPresent | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: b13ef1b6-e340-401d-93ca-2710c3842205
caps.latest.revision: 5
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 5
---
# System.ServiceModel.Channels.HttpsClientCertificateNotPresent
Ein Clientzertifikat ist erforderlich.Es wurde kein Zertifikat in der Anforderung gefunden.  
  
## Beschreibung  
 Diese Ablaufverfolgung gibt an, dass der HTTP\-Listener eine HTTPS\-Anforderung empfangen hat, die keinem Clientzertifikat zugeordnet war.Da der Listener so konfiguriert wurde, dass für alle HTTPS\-Anforderungen Clientzertifikate erforderlich sind, konnte der Listener die Authentizität des Clients nicht überprüfen.  
  
## Siehe auch  
 [Ablaufverfolgung](../../../../../docs/framework/wcf/diagnostics/tracing/index.md)   
 [Verwenden der Ablaufverfolgung zum Beheben von Anwendungsfehlern](../../../../../docs/framework/wcf/diagnostics/tracing/using-tracing-to-troubleshoot-your-application.md)   
 [Verwaltung und Diagnose](../../../../../docs/framework/wcf/diagnostics/index.md)