---
title: "&lt;security&gt; von &lt;msmqIntegrationBinding&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: ae5c68a8-14a2-4c6e-b9e0-3e94e3e9135e
caps.latest.revision: 13
author: "BrucePerlerMS"
ms.author: "bruceper"
manager: "mbaldwin"
caps.handback.revision: 13
---
# &lt;security&gt; von &lt;msmqIntegrationBinding&gt;
Definiert die Transportsicherheitseinstellungen für den Message Queuing \(MSMQ\)\-Integrationskanal.  
  
## Syntax  
  
```  
  
<msmqIntegrationBinding>  
   <binding>   
       <security mode="None/Transport">  
         <transport msmqAuthenticationMode="None/Windows/Certificate"  
            msmqEncryptionAlgorithm="RC4Stream/AES"  
            msmqProtectionLevel="None/Sign/EncryptAndSign"  
            msmqSecureHashAlgorithm="MD5/SHA1/SHA256/SHA512" />  
          <message  algorithmSuite="Aes128/Aes192/Aes256/Rsa15Aes128/ Rsa15Aes256/TripleDes"  
                        clientCredentialType="None/Windows/UserName/Certificate/CardSpace"  
            defaultProtectionLevel="None/Sign/EncryptAndSign" />  
       </security>  
   </binding>  
</msmqIntegrationBinding>   
```  
  
## Attribute und Elemente  
 In den folgenden Abschnitten werden Attribute sowie untergeordnete und übergeordnete Elemente beschrieben.  
  
### Attribute  
  
|Attribut|Beschreibung|  
|--------------|------------------|  
|Modus|Gibt den Sicherheitstyp an, der Integrität, Vertraulichkeit und Authentifizierung mit dem Message Queuing\-Integrationskanal steuert.  Folgende Werte sind gültig:<br /><br /> -   None: Die Sicherheit wird deaktiviert.<br />-   Transport: Schutz und Authentifizierung werden vom Transport bereitgestellt.  Dies bezieht sich auf die Nachrichtensicherheit zwischen beiden Warteschlangen\-Managern.  Es besteht keine Sicherheit zwischen der Anwendung und dem Warteschlangen\-Manager.  Vorhandene Msmq\-Anwendungen sind mit diesem Typ des Sicherheitsmodus funktional äquivalent.<br /><br /> Der Standardwert ist `Transport`.  Dieses Attribut ist vom Typ <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationSecurityMode>.|  
  
### Untergeordnete Elemente  
  
|Element|Beschreibung|  
|-------------|------------------|  
|[\<transport\>](../../../../../docs/framework/configure-apps/file-schema/wcf/transport-of-msmqintegrationbinding.md)|Definiert die Sicherheitseinstellungen für Message Queuing\-Integration und \-Transport.  Dieses Element ist vom Typ <xref:System.ServiceModel.Configuration.MsmqTransportSecurityElement>.|  
  
### Übergeordnete Elemente  
  
|Element|Beschreibung|  
|-------------|------------------|  
|[\<Bindung\>](../../../../../docs/framework/misc/binding.md)|Das Bindungselement von [\<msmqIntegrationBinding\>](../../../../../docs/framework/configure-apps/file-schema/wcf/msmqintegrationbinding.md).|  
  
## Siehe auch  
 <xref:System.ServiceModel.Configuration.MsmqIntegrationSecurityElement>   
 <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding.Security%2A>   
 <xref:System.ServiceModel.Configuration.MsmqIntegrationBindingElement.Security%2A>   
 <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationSecurity>   
 [Warteschlangen in WCF](../../../../../docs/framework/wcf/feature-details/queues-in-wcf.md)   
 [Sichern von Diensten und Clients](../../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)   
 [Bindungen](../../../../../docs/framework/wcf/bindings.md)   
 [Konfigurieren der vom System bereitgestellten Bindungen](../../../../../docs/framework/wcf/feature-details/configuring-system-provided-bindings.md)   
 [Using Bindings to Configure Windows Communication Foundation Services and Clients](http://msdn.microsoft.com/de-de/bd8b277b-932f-472f-a42a-b02bb5257dfb)   
 [\<Bindung\>](../../../../../docs/framework/misc/binding.md)   
 [\<msmqIntegrationBinding\>](../../../../../docs/framework/configure-apps/file-schema/wcf/msmqintegrationbinding.md)