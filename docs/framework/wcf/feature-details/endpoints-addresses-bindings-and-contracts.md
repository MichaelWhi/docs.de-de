---
title: "Endpunkte: Adressen, Bindungen und Vertr&#228;ge | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "Endpunkte [WCF]"
  - "WCF [WCF], Endpunkte"
  - "Windows Communication Foundation [WCF], Endpunkte"
ms.assetid: 9ddc46ee-1883-4291-9926-28848c57e858
caps.latest.revision: 14
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 14
---
# Endpunkte: Adressen, Bindungen und Vertr&#228;ge
Die gesamte Kommunikation mit einem [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)]\-Dienst verläuft über die *Endpunkte* des Diensts.Endpunkte ermöglichen Clients den Zugriff auf die Funktionalität, die ein [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Dienst anbietet.  
  
 Jeder Endpunkt verfügt über vier Eigenschaften:  
  
-   Eine Adresse, die angibt, wo der Endpunkt zu finden ist.  
  
-   Eine Bindung, die angibt, wie ein Client mit dem Endpunkt kommunizieren kann.  
  
-   Ein Vertrag, der die verfügbaren Vorgänge identifiziert.  
  
-   Eine Gruppe von Verhaltensweisen, die lokale Implementierungsdetails des Endpunkts angeben.  
  
 In diesem Thema wird diese Endpunktstruktur erläutert, und es wird erklärt, wie sie im [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Objektmodell dargestellt wird.  
  
## Die Struktur eines Endpunkts  
 Jeder Endpunkt besteht aus Folgendem:  
  
-   Adresse: Die Adresse gewährleistet eine eindeutige Identifizierung des Endpunkts und teilt potenziellen Consumern des Diensts den Standort des Diensts mit.Sie wird im [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Objektmodell durch die <xref:System.ServiceModel.EndpointAddress>\-Klasse dargestellt.Eine <xref:System.ServiceModel.EndpointAddress>\-Klasse enthält:  
  
    -   Eine <xref:System.ServiceModel.EndpointAddress.Uri%2A>\-Eigenschaft, die die Adresse des Diensts darstellt.  
  
    -   Eine <xref:System.ServiceModel.EndpointAddress.Identity%2A>\-Eigenschaft, die die Sicherheits\-ID des Diensts und eine Auflistung optionaler Nachrichtenheader darstellt.Die optionalen Nachrichtenheader werden verwendet, um zusätzliche und ausführlichere Adressinformationen bereitzustellen, um den Endpunkt zu identifizieren oder damit zu interagieren.  
  
     [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Angeben einer Endpunktadresse](../../../../docs/framework/wcf/specifying-an-endpoint-address.md).  
  
-   Bindung: Die Bindung gibt an, wie eine Kommunikation mit dem Endpunkt stattfindet.Dies umfasst Folgendes:  
  
    -   Das Transportprotokoll, das verwendet werden soll \(z. B. TCP oder HTTP\).  
  
    -   Die Codierung, die für die Nachrichten verwendet werden soll \(z. B. Text oder binär\).  
  
    -   Die erforderlichen Sicherheitsanforderungen \(z. B. SSL\- oder SOAP\-Nachrichtensicherheit\).  
  
     [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [WCF\-Bindungsübersicht](../../../../docs/framework/wcf/bindings-overview.md).Eine Bindung wird im [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Objektmodell durch die abstrakte Basisklasse <xref:System.ServiceModel.Channels.Binding> dargestellt.Für die meisten Szenarien können Benutzer eine der vom System bereitgestellten Bindungen verwenden.Weitere Informationen finden Sie unter [Vom System bereitgestellte Bindungen](../../../../docs/framework/wcf/system-provided-bindings.md).  
  
-   Verträge: Der Dienstvertrag zeigt, welche Funktionen der Endpunkt dem Client zur Verfügung stellt.Ein Vertrag gibt Folgendes an:  
  
    -   Welche Vorgänge von einem Client aufgerufen werden können.  
  
    -   Die Form der Nachricht.  
  
    -   Der Typ der Eingabeparameter oder Daten, die zum Aufrufen des Vorgangs erforderlich sind.  
  
    -   Den Typ der Verarbeitung oder der Antwortnachricht, den der Client erwarten kann.  
  
     Weitere Informationen zum Definieren eines Vertrags finden Sie unter [Entwerfen von Dienstverträgen](../../../../docs/framework/wcf/designing-service-contracts.md).\)  
  
-   Verhaltensweisen: Sie können das lokale Verhalten des Dienstendpunkts mithilfe von Endpunktverhaltensweisen anpassen.Endpunktverhaltensweisen erreichen dies, indem sie an der Erstellung einer [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]\-Laufzeit teilnehmen.Ein Beispiel für ein Endpunktverhalten ist die <xref:System.ServiceModel.Description.ServiceEndpoint.ListenUri%2A>\-Eigenschaft, mit der Sie eine andere Listeningadresse als die SOAP\- oder die WSDL\-Adresse \(WSDL \= Web Services Description Language\) angeben können.[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][ClientViaBehavior](../../../../docs/framework/wcf/diagnostics/wmi/clientviabehavior.md).  
  
## Definieren von Endpunkten  
 Sie können den Endpunkt für einen Dienst entweder verbindlich durch Verwenden von Code oder deklarativ durch Konfiguration angeben.[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][Gewusst wie: Erstellen eines Dienstendpunkts in einer Konfiguration](../../../../docs/framework/wcf/feature-details/how-to-create-a-service-endpoint-in-configuration.md) und [Gewusst wie: Erstellen eines Dienstendpunkts im Code](../../../../docs/framework/wcf/feature-details/how-to-create-a-service-endpoint-in-code.md).  
  
## In diesem Abschnitt  
 In diesem Abschnitt wird der Zweck von Bindungen, Endpunkten und Adressen erläutert. Darüber hinaus wird gezeigt, wie Sie eine Bindung und einen Endpunkt konfigurieren und wie Sie das `ClientVia`\-Verhalten und die `ListenUri`\-Eigenschaft verwenden.  
  
 [Adressen](../../../../docs/framework/wcf/feature-details/endpoint-addresses.md)  
 Beschreibt, wie Endpunkte in [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] behandelt werden.  
  
 [Bindungen](../../../../docs/framework/wcf/feature-details/bindings.md)  
 Beschreibt, wie mit Bindungen Transport, Codierung und Protokolldetails angegeben werden, die für die Kommunikation zwischen Clients und Diensten erforderlich sind.  
  
 [Verträge](../../../../docs/framework/wcf/feature-details/contracts.md)  
 Beschreibt, wie Verträge die Methoden eines Diensts definieren.  
  
 [Gewusst wie: Erstellen eines Dienstendpunkts in einer Konfiguration](../../../../docs/framework/wcf/feature-details/how-to-create-a-service-endpoint-in-configuration.md)  
 Beschreibt, wie Sie einen Dienstendpunkt in einer Konfiguration erstellen.  
  
 [Gewusst wie: Erstellen eines Dienstendpunkts im Code](../../../../docs/framework/wcf/feature-details/how-to-create-a-service-endpoint-in-code.md)  
 Beschreibt, wie Sie einen Dienstendpunkt im Code erstellen.  
  
 [Vorgehensweise: Verwenden von "Svcutil.exe" zum Überprüfen von kompiliertem Dienstcode](../../../../docs/framework/wcf/feature-details/how-to-use-svcutil-exe-to-validate-compiled-service-code.md)  
 Beschreibt, wie Sie mit dem [ServiceModel Metadata Utility\-Tool \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) Fehler in Dienstimplementierungen und Konfigurationen erkennen, ohne den Dienst zu hosten.  
  
## Siehe auch  
 [Konfigurieren von Diensten](../../../../docs/framework/wcf/configuring-services.md)   
 [Erweitern von Bindungen](../../../../docs/framework/wcf/extending/extending-bindings.md)