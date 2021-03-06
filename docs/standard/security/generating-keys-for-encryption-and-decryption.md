---
title: "Generating Keys for Encryption and Decryption | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "keys, encryption and decryption"
  - "keys, asymmetric"
  - "keys, symmetric"
  - "encryption [.NET Framework], keys"
  - "symmetric keys"
  - "asymmetric keys [.NET Framework]"
  - "cryptography [.NET Framework], keys"
ms.assetid: c197dfc9-a453-4226-898d-37a16638056e
caps.latest.revision: 14
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 14
---
# Generating Keys for Encryption and Decryption
Das Erstellen und Verwalten von Schlüsseln ist ein wichtiger Bestandteil des kryptografischen Prozesses. Bei symmetrischen Algorithmen müssen ein Schlüssel und ein Initialisierungsvektor \(IV\) erstellt werden. Der Schlüssel muss vor Unbefugten, die Ihre Daten nicht entschlüsseln können sollen, geheim gehalten werden. Der IV muss nicht geheim sein, sollte aber für jede Sitzung geändert werden. Bei asymmetrischen Algorithmen müssen ein öffentlicher und ein privater Schlüssel erstellt werden. Der öffentliche Schlüssel kann allgemein zugänglich sein, während der private Schlüssel nur dem Teilnehmer bekannt sein darf, der die mit dem öffentlichen Schlüssel verschlüsselten Daten entschlüsselt. In diesem Abschnitt wird beschrieben, wie Schlüssel für symmetrische und asymmetrische Algorithmen erzeugt und verwaltet werden.  
  
## Symmetrische Schlüssel  
 Für die von .NET Framework bereitgestellten symmetrischen Verschlüsselungsklassen sind ein Schlüssel und ein neuer Initialisierungsvektor \(IV\) erforderlich, damit Daten verschlüsselt und entschlüsselt werden können. Wenn Sie mit dem Standardkonstruktor eine neue Instanz einer verwalteten symmetrischen Kryptografieklasse erstellen, werden automatisch ein neuer Schlüssel und ein neuer IV erzeugt. Alle Benutzer, die zum Entschlüsseln der Daten berechtigt sind, müssen über den gleichen Schlüssel und IV verfügen und den gleichen Algorithmus verwenden. Generell sollten für jede Sitzung ein neuer Schlüssel und IV erstellt und weder Schlüssel noch IV für eine spätere Sitzung gespeichert werden.  
  
 Um den symmetrischen Schlüssel und IV der Gegenseite mitzuteilen, wird der symmetrische Schlüssel in der Regel asymmetrisch verschlüsselt. Das Senden des unverschlüsselten Schlüssels über ein nicht sicheres Netzwerk birgt ein großes Sicherheitsrisiko, da jeder, der den Schlüssel und den IV abfängt, die Daten entschlüsseln kann. Weitere Informationen zum verschlüsselten Datenaustausch finden Sie unter [Erstellen eines kryptografischen Schemas](../../../docs/standard/security/creating-a-cryptographic-scheme.md).  
  
 Im folgenden Beispiel wird eine neue Instanz der <xref:System.Security.Cryptography.TripleDESCryptoServiceProvider>\-Klasse erstellt, durch die der TripleDES\-Algorithmus implementiert wird.  
  
```vb  
Dim TDES As TripleDESCryptoServiceProvider = new TripleDESCryptoServiceProvider()  
  
```  
  
```csharp  
TripleDESCryptoServiceProvider TDES = new TripleDESCryptoServiceProvider();  
```  
  
 Beim Ausführen des obigen Codes werden ein neuer Schlüssel und IV erzeugt und in die **Key**\-Eigenschaft bzw. die **IV**\-Eigenschaft aufgenommen.  
  
 Manchmal müssen u. U. mehrere Schlüssel erzeugt werden. In diesem Fall können Sie zuerst eine neue Instanz einer Klasse, durch die ein symmetrischer Algorithmus implementiert wird, erstellen und erzeugen dann durch den Aufruf der **GenerateKey**\-Methode und der **GenerateIV**\-Methode einen neuen Schlüssel und IV. Das folgende Codebeispiel veranschaulicht, wie nach dem Erstellen einer neuen Instanz der asymmetrischen kryptografischen Klasse neue Schlüssel und IVs erzeugt werden.  
  
```vb  
Dim TDES As TripleDESCryptoServiceProvider = new TripleDESCryptoServiceProvider() TDES.GenerateIV() TDES.GenerateKey()  
  
```  
  
