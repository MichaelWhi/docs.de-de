---
title: "Dienstidentit&#228;tsbeispiel | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 79fa8c1c-85bb-4b67-bc67-bfaf721303f8
caps.latest.revision: 24
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 24
---
# Dienstidentit&#228;tsbeispiel
Dieses Dienstidentitätsbeispiel veranschaulicht das Festlegen der Identität eines Diensts.Während der Entwurfszeit kann ein Client die Identität mithilfe der Metadaten des Diensts abrufen und zur Laufzeit die Identität des Diensts authentifizieren.Das Konzept der Dienstidentität besteht darin, dass ein Client einen Dienst authentifizieren kann, bevor er einen seiner Vorgänge aufruft. Auf diese Weise wird der Client vor nicht authentifizierten Aufrufen geschützt.Bei einer sicheren Verbindung authentifiziert der Dienst auch die Anmeldeinformationen eines Clients, bevor er diesem Zugriff gewährt. Diese Funktion steht jedoch nicht im Mittelpunkt dieses Beispiels.Beispiele zur Serverauthentifizierung finden Sie unter [Client](../../../../docs/framework/wcf/samples/client.md).  
  
> [!NOTE]
>  Die Setupprozedur und die Buildanweisungen für dieses Beispiel befinden sich am Ende dieses Themas.  
  
 In diesem Beispiel werden die folgenden Funktionen veranschaulicht:  
  
-   Wie die verschiedenen Identitätstypen auf unterschiedlichen Endpunkten zu einem Dienst festgelegt werden.Jeder Identitätstyp hat andere Funktionen.Der zu verwendende Identitätstyp ist vom Typ der Sicherheitsanmeldeinformationen abhängig, die bei der Bindung des Endpunkts verwendet werden.  
  
-   Die Identität kann entweder deklarativ in der Konfiguration oder imperativ im Code festgelegt werden.In der Regel sollten Sie sowohl für den Client als auch für den Dienst die Konfiguration verwenden, um die Identität festzulegen.  
  
-   Vorgehensweise: Festlegen einer benutzerdefinierten Identität für den ClientEine benutzerdefinierte Identität ist normalerweise eine Anpassung eines vorhandenen Identitätstyps, mit dem ein Client andere in den Dienstanmeldeinformationen enthaltene Anspruchsinformationen überprüfen kann. Auf diese Weise können Autorisierungsentscheidungen getroffen werden, bevor der Dienst aufgerufen wird.  
  
    > [!NOTE]
    >  Dieses Beispiel überprüft die Identität eines Zertifikats mit der Bezeichnung "identity.com" und den RSA\-Schlüssel in diesem Zertifikat.Wenn Sie die Identitätstypen "Zertifikat" und "RSA" in der Konfiguration des Clients verwenden, können diese Werte problemlos durch das Überprüfen des WSDL für den Dienst abgerufen werden, auf dem diese Werte serialisiert sind.  
  
 Der folgende Beispielcode zeigt, wie Sie mit "WSHttpBinding" die Identität eines Dienstendpunkts mit dem DNS \(Domain Name Server\) eines Zertifikats konfigurieren.  
  
```  
//Create a service endpoint and set its identity to the certificate's DNS                 
WSHttpBinding wsAnonbinding = new WSHttpBinding (SecurityMode.Message);  
// Client are Anonymous to the service  
wsAnonbinding.Security.Message.ClientCredentialType = MessageCredentialType.None;  
WServiceEndpoint ep = serviceHost.AddServiceEndpoint(typeof(ICalculator),wsAnonbinding, String.Empty);  
EndpointAddress epa = new EndpointAddress(dnsrelativeAddress,EndpointIdentity.CreateDnsIdentity("identity.com"));  
ep.Address = epa;  
  
```  
  
 Die Identität kann auch in der Konfiguration der Datei "App.config" angegeben werden.Im folgenden Beispiel wird gezeigt, wie die UPN\-Identität \(User Principal Name\) für einen Dienstendpunkt festgelegt wird.  
  
```  
<endpoint address="upnidentity"  
        behaviorConfiguration=""  
        binding="wsHttpBinding"  
        bindingConfiguration="WSHttpBinding_Windows"  
        name="WSHttpBinding_ICalculator_Windows"  
        contract="Microsoft.ServiceModel.Samples.ICalculator">  
  <!-- Set the UPN identity for this endpoint -->  
  <identity>  
      <userPrincipalName value="host\myservice.com" />  
  </identity >  
</endpoint>  
  
```  
  
 Eine benutzerdefinierte Identität kann durch Ableiten von den <xref:System.ServiceModel.EndpointIdentity>\- und <xref:System.ServiceModel.Security.IdentityVerifier>\-Klassen auf dem Client eingerichtet werden.Im Prinzip kann die <xref:System.ServiceModel.Security.IdentityVerifier>\-Klasse als Cliententsprechung der `AuthorizationManager`\-Klasse des Diensts betrachtet werden.Im folgenden Codebeispiel wird eine Implementierung von `OrgEndpointIdentity` veranschaulicht. Hier wird der Name einer Organisation gespeichert, der mit dem Antragstellernamen des Serverzertifikats verglichen wird.Die Autorisierungsprüfung des Organisationsnamens findet in der `CheckAccess`\-Methode der `CustomIdentityVerifier`\-Klasse statt.  
  
