---
title: "Verwenden von Variablen mit einem .NET&#160;Framework&#160;3.5-Ruleset | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 27b56249-22fe-4252-840f-74c0d6c7a6b3
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 7
---
# Verwenden von Variablen mit einem .NET&#160;Framework&#160;3.5-Ruleset
Dieses Beispiel veranschaulicht, wie ein Workflow erstellt wird, der mithilfe der <xref:System.Activities.Statements.Interop>\-Aktivität eine in [!INCLUDE[netfx35_short](../../../../includes/netfx35-short-md.md)] geschriebene benutzerdefinierte Aktivität integriert, die eine Richtlinie und Regeln verwendet.Der Workflow übergibt Daten an die benutzerdefinierte Aktivität, indem er Variablen an die von der benutzerdefinierten Aktivität verfügbar gemachten Abhängigkeitseigenschaften bindet.  
  
## Exemplarische Vorgehensweise  
  
#### So überprüfen Sie TravelRuleLibrary  
  
1.  Öffnen Sie in [!INCLUDE[vs_current_short](../../../../includes/vs-current-short-md.md)] die Projektmappendatei "InteropWith35RuleSet.sln".  
  
2.  Öffnen Sie "TravelRuleSet.cs" im Workflow\-Designer.  
  
     Es wird eine benutzerdefinierte sequenzielle Aktivität angezeigt, die eine <xref:System.Workflow.Activities.PolicyActivity> enthält.  
  
3.  Doppelklicken Sie auf die DiscountPolicy\-Richtlinienaktivität, um die Regeln zu überprüfen.  
  
     Die Regeln werden im Regel\-Editor angezeigt.  
  
4.  Klicken Sie mit der rechten Maustaste auf `DiscountPolicy`, und wählen Sie die Option **Code anzeigen**, um den Code\-Beside\-C\#\-Code für die Aktivität zu überprüfen.  
  
     Achten Sie auf die Einstellung der Abhängigkeitseigenschaft für `DiscountLevel`.Diese entspricht den Argumenten in [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)].[!INCLUDE[crabout](../../../../includes/crabout-md.md)] zu Argumente finden Sie unter [Variablen und Argumente](../../../../docs/framework/windows-workflow-foundation//variables-and-arguments.md).  
  
## InteropWith35RuleSet  
 Dies ist ein sequenzielles Workflowprojekt, das die <xref:System.Activities.Statements.Interop>\-Aktivität für die Integration mit dem im `TravelRuleLibrary`\-Projekt erstellten benutzerdefinierten Regelsatz verwendet.Variablen werden in der <xref:System.Activities.Statements.Sequence>\-Aktivität auf oberster Ebene erstellt.Die <xref:System.Activities.Statements.Interop>\-Aktivität wird zur Integration mit der `TravelRuleSet`\-Aktivität verwendet.Die Variablen in der <xref:System.Activities.Statements.Sequence> deklarierten Variablen werden zum Binden an die Abhängigkeitseigenschaften verwendet.  
  
## So verwenden Sie dieses Beispiel  
  
1.  Öffnen Sie in [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)] die Projektmappendatei "InteropWith35RuleSet.sln".  
  
2.  Drücken Sie STRG\+UMSCHALT\+B, um die Projektmappe zu erstellen.  
  
3.  Drücken Sie STRG\+F5, um die Projektmappe auszuführen.  
  
> [!IMPORTANT]
>  Die Beispiele sind möglicherweise bereits auf dem Computer installiert.Suchen Sie nach dem folgenden Verzeichnis \(Standardverzeichnis\), bevor Sie fortfahren.  
>   
>  `<Installationslaufwerk>:\WF_WCF_Samples`  
>   
>  Wenn dieses Verzeichnis nicht vorhanden ist, rufen Sie [Windows Communication Foundation \(WCF\) and Windows Workflow Foundation \(WF\) Samples for .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) auf, um alle [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)]\- und [!INCLUDE[wf1](../../../../includes/wf1-md.md)]\-Beispiele herunterzuladen.Dieses Beispiel befindet sich im folgenden Verzeichnis.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Built-InActivities\InteropWith35RuleSet`  
  
## Siehe auch