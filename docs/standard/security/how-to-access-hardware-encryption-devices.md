---
title: "How to: Access Hardware Encryption Devices | Microsoft Docs"
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
  - "encryption"
  - "key card"
  - "cryptography"
  - "hardware encryption"
  - "CspParameters"
ms.assetid: b0e734df-6eb4-4b16-b48c-6f0fe82d5f17
caps.latest.revision: 9
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 9
---
# How to: Access Hardware Encryption Devices
Mit der <xref:System.Security.Cryptography.CspParameters>\-Klasse können Sie auf Verschlüsselungsgeräte zuzugreifen.  Beispielsweise können Sie diese Klasse dazu verwenden, Ihre Anwendung auf eine Smartcard, einen hardwaremäßigen Zufallszahlengenerator oder eine Hardwareimplementierung eines bestimmten kryptografischen Algorithmus abzustimmen.  
  
 Die <xref:System.Security.Cryptography.CspParameters>\-Klasse erstellt einen Kryptografiedienstanbieter \(Cryptographic Service Provider, CSP\), der auf ein ordnungsgemäß installiertes Verschlüsselungsgerät zugreift.  Sie können die Verfügbarkeit eines CSP überprüfen, indem Sie sich mit dem Registrierungs\-Editor \(Regedit.exe\) den folgenden Registrierungsschlüssel ansehen: HKEY\_LOCAL\_MACHINE\\Software\\Microsoft\\Cryptography\\Defaults\\Provider.  
  
### So signieren Sie Daten mit einer Schlüsselkarte  
  
1.  Erstellen Sie eine neue Instanz der <xref:System.Security.Cryptography.CspParameters>\-Klasse, wobei Sie den ganzzahligen Anbietertyp sowie den Anbieternamen an den Konstruktor übergeben  
  
2.  Übergeben Sie die entsprechenden Flags an die <xref:System.Security.Cryptography.CspParameters.Flags%2A>\-Eigenschaft des neu erstellten <xref:System.Security.Cryptography.CspParameters>\-Objekts.  Übergeben Sie z. B. das <xref:System.Security.Cryptography.CspProviderFlags>\-Flag.  
  
3.  Erstellen Sie eine neue Instanz einer <xref:System.Security.Cryptography.AsymmetricAlgorithm>\-Klasse \(beispielsweise die <xref:System.Security.Cryptography.RSACryptoServiceProvider>\-Klasse\), wobei Sie das <xref:System.Security.Cryptography.CspParameters>\-Objekt an den Konstruktor übergeben.  
  
4.  Signieren Sie Ihre Daten mit einer der `Sign`\-Methoden, und überprüfen Sie Ihre Daten mit einer der `Verify`\-Methoden.  
  
### So generieren Sie eine Zufallszahl mit einem hardwaremäßigen Zufallszahlengenerator  
  
1.  Erstellen Sie eine neue Instanz der <xref:System.Security.Cryptography.CspParameters>\-Klasse, wobei Sie den ganzzahligen Anbietertyp sowie den Anbieternamen an den Konstruktor übergeben  
  
2.  Erstellen Sie eine neue Instanz der <xref:System.Security.Cryptography.RNGCryptoServiceProvider>\-Klasse, wobei Sie das <xref:System.Security.Cryptography.CspParameters>\-Objekt an den Konstruktor übergeben.  
  
3.  Erstellen Sie mit der <xref:System.Security.Cryptography.RNGCryptoServiceProvider.GetBytes%2A>\-Methode oder der <xref:System.Security.Cryptography.RNGCryptoServiceProvider.GetNonZeroBytes%2A>\-Methode einen zufälligen Wert.  
  
## Beispiel  
 Im folgenden Codebeispiel wird veranschaulicht, wie Daten mit einer Smartcard signiert werden.  Im Codebeispiel wird ein <xref:System.Security.Cryptography.CspParameters>\-Objekt erstellt, das eine Smartcard verfügbar macht, und anschließend wird mit dem CSP ein <xref:System.Security.Cryptography.RSACryptoServiceProvider>\-Objekt initialisiert.  Danach werden im Codebeispiel einige Daten signiert und überprüft.  
  
 [!code-cpp[Cryptography.SmartCardCSP#1](../../../samples/snippets/cpp/VS_Snippets_CLR/Cryptography.SmartCardCSP/CPP/Cryptography.SmartCardCSP.cpp#1)]
 [!code-csharp[Cryptography.SmartCardCSP#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Cryptography.SmartCardCSP/CS/example.cs#1)]
 [!code-vb[Cryptography.SmartCardCSP#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Cryptography.SmartCardCSP/VB/example.vb#1)]  
  
## Kompilieren des Codes  
  
-   Fügen Sie die Namespaces <xref:System> und <xref:System.Security.Cryptography> ein.  
  
-   Sie müssen einen Smartcardleser sowie auf dem Computer installierte Treiber haben.  
  
-   Sie müssen das <xref:System.Security.Cryptography.CspParameters>\-Objekt mit den entsprechenden Informationen über den Kartenleser initialisieren.  Weitere Informationen finden Sie in der Dokumentation zu Ihrem Kartenleser.