```  
  
// This custom EndpointIdentity stores an organization name   
public class OrgEndpointIdentity : EndpointIdentity  
{  
    private string orgClaim;  
    public OrgEndpointIdentity(string orgName)  
    {  
        orgClaim = orgName;  
    }  
    public string OrganizationClaim  
    {  
        get { return orgClaim; }  
        set { orgClaim = value; }  
    }  
}  
  
//This custom IdentityVerifier uses the supplied OrgEndpointIdentity to  
//check that X.509 certificate's distinguished name claim contains  
//the organization name e.g. the string value "O=Contoso"   
class CustomIdentityVerifier : IdentityVerifier  
{  
    public override bool CheckAccess(EndpointIdentity identity, AuthorizationContext authContext)  
    {  
        bool returnvalue = false;  
        foreach (ClaimSet claimset in authContext.ClaimSets)  
        {  
            foreach (Claim claim in claimset)  
            {  
                if (claim.ClaimType == "http://schemas.microsoft.com/ws/2005/05/identity/claims/x500distinguishedname")  
                {  
                    X500DistinguishedName name = (X500DistinguishedName)claim.Resource;  
                    if (name.Name.Contains(((OrgEndpointIdentity)identity).OrganizationClaim))  
                    {  
                        Console.WriteLine("Claim Type: {0}",claim.ClaimType);  
                        Console.WriteLine("Right: {0}", claim.Right);  
                        Console.WriteLine("Resource: {0}",claim.Resource);  
                        Console.WriteLine();  
                        returnvalue = true;  
                    }  
                }  
            }  
        }  
        return returnvalue;  
    }  
}  
```  
  
 In diesem Beispiel wird ein Zertifikat namens "identity.com" verwendet, das sich im sprachspezifischen Identitätsprojektmappenordner befindet.  
  
### So richten Sie das Beispiel ein, erstellen es und führen es aus  
  
1.  Stellen Sie sicher, dass Sie die [Einmaliges Setupverfahren für Windows Communication Foundation\-Beispiele](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md) ausgeführt haben.  
  
2.  Folgen Sie zum Erstellen der C\#\- bzw. Visual Basic .NET\-Version der Projektmappe den Anweisungen unter [Erstellen der Windows Communication Foundation\-Beispiele](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3.  Um das Beispiel in einer Konfiguration mit einem Computer oder computerübergreifend auszuführen, folgen Sie den Anweisungen unter [Durchführen der Windows Communication Foundation\-Beispiele](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
### So führen Sie das Beispiel auf demselben Computer aus  
  
1.  Importieren Sie unter [!INCLUDE[wxp](../../../../includes/wxp-md.md)] oder [!INCLUDE[wv](../../../../includes/wv-md.md)] die Zertifikatdatei Identity.pfx im Identitätsprojektmappenordner mithilfe des MMC\-Snap\-In\-Tools in den Zertifikatspeicher LocalMachine\/Mein \(persönlicher\) Zertifikatspeicher.Diese Datei ist durch ein Kennwort geschützt.Während des Imports werden Sie um ein Kennwort gebeten.Geben Sie in das Kennwortfeld `xyz` ein.Weitere Informationen finden Sie im Thema [Vorgehensweise: Anzeigen von Zertifikaten mit dem MMC\-Snap\-In](../../../../docs/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in.md).Führen Sie anschließend Setup.bat an einer Visual Studio\-Eingabeaufforderung mit Administratorrechten aus, um dieses Zertifikat zur Verwendung auf dem Client in den Speicher CurrentUser\/TrustedPeople zu kopieren.  
  
