---
title: "Konfigurieren von Apps mithilfe von Konfigurationsdateien | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "AutoGeneratedOrientationPage"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - ".NET Framework-Anwendungskonfiguration, Konfigurationsdateien"
  - "<runtime>-Element"
  - "Anwendungskonfigurationsdateien"
  - "Anwendungskonfigurationsdateien, Info"
  - "Anwendungskonfigurationsdateien, format"
  - "Anwendungskonfigurationsdateien, Computer"
  - "Anwendungskonfigurationsdateien, Sicherheit"
  - "Anwendungen [.NET Framework], Konfigurationsdateien"
  - "ASP.NET, Konfigurieren"
  - "Konfigurationsdateien [.NET Framework]"
  - "Konfigurationsdateien [.NET Framework], Anwendung"
  - "Konfigurationsdateien [.NET Framework], format"
  - "Konfigurationsdateien [.NET Framework], Computer"
  - "Konfigurationsdateien [.NET Framework], Sicherheit"
  - "Endtags"
  - "Hosts, Anwendung"
  - "Computerkonfigurationsdateien"
  - "Sicherheitskonfigurationsdateien"
  - "Starttags"
ms.assetid: 86bd26d3-737e-4484-9782-19b17f34cd1f
caps.latest.revision: 28
author: "mcleblanc"
ms.author: "markl"
manager: "markl"
caps.handback.revision: 28
---
# Konfigurieren von Apps mithilfe von Konfigurationsdateien
.NET Framework bietet Entwicklern und Administratoren die Möglichkeit, die Ausführung von Anwendungen über Konfigurationsdateien flexibel zu steuern.  Konfigurationsdateien sind XML\-Dateien, die je nach Bedarf verändert werden können.  So kann der Administrator bestimmen, auf welche geschützten Ressourcen eine Anwendung zugreifen kann, welche Assemblyversionen sie verwenden soll und wo sich Remoteanwendungen befinden.  Entwickler wiederum können Einstellungen in Konfigurationsdateien einfügen, sodass eine Anwendung nicht jedes Mal neu kompiliert werden muss, wenn sich ihre Einstellungen ändern.  In diesem Abschnitt wird beschrieben, was konfiguriert werden kann und warum die Konfiguration einer Anwendung von Nutzen sein kann.  
  
> [!NOTE]
>  Verwalteter Code kann mithilfe der Klassen im <xref:System.Configuration>\-Namespace Einstellungen aus den Konfigurationsdateien lesen, jedoch keine Einstellungen in diese Dateien schreiben.  
  
 Dieses Thema beschreibt die Syntax von Konfigurationsdateien und enthält Informationen zu den drei Arten von Konfigurationsdateien: Computer, Anwendung und Sicherheit.  
  
## Format von Konfigurationsdateien  
 Konfigurationsdateien enthalten Elemente, so genannte logische Datenstrukturen zur Einstellung von Konfigurationsinformationen.  Innerhalb der Konfigurationsdatei können Sie Anfang und Ende eines Elements durch Tags markieren.  Zum Beispiel besteht das Element `<runtime>` aus `<runtime>` *untergeordneten* `</runtime>`\-Elementen.  Ein leeres Element würde in Form von `<runtime/>` oder `<runtime>``</runtime>` geschrieben.  
  
 Wie bei allen XML\-Dateien wird bei der Syntax von Konfigurationsdateien die Groß\-\/Kleinschreibung berücksichtigt.  
  
 Sie können Konfigurationseinstellungen mithilfe von vordefinierten Attributen vornehmen. Diese Attribute sind Name\/Wert\-Paare innerhalb des Anfangstags eines Elements.  Durch den folgenden Beispielcode werden zwei Attribute \(`version` und `href`\) für das `<codeBase>`\-Element angegeben, das den Ort bezeichnet, an dem die Common Language Runtime eine Assembly auffinden kann \(weitere Informationen hierzu finden Sie unter [Festlegen des Speicherortes einer Assembly](../../../docs/framework/configure-apps/specify-assembly-location.md)\).  
  
```  
<codeBase version="2.0.0.0"  
          href="http://www.litwareinc.com/myAssembly.dll"/>  
```  
  