```csharp  
TripleDESCryptoServiceProvider TDES = new TripleDESCryptoServiceProvider(); TDES.GenerateIV(); TDES.GenerateKey();  
```  
  
 Beim Ausführen des obigen Codes werden beim Erstellen der neuen **TripleDESCryptoServiceProvider**\-Instanz ein Schlüssel und IV erzeugt. Ein weiterer Schlüssel und ein weiterer IV werden beim Aufruf der **GenerateKey**\-Methode und der **GenerateIV**\-Methode erstellt.  
  
## Asymmetrische Schlüssel  
 .NET Framework stellt die Klassen <xref:System.Security.Cryptography.RSACryptoServiceProvider> und <xref:System.Security.Cryptography.DSACryptoServiceProvider> für asymmetrische Verschlüsselung bereit. Durch diese Klassen wird beim Erstellen einer neuen Instanz mit dem Standardkonstruktor ein öffentliches\/privates Schlüsselpaar erzeugt. Asymmetrische Schlüssel können entweder nur für eine Sitzung erzeugt oder gespeichert und für mehrere Sitzungen verwendet werden. Während der öffentliche Schlüssel öffentlich verfügbar sein kann, sollte der private Schlüssel streng geheim gehalten werden.  
  
 Bei jedem Erstellen einer neuen Instanz einer asymmetrischen Algorithmusklasse wird ein öffentliches\/privates Schlüsselpaar erzeugt. Nachdem eine neue Instanz der Klasse erstellt worden ist, können die Schlüsselinformationen mit einer der folgenden beiden Methoden extrahiert werden:  
  
-   mit der <xref:System.Security.Cryptography.RSA.ToXmlString%2A>\-Methode, die eine XML\-Darstellung der Schlüsselinformationen zurückgibt;  
  
-   mit der <xref:System.Security.Cryptography.RSACryptoServiceProvider.ExportParameters%2A>\-Methode, die eine <xref:System.Security.Cryptography.RSAParameters>\-Struktur mit den Schlüsselinformationen zurückgibt.  
  
 Bei beiden Methoden wird ein boolescher Wert akzeptiert, der angibt, ob nur die Informationen des öffentlichen Schlüssels oder die Informationen beider Schlüssel \(öffentlich und privat\) zurückgegeben werden sollen. Eine **RSACryptoServiceProvider**\-Klasse kann mit dem Wert einer **RSAParameters**\-Struktur initialisiert werden, indem die <xref:System.Security.Cryptography.RSACryptoServiceProvider.ImportParameters%2A>\-Methode verwendet wird.  
  
 Asymmetrische private Schlüssel sollten in keinem Fall in vollem Wortlaut oder in Klartext auf dem lokalen Computer gespeichert werden. Wenn ein privater Schlüssel gespeichert werden muss, sollten Sie einen Schlüsselcontainer verwenden. Weitere Informationen zum Speichern eines privaten Schlüssels in einem Schlüsselcontainer finden Sie unter [How to: Store Asymmetric Keys in a Key Container](../../../docs/standard/security/how-to-store-asymmetric-keys-in-a-key-container.md).  
  
 Im folgenden Codebeispiel wird eine neue Instanz der **RSACryptoServiceProvider**\-Klasse erstellt, wodurch ein öffentliches\/privates Schlüsselpaar erzeugt wird. Dann werden die Informationen des öffentlichen Schlüssels in einer **RSAParameters\-**Struktur gespeichert.  
  
```vb  
'Generate a public/private key pair. Dim RSA as RSACryptoServiceProvider = new RSACryptoServiceProvider() 'Save the public key information to an RSAParameters structure. Dim RSAKeyInfo As RSAParameters = RSA.ExportParameters(false)  
  
```  
  
```csharp  
//Generate a public/private key pair. RSACryptoServiceProvider RSA = new RSACryptoServiceProvider(); //Save the public key information to an RSAParameters structure. RSAParameters RSAKeyInfo = RSA.ExportParameters(false);  
```  
  
## Siehe auch  
 [Encrypting Data](../../../docs/standard/security/encrypting-data.md)   
 [Decrypting Data](../../../docs/standard/security/decrypting-data.md)   
 [Kryptografische Dienste](../../../docs/standard/security/cryptographic-services.md)   
 [How to: Store Asymmetric Keys in a Key Container](../../../docs/standard/security/how-to-store-asymmetric-keys-in-a-key-container.md)