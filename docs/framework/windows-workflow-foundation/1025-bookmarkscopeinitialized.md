---
title: "1025 - BookmarkScopeInitialized | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 63584434-e709-471d-9e96-97d3d99e70d6
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# 1025 - BookmarkScopeInitialized
## Eigenschaften  
  
|||  
|-|-|  
|ID|1025|  
|Schlüsselwörter|WFRuntime|  
|Ebene|Ausführlich|  
|Kanal|Microsoft\-Windows\-Application Server\-Applications\/Debug|  
  
## Beschreibung  
 Gibt an, dass ein BookmarkScope initialisiert wurde.  
  
## Meldung  
 BookmarkScope mit der TemporaryId: '%1' wurde mit der ID: '%2' initialisiert.  
  
## Details  
  
|Datenelementname|Datenelementtyp|Beschreibung|  
|----------------------|---------------------|------------------|  
|TemporaryId|xs:string|Die temporäre ID des Lesezeichens.|  
|InitializedId|xs:string|Die Initialisierungs\-ID des Lesezeichens.|  
|AppDomain|xs:string|Die von AppDomain.CurrentDomain.FriendlyName zurückgegebene Zeichenfolge.|