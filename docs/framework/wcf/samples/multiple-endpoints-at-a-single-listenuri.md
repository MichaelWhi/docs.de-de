---
title: "Mehrere Endpunkte unter einem ListenUri | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 911ffad4-4d47-4430-b7c2-79192ce6bcbd
caps.latest.revision: 13
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 13
---
# Mehrere Endpunkte unter einem ListenUri
In diesem Beispiel wird ein Dienst gezeigt, der mehrere Endpunkte unter einem einzelnen `ListenUri` hostet.Dieses Beispiel basiert auf dem [Erste Schritte](../../../../docs/framework/wcf/samples/getting-started-sample.md), das einen Rechnerdienst implementiert.  
  
> [!NOTE]
>  Die Setupprozedur und die Erstellungsanweisungen für dieses Beispiel befinden sich am Ende dieses Themas.  
  
 Wie im [Mehrere Endpunkte](../../../../docs/framework/wcf/samples/multiple-endpoints.md)\-Beispiel gezeigt, kann ein Dienst mehrere Endpunkte hosten. Jeder dieser Endpunkte kann unterschiedliche Adressen und möglicherweise auch verschiedene Bindungen haben.In diesem Beispiel wird gezeigt, dass mehrere Endpunkte auch bei der gleichen Adresse gehostet werden können.Außerdem werden in diesem Beispiel die Unterschiede zwischen den beiden Arten der Adressen eines Dienstendpunkts dargestellt: `EndpointAddress` und `ListenUri`.  
  
 Die `EndpointAddress` ist die logische Adresse eines Diensts.An diese Adresse werden SOAP\-Nachrichten adressiert.Der `ListenUri` ist die physische Adresse des Diensts.Er verfügt über die Informationen zu Port und Adresse, an denen der Dienst auf dem aktuellen Computer tatsächlich Nachrichten überwacht.In den meisten Fällen müssen diese Adressen nicht unterschiedlich sein. Wenn ein `ListenUri` nicht explizit angegeben ist, wird er standardmäßig auf den URI der `EndpointAddress` des Endpunkts festgelegt.In einigen Fällen ist es jedoch nützlich, die Adressen zu unterscheiden, z. B. wenn Sie einen Router konfigurieren, der Nachrichten akzeptiert, die möglicherweise an eine Reihe verschiedener Dienste adressiert sind.  
  
## Dienst  
 Der Dienst in diesem Beispiel besitzt zwei Verträge: `ICalculator` und `IEcho`.Zusätzlich zum üblichen `IMetadataExchange`\-Endpunkt gibt es drei Anwendungsendpunkte, wie im folgenden Code gezeigt.  
  
```  
<endpoint address="urn:Stuff"  
        binding="wsHttpBinding"  
        contract="Microsoft.ServiceModel.Samples.ICalculator"   
        listenUri="http://localhost/servicemodelsamples/service.svc" />  
<endpoint address="urn:Stuff"  
        binding="wsHttpBinding"  
        contract="Microsoft.ServiceModel.Samples.IEcho"   
        listenUri="http://localhost/servicemodelsamples/service.svc" />  
<endpoint address="urn:OtherEcho"  
        binding="wsHttpBinding"  
        contract="Microsoft.ServiceModel.Samples.IEcho"   
        listenUri="http://localhost/servicemodelsamples/service.svc" />  
```  
  
 Alle drei Endpunkte werden unter demselben `ListenUri` gehostet und verwenden dieselbe `binding`. Endpunkte unter demselben `ListenUri` müssen dieselbe Bindung besitzen, da sie gemeinsam einen einzigen Kanalstapel nutzen, der Nachrichten an dieser physischen Adresse des Computers überwacht.Die `address` jedes Endpunkts ist ein URN. Obwohl Adressen in der Regel physische Speicherplätze darstellen, kann die Adresse eigentlich ein beliebiger URI sein, da die Adresse für den Abgleich und die Filterung verwendet wird, wie in diesem Beispiel gezeigt.  
  
 Da alle drei Endpunkte denselben `ListenUri` nutzen, muss [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] beim Eintreffen einer Nachricht an diesem URI entscheiden, für welchen Endpunkt die Nachricht bestimmt ist.Jeder Endpunkt besitzt einen Nachrichtenfilter, der aus zwei Teilen besteht: dem Adressfilter und dem Vertragsfilter.Der Adressfilter gleicht `To` der SOAP\-Nachricht mit der Adresse des Dienstendpunkts ab.So sind beispielsweise nur an `To "Urn:OtherEcho"` adressierte Nachrichten für den dritten Endpunkt dieses Diensts vorgesehen.Der Vertragsfilter gleicht die den Vorgängen eines besonderen Vertrags zugeordneten Aktionen ab.Nachrichten mit der Aktion von `IEcho``Echo` stimmen beispielsweise mit den Vertragsfiltern des zweiten und dritten Endpunkts dieses Diensts überein, da beide Endpunkte den `IEcho`\-Vertrag hosten.  
  
 Deshalb ermöglicht es die Kombination aus Adressfilter und Vertragsfilter, jede am `ListenUri` dieses Diensts eingehende Nachricht zum richtigen Endpunkt weiterzuleiten.Der dritte Endpunkt wird von den anderen beiden unterschieden, da er Nachrichten akzeptiert, die von den anderen Endpunkten an eine andere Adresse gesendet wurden.Der erste und der zweite Endpunkt werden anhand ihrer Verträge \(die Aktion der eingehenden Nachricht\) voneinander unterschieden.  
  