## Computerkonfigurationsdateien  
 Die Computerkonfigurationsdatei Machine.config enthält Einstellungen, die für den gesamten Computer gelten.  Diese Datei befindet sich im Verzeichnis "%*runtime install path*%\\Config".  Machine.config enthält Konfigurationseinstellungen für die computerweite Assemblybindung, für integrierte [Remotingkanäle](http://msdn.microsoft.com/de-de/6e9b60e0-9bc0-47b4-a8ef-3b78585f9a18) und für ASP.NET.  
  
 Das Konfigurationssystem durchsucht zuerst die Computerkonfigurationsdatei nach dem [appSettings\-Element](http://msdn.microsoft.com/de-de/0d65a3f1-c522-423d-89b6-44921b6daebb) und anderen Konfigurationsabschnitten, die von einem Entwickler möglicherweise definiert wurden.  Anschließend wird die Anwendungskonfigurationsdatei durchsucht.  Aus Gründen der Übersichtlichkeit der Computerkonfigurationsdatei empfiehlt es sich, diese Einstellungen in der Anwendungskonfigurationsdatei zu speichern.  Wenn sich diese Einstellungen in der Computerkonfigurationsdatei befinden, ist das System jedoch u. U. einfacher zu warten.  Wird z. B. die Komponente eines Drittanbieters sowohl von der Client\- als auch von der Serveranwendung verwendet, ist es einfacher, die Einstellungen für diese Komponente in der gleichen Datei zu speichern.  In diesem Fall ist die Computerkonfigurationsdatei dafür am besten geeignet, und Sie vermeiden redundante Informationen in zwei verschiedenen Dateien.  
  
> [!NOTE]
>  Wenn Sie eine Anwendung mithilfe von XCOPY bereitstellen, werden die Einstellungen nicht in die Computerkonfigurationsdatei kopiert.  
  
 Weitere Informationen dazu, wie die Common Language Runtime die Computerkonfigurationsdatei für die Assemblybindung verwendet, finden Sie unter [So sucht Common Language Runtime nach Assemblys](../../../docs/framework/deployment/how-the-runtime-locates-assemblies.md).  
  
## Anwendungskonfigurationsdateien  
 Eine Anwendungskonfigurationsdatei enthält App\-spezifische Einstellungen.  Eine solche Datei enthält Konfigurationseinstellungen, die von der Common Language Runtime gelesen werden \(z. B. Assemblybindungsrichtlinien, Remotingobjekte usw.\), und Einstellungen, die von der App gelesen werden können.  
  
 Name und Speicherort der Anwendungskonfigurationsdatei sind vom Host der App abhängig; folgende Hosts sind möglich:  
  
-   Von einer ausführbaren Datei gehostete App.  
  
     Diese Apps haben zwei Konfigurationsdateien: eine Quellkonfigurationsdatei, die von Entwicklern während der Entwicklung geändert wird, und eine Ausgabedatei, die mit der Anwendung verteilt wird.  
  
     Wenn Sie in Visual Studio entwickeln, legen Sie die Quellkonfigurationsdatei für Ihre App im Projektverzeichnis ab, und legen Sie die zugehörige Eigenschaft **In Ausgabeverzeichnis kopieren** auf **Immer kopieren** oder **Kopieren, wenn neuer** fest.  Die Konfigurationsdatei trägt den gleichen Namen wie die App, die Dateierweiterung lautet .config.  Beispielsweise sollte eine App mit dem Namen myApp.exe eine Quellkonfigurationsdatei mit dem Namen myApp.exe.config haben.  
  
     Visual Studio kopiert die Quellkonfigurationsdatei automatisch in das Verzeichnis, in dem die kompilierte Assembly platziert wird, um die Ausgabekonfigurationsdatei zu erstellen, die mit der App bereitgestellt wird.  In einigen Fällen wird die Ausgabekonfigurationsdatei von Visual Studio möglicherweise geändert. Weitere Informationen dazu finden Sie im Abschnitt [Umleiten von Assemblyversionen auf App-Ebene](../../../docs/framework/configure-apps/redirect-assembly-versions.md#BKMK_Redirectingassemblyversionsattheapplevel) des Artikels [Umleiten von Assemblyversionen](../../../docs/framework/configure-apps/redirect-assembly-versions.md).  
  
-   Von ASP.NET gehostete App.  
  
     Weitere Informationen zu ASP.NET\-Konfigurationsdateien finden Sie unter [ASP.NET\-Konfigurationseinstellungen](http://msdn.microsoft.com/de-de/116608f3-c03d-4413-9fc7-978703e18b0f).  
  
-   Von Internet Explorer gehostete App.  
  
     Verfügt eine von Internet Explorer gehostete App über eine Konfigurationsdatei, wird der Speicherort dieser Datei in einem `<link>`\-Tag mit folgender Syntax angegeben:  
  
     \<link rel\="*Konfigurationsdateiname*" href\="*location*"\>  
  
     In diesem Tag bezeichnet `location` die URL, an der sich die Konfigurationsdatei befindet.  Dadurch wird die App\-Basis festgelegt.  Die Konfigurationsdatei und die App müssen sich in derselben Website befinden.  
  
## Sicherheitskonfigurationsdateien  
 Sicherheitskonfigurationsdateien enthalten Informationen zur Codegruppenhierarchie und zu Berechtigungen, die einer Richtlinienebene zugeordnet sind.  Es wird dringend empfohlen, mithilfe des [Sicherheitsrichtlinientools für den Codezugriff \(Caspol.exe\)](../../../docs/framework/tools/caspol-exe-code-access-security-policy-tool.md) die Sicherheitsrichtlinie dahingehend zu ändern, dass Richtlinienänderungen keinesfalls zur Beschädigung der Sicherheitskonfigurationsdateien führen können.  
  
> [!NOTE]
>  Ab [!INCLUDE[net_v40_short](../../../includes/net-v40-short-md.md)] sind die Sicherheitskonfigurationsdateien nur vorhanden, wenn die Sicherheitsrichtlinien geändert wurden.  
  
 Die Sicherheitskonfigurationsdateien sind an folgenden Speicherorten abgelegt:  
  
-   Konfigurationsdatei für Unternehmensrichtlinien: %*Laufzeit\-Installationspfad*%\\Config\\Enterprisesec.config  
  
-   Konfigurationsdatei für Computerrichtlinien: %*Laufzeit\-Installationspfad*%\\Config\\Security.config  
  
-   Konfigurationsdatei für Benutzerrichtlinien: %USERPROFILE%\\Application data\\Microsoft\\CLR security config\\v*xx.xx*\\Security.config  
  
## In diesem Abschnitt  
 [Gewusst wie: Suchen von Assemblys mit DEVPATH](../../../docs/framework/configure-apps/how-to-locate-assemblies-by-using-devpath.md)  
 Beschreibt, wie die Common Language Runtime angewiesen wird, beim Suchen nach Assemblys die DEVPATH\-Umgebungsvariable zu verwenden.  
  
 [Umleiten von Assemblyversionen](../../../docs/framework/configure-apps/redirect-assembly-versions.md)  
 Beschreibt das Festlegen des Speicherorts und der zu verwendenden Version einer Assembly.  
  
 [Festlegen des Speicherortes einer Assembly](../../../docs/framework/configure-apps/specify-assembly-location.md)  
 Beschreibt, wie festgelegt wird, wo die Common Language Runtime nach einer Assembly suchen soll.  
  
 [Konfigurieren kryptografischer Klassen](../../../docs/framework/configure-apps/configure-cryptography-classes.md)  
 Beschreibt die Zuordnung eines Algorithmusnamens zu einer kryptografischen Klasse und eines Objektbezeichners zu einem kryptografischen Algorithmus.  
  
 [Gewusst wie: Erstellen einer Herausgeberrichtlinie](../../../docs/framework/configure-apps/how-to-create-a-publisher-policy.md)  
 Beschreibt die Situationen, in denen Sie eine Herausgeberrichtliniendatei hinzufügen sollten, um Assemblyumleitung und CodeBase\-Einstellungen anzugeben, sowie die entsprechende Vorgehensweise.  
  
 [Konfigurationsdateischema](../../../docs/framework/configure-apps/file-schema/index.md)  
 Beschreibt die Schemahierarchie für Startup, Laufzeit, Netzwerk und anderen Arten von Konfigurationseinstellungen.  
  
## Siehe auch  
 [Konfigurationsdateischema](../../../docs/framework/configure-apps/file-schema/index.md)   
 [Festlegen des Speicherortes einer Assembly](../../../docs/framework/configure-apps/specify-assembly-location.md)   
 [Umleiten von Assemblyversionen](../../../docs/framework/configure-apps/redirect-assembly-versions.md)   
 [Registrieren von Remoteobjekten mit Konfigurationsdateien](http://msdn.microsoft.com/de-de/bc503ee1-c811-4f82-9525-470343326adc)   
 [ASP.NET Web Site Administration](../Topic/ASP.NET%20Web%20Site%20Administration.md)   
 [NIB: Security Policy Management](http://msdn.microsoft.com/de-de/d754e05d-29dc-4d3a-a2c2-95eaaf1b82b9)   
 [Caspol.exe \(Code Access Security Policy Tool\)](../../../docs/framework/tools/caspol-exe-code-access-security-policy-tool.md)   
 [Assemblys in der Common Language Runtime \(CLR\)](../../../docs/framework/app-domains/assemblies-in-the-common-language-runtime.md)   
 [Remoteobjekte](http://msdn.microsoft.com/de-de/515686e6-0a8d-42f7-8188-73abede57c58)