2.  Führen Sie unter [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] Setup.bat aus dem Beispielinstallationsordner an einer [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)]\-Eingabeaufforderung mit Administratorrechten aus.Hiermit werden alle Zertifikate installiert, die zum Ausführen des Beispiels erforderlich sind.  
  
    > [!NOTE]
    >  Die Batchdatei Setup.bat ist darauf ausgelegt, an einer [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)]\-Eingabeaufforderung ausgeführt zu werden.Die innerhalb der [!INCLUDE[vs_current_long](../../../../includes/vs-current-long-md.md)]\-Eingabeaufforderung festgelegte PATH\-Umgebungsvariable zeigt auf das Verzeichnis mit den ausführbaren Dateien, die für das Skript Setup.bat erforderlich sind.Wenn Sie mit dem Beispiel fertig sind, müssen Sie die Datei Cleanup.bat ausführen, um die Zertifikate zu entfernen.In anderen Sicherheitsbeispielen werden die gleichen Zertifikate verwendet.  
  
3.  Starten Sie "Service.exe" aus dem Verzeichnis "\\service\\bin".Stellen Sie sicher, dass der Dienst seine Bereitschaft und eine Aufforderung zum Drücken der \<EINGABETASTE\> anzeigt, um den Dienst zu beenden.  
  
4.  Starten Sie "Client.exe" aus dem Verzeichnis "\\client\\bin" oder durch das Drücken der Taste F5 in Visual Studio zum Erstellen und Ausführen.In der Clientkonsolenanwendung wird Clientaktivität angezeigt.  
  
5.  Wenn der Client und der Dienst nicht miteinander kommunizieren können, finden Sie weitere Informationen unter [Troubleshooting Tips](http://msdn.microsoft.com/de-de/8787c877-5e96-42da-8214-fa737a38f10b).  
  
### So führen Sie das Beispiel computerübergreifend aus  
  
1.  Bevor Sie die Clientseite des Beispiels erstellen, müssen Sie den Wert der Dienstendpunktadresse in der Datei Client.cs in der `CallServiceCustomClientIdentity`\-Methode ändern.Erstellen Sie anschließend das Beispiel.  
  
2.  Erstellen Sie auf dem Dienstcomputer ein Verzeichnis.  
  
3.  Kopieren Sie die Dienstprogrammdateien aus service\\bin in das Verzeichnis auf dem Dienstcomputer.Kopieren Sie außerdem die Dateien Setup.bat und Cleanup.bat auf den Dienstcomputer.  
  
4.  Erstellen Sie auf dem Clientcomputer ein Verzeichnis für die Clientbinärdateien.  
  
5.  Kopieren Sie die Clientprogrammdateien in das Clientverzeichnis auf dem Clientcomputer.Kopieren Sie die Dateien Setup.bat, Cleanup.bat und ImportServiceCert.bat ebenfalls auf den Client.  
  
6.  Führen Sie auf dem Dienstcomputer `setup.bat service` an einer Visual Studio\-Eingabeaufforderung mit Administratorrechten aus.Durch Ausführen von `setup.bat` mit dem Argument `service` wird ein Dienstzertifikat mit dem vollqualifizierten Domänennamen des Computers erstellt und in die Datei Service.cer exportiert.  
  
7.  Kopieren Sie die Datei Service.cer aus dem Dienstverzeichnis in das Clientverzeichnis auf dem Clientcomputer.  
  
8.  Ändern Sie in der Datei Client.exe.config auf dem Clientcomputer den Wert für die Adresse des Endpunkts, sodass er mit der neuen Adresse Ihres Diensts übereinstimmt.Es gibt mehrere Instanzen, die geändert werden müssen.  
  
9. Führen Sie auf dem Client ImportServiceCert.bat an einer Visual Studio\-Eingabeaufforderung mit Administratorrechten aus.Dadurch wird das Dienstzertifikat aus der Datei Service.cer in den Speicher CurrentUser – TrustedPeople importiert.  
  
10. Starten Sie auf dem Dienstcomputer Service.exe an einer Eingabeaufforderung.  
  
11. Starten Sie auf dem Clientcomputer Client.exe an einer Eingabeaufforderung.Wenn der Client und der Dienst nicht miteinander kommunizieren können, finden Sie weitere Informationen unter [Troubleshooting Tips](http://msdn.microsoft.com/de-de/8787c877-5e96-42da-8214-fa737a38f10b).  
  
### So stellen Sie den Zustand vor Ausführung des Beispiels wieder her  
  
-   Führen Sie Cleanup.bat im Beispielordner aus, nachdem Sie das Beispiel fertig ausgeführt haben.  
  
    > [!NOTE]
    >  Wenn dieses Beispiel computerübergreifend ausgeführt wird, entfernt dieses Skript keine Dienstzertifikate auf einem Client.Wenn Sie [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)]\-Beispiele ausgeführt haben, die Zertifikate computerübergreifend verwenden, müssen Sie die Dienstzertifikate entfernen, die im Speicher CurrentUser – TrustedPeople installiert wurden.Verwenden Sie dazu den folgenden Befehl: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` Beispiel: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`.  
  
## Siehe auch