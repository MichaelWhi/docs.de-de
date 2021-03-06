---
title: "Vorgehensweise: Angeben einer Clientbindung im Code | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 6bee5da4-adf7-42e6-8f78-63a9e5c6dbad
caps.latest.revision: 9
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 9
---
# Vorgehensweise: Angeben einer Clientbindung im Code
In diesem Beispiel wird ein Client erstellt, um einen Rechnerdienst zu nutzen, und die Bindung des Clients wird imperativ im Code angegeben.Der Client greift auf den `CalculatorService` zu, der die `ICalculator`\-Schnittstelle implementiert, und sowohl der Dienst als auch der Client verwenden die Klasse <xref:System.ServiceModel.BasicHttpBinding>.  
  
 In dieser Prozedur wird davon ausgegangen, dass der Rechnerdienst ausgeführt wird.Informationen über das Erstellen des Diensts finden Sie unter [Vorgehensweise: Angeben einer Dienstbindung in einer Konfiguration](../../../docs/framework/wcf/how-to-specify-a-service-binding-in-configuration.md).Außerdem wird das [ServiceModel Metadata Utility\-Tool \(Svcutil.exe\)](../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) von [!INCLUDE[indigo1](../../../includes/indigo1-md.md)] verwendet, um die Clientkomponenten automatisch zu generieren.Das Tool generiert den Clientcode für den Zugriff auf den Dienst.  
  
 Der Client wird in zwei Schritten erstellt.Svcutil.exe generiert den `ClientCalculator`, der die `ICalculator`\-Schnittstelle implementiert.Anschließend wird die Clientanwendung erstellt, indem eine Instanz von `ClientCalculator` erstellt wird und dann die Bindung und die Adresse für den Dienst im Code angegeben werden.  
  
 Die Quellkopie dieses Beispiels finden Sie im Beispiel [BasicBinding](../../../docs/framework/wcf/samples/basicbinding.md).  
  
### So geben Sie eine benutzerdefinierte Bindung im Code an  
  
1.  Verwenden Sie Svcutil.exe in der Befehlszeile, um Code von Dienstmetadaten zu generieren.  
  
    ```  
    Svcutil.exe <service's Metadata Exchange (MEX) address or HTTP GET address>   
    ```  
  
2.  Der generierte Client enthält die `ICalculator`\-Schnittstelle, die den Dienstvertrag definiert, dem die Clientimplementierung entsprechen muss.  
  
     [!code-csharp[C_HowTo_CodeClientBinding#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_codeclientbinding/cs/client.cs#1)]
     [!code-vb[C_HowTo_CodeClientBinding#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_codeclientbinding/vb/client.vb#1)]  
  
3.  Der generierte Client enthält außerdem die Implementierung vom `ClientCalculator`.  
  
     [!code-csharp[C_HowTo_CodeClientBinding#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_codeclientbinding/cs/client.cs#2)]
     [!code-vb[C_HowTo_CodeClientBinding#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_codeclientbinding/vb/client.vb#2)]  
  
4.  Erstellen Sie eine Instanz des `ClientCalculator`, der die <xref:System.ServiceModel.BasicHttpBinding>\-Klasse in einer Clientanwendung verwendet, und rufen Sie anschließend die Dienstvorgänge an der angegebenen Adresse auf.  
  
     [!code-csharp[C_HowTo_CodeClientBinding#3](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_codeclientbinding/cs/client.cs#3)]
     [!code-vb[C_HowTo_CodeClientBinding#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_codeclientbinding/vb/client.vb#3)]  
  
5.  Kompilieren Sie den Code, und führen Sie den Client aus.  
  
## Siehe auch  
 [Verwenden von Bindungen, um Dienste und Clients zu konfigurieren](../../../docs/framework/wcf/using-bindings-to-configure-services-and-clients.md)