## Client  
 So wie Endpunkte auf dem Server zwei verschiedene Adressen besitzen, weisen Clientendpunkte auch zwei Adressen auf.Sowohl auf dem Server als auch auf dem Client lautet die logische Adresse `EndpointAddress`.Die physische Adresse wird jedoch auf dem Server `ListenUri` und auf dem Client `Via` genannt.  
  
 Wie auf dem Server sind diese beiden Adressen standardmäßig identisch.Um einen `Via` auf dem Client anzugeben, der sich von der Adresse des Endpunkts unterscheidet, wird `ClientViaBehavior` verwendet:  
  
```  
Uri via = new Uri("http://localhost/ServiceModelSamples/service.svc");  
CalculatorClient calcClient = new CalculatorClient();  
calcClient.ChannelFactory.Endpoint.Behaviors.Add(  
        new ClientViaBehavior(via));  
```  
  
 Wie üblich kommt die Adresse aus der Clientkonfigurationsdatei, die von "Svcutil.exe" generiert wurde.`Via` \(entspricht `ListenUri` des Diensts\) wird nicht in den Metadaten des Diensts angezeigt, weshalb diese Information dem Client out\-of\-band mitgeteilt werden muss \(genauso wie die Metadatenadresse des Diensts\).  
  
 In diesem Beispiel sendet der Client Nachrichten an jeden der drei Endpunkte des Servers, um zu zeigen, dass er mit allen drei Endpunkten kommunizieren \(und sie unterscheiden\) kann, selbst dann, wenn sie alle denselben `Via` besitzen.  
  
#### So richten Sie das Beispiel ein, erstellen es und führen es aus  
  
1.  Vergewissern Sie sich, dass Sie die [Einmaliges Setupverfahren für Windows Communication Foundation\-Beispiele](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md) ausgeführt haben.  
  
2.  Folgen Sie zum Erstellen der C\#\- bzw. Visual Basic .NET\-Version der Projektmappe den Anweisungen unter [Erstellen der Windows Communication Foundation\-Beispiele](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3.  Wenn Sie das Beispiel in einer Konfiguration mit einem Computer oder computerübergreifend ausführen möchten, folgen Sie den unter [Durchführen der Windows Communication Foundation\-Beispiele](../../../../docs/framework/wcf/samples/running-the-samples.md) aufgeführten Anweisungen.  
  
    > [!NOTE]
    >  Bei einer computerübergreifenden Konfiguration müssen Sie "localhost" in der Datei "Client.cs" durch den Namen des Dienstcomputers ersetzen.  
  
> [!IMPORTANT]
>  Die Beispiele sind möglicherweise bereits auf dem Computer installiert.Überprüfen Sie das folgende \(standardmäßige\) Verzeichnis, bevor Sie fortfahren.  
>   
>  `<Installationslaufwerk>:\WF_WCF_Samples`  
>   
>  Wenn dieses Verzeichnis nicht vorhanden ist, rufen Sie [Windows Communication Foundation \(WCF\) and Windows Workflow Foundation \(WF\) Samples for .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) auf, um alle [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)]\- und [!INCLUDE[wf1](../../../../includes/wf1-md.md)]\-Beispiele herunterzuladen.Dieses Beispiel befindet sich im folgenden Verzeichnis.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\MultipleEndpointsSingleUri`  
  
## Siehe auch