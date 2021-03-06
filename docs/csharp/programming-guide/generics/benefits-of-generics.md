---
title: Vorteile von Generika (C#-Programmierhandbuch)
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- generics [C#], benefits
ms.assetid: 80f037cd-9ea7-48be-bfc1-219bfb2d4277
caps.latest.revision: 23
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: 8b05a3a695064764618564293e6ccd92e2ce60be
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="benefits-of-generics-c-programming-guide"></a>Vorteile von Generika (C#-Programmierhandbuch)
Generika bieten die Lösung für eine Einschränkung in früheren Versionen der Common Language Runtime und der C#-Sprache, bei der Generalisierung durch die Umwandlung von Typen in den und aus dem universellen Basistyp <xref:System.Object> erfolgt. Durch Erstellen einer generischen Klasse können Sie eine Auflistung erstellen, die zur Kompilierzeit typsicher ist.  
  
 Die Einschränkungen bei der Verwendung nicht generischer Auflistungsklassen können durch das Schreiben eines kurzen Programms veranschaulicht werden, das die Auflistungsklasse <xref:System.Collections.ArrayList> aus der .NET Framework-Klassenbibliothek verwendet. <xref:System.Collections.ArrayList> ist eine äußerst praktische Auflistungsklasse, die unverändert verwendet werden kann, um einen beliebigen Verweis- oder Werttyp zu speichern.  
  
 [!code-cs[csProgGuideGenerics#4](../../../csharp/programming-guide/generics/codesnippet/CSharp/benefits-of-generics_1.cs)]  
  
 Aber diese Vereinfachung ist mit Nachteilen verbunden. Jeder Verweis- oder Werttyp, der zu einem <xref:System.Collections.ArrayList>-Objekt hinzugefügt wird, wird implizit nach oben in <xref:System.Object> umgewandelt. Wenn es sich bei den Elementen um Werttypen handelt, müssen Sie mittels Boxing geschachtelt werden, wenn sie zur Liste hinzugefügt werden, und mittels Unboxing wieder entpackt werden, wenn sie abgerufen werden. Sowohl die Umwandlung von Typen als auch das Boxing und Unboxing beeinträchtigen die Leistung; die Auswirkung von Boxing und Unboxing kann sehr groß sein, wenn Sie lange Auflistungen durchlaufen müssen.  
  
 Die andere Einschränkung ist die fehlende Typüberprüfung zur Kompilierzeit; da ein <xref:System.Collections.ArrayList>-Objekt alles in <xref:System.Object> umwandelt, kann zur Kompilierzeit nicht vermieden werden, dass der Clientcode etwas wie das Folgende macht:  
  
 [!code-cs[csProgGuideGenerics#5](../../../csharp/programming-guide/generics/codesnippet/CSharp/benefits-of-generics_2.cs)]  
  
 Zwar ist das Erstellen einer heterogenen Auflistung durchaus akzeptabel und manchmal beabsichtigt, aber das Kombinieren von Zeichenfolgen und `ints` in einem einzigen <xref:System.Collections.ArrayList>-Objekt ist meist eher ein Programmierfehler, der bis zur Laufzeit nicht erkannt wird.  
  
 In den Versionen 1.0 und 1.1 der C#-Sprache können Sie generalisierten Code in der Auflistung der .NET Framework-Basisklassenbibliothek nur vermeiden, wenn Sie Ihre eigenen typspezifischen Auflistungen schreiben. Da eine solche Klasse nur für einen Datentyp verwendet werden kann, verlieren Sie so aber die Vorteile der Generalisierung und müssen die Klasse für jeden Typ, der gespeichert wird, umschreiben.  
  
 Was <xref:System.Collections.ArrayList> und ähnliche Klassen eigentlich benötigen, ist eine Möglichkeit für den Clientcode, instanzweise den zu verwendenden Datentyp anzugeben. Dann wäre keine Typumwandlung nach oben zu `T:System.Object` mehr nötig, und der Compiler könnte eine Typüberprüfung durchführen. Das heißt, dass <xref:System.Collections.ArrayList> einen Typparameter benötigt. Genau das bieten Generika. In der generischen <xref:System.Collections.Generic.List%601>-Auflistung im `N:System.Collections.Generic`-Namespace sieht der gleiche Vorgang des Hinzufügens von Elementen zur Auflistung ungefähr so aus:  
  
 [!code-cs[csProgGuideGenerics#6](../../../csharp/programming-guide/generics/codesnippet/CSharp/benefits-of-generics_3.cs)]  
  
 Bei Clientcode ist im Vergleich von <xref:System.Collections.Generic.List%601> mit <xref:System.Collections.ArrayList> als Syntax lediglich das Typargument in der Deklaration und Instanziierung hinzugekommen. Zwar ist die Codierung etwas komplexer, aber dafür können Sie eine Liste erstellen, die nicht nur sicherer als <xref:System.Collections.ArrayList> ist, sondern auch bedeutend schneller, vor allem wenn es sich bei den Listenelementen um Werttypen handelt.  
  
## <a name="see-also"></a>Siehe auch  
 <xref:System.Collections.Generic>   
 [C#-Programmierhandbuch](../../../csharp/programming-guide/index.md)   
 [Einführung in Generika](../../../csharp/programming-guide/generics/introduction-to-generics.md)   
 [Boxing und Unboxing](../../../csharp/programming-guide/types/boxing-and-unboxing.md)   
 [Best Practices für Auflistungen](http://go.microsoft.com/fwlink/?LinkId=112403)

