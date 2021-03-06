---
title: Main() und Befehlszeilenargumente (C#-Programmierhandbuch)
ms.date: 2017-08-02
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- CS5001
- main_CSharpKeyword
- Main
dev_langs:
- CSharp
helpviewer_keywords:
- Main method [C#]
- C# language, command-line arguments
- arguments [C#], command-line
- command line [C#], arguments
- command-line arguments [C#], Main method
ms.assetid: 73a17231-cf96-44ea-aa8a-54807c6fb1f4
caps.latest.revision: 30
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
ms.sourcegitcommit: d019d1c5757a961c03439d756e808ae13fd8a67b
ms.openlocfilehash: 51408654abd0dcd2f7159438b507c44bd579bfd9
ms.contentlocale: de-de
ms.lasthandoff: 08/03/2017

---
# <a name="main-and-command-line-arguments-c-programming-guide"></a>Main() und Befehlszeilenargumente (C#-Programmierhandbuch)

Die `Main`-Methode ist der Einstiegspunkt einer C#-Anwendung. (Bibliotheken und Dienste erfordern keine `Main`-Methode als Einstiegspunkt.) Wenn die Anwendung gestartet wird, ist die `Main`-Methode die erste Methode, die aufgerufen wird.

 In einem C#-Programm kann nur ein Einstiegspunkt vorhanden sein. Wenn Sie mehr als eine Klasse mit einer `Main`-Methode aufweisen, müssen Sie das Programm mit der **/main**-Compileroption kompilieren, um anzugeben, welche `Main`-Methode als Einstiegspunkt verwendet wird. Weitere Informationen hierzu finden Sie unter [/main (C# Compiler Options)](../../../csharp/language-reference/compiler-options/main-compiler-option.md).

 [!code-cs[csProgGuideMain#17](../../../csharp/programming-guide/inside-a-program/codesnippet/CSharp/main-and-command-line-arguments_1.cs)]

## <a name="overview"></a>Übersicht

- Die `Main`-Methode ist der Einstiegspunkt eines ausführbaren Programms. Hier beginnt und endet die Programmsteuerung.
- `Main` wird innerhalb einer Klasse oder Struktur deklariert. `Main` muss [statisch](../../../csharp/language-reference/keywords/static.md), aber nicht [öffentlich](../../../csharp/language-reference/keywords/public.md) sein. (Im Beispiel oben erhält es den Standardzugriff [private](../../../csharp/language-reference/keywords/private.md).) Die einschließende Klasse oder Struktur muss nicht statisch sein.
- `Main` kann über die Rückgabetypen `void` oder `int` verfügen; seit C# 7.1 sind auch die Rückgabetypen `Task` oder `Task<int>` möglich.
- Wenn – und nur wenn – `Main` einen `Task`- oder `Task<int>`-Wert zurückgibt, darf die Deklaration von `Main` den [`async`](../../language-reference/keywords/async.md)-Modifizierer enthalten. Beachten Sie, dass diese insbesondere eine `async void Main`-Methode ausschließt.
- Die `Main`-Methode kann mit oder ohne `string[]`-Parameter deklariert werden, der die Befehlszeilenargumente enthält. Bei Verwendung von [!INCLUDE[vsprvs](~/includes/vsprvs-md.md)] zum Erstellen von Windows-Anwendungen können Sie den Parameter manuell hinzufügen oder die <xref:System.Environment>-Klasse verwenden, um die Befehlszeilenargumente abzurufen. Parameter werden als mit Null indizierte Befehlszeilenargumente gelesen. Im Gegensatz zu C und C++ wird der Name des Programms nicht als erstes Befehlszeilenargument behandelt.

Das Hinzufügen der `async`-Rückgabetypen `Task` und `Task<int>` vereinfacht den Programmcode, wenn Konsolenanwendungen asynchrone Vorgänge in `Main` starten und mit `await` darauf warten müssen.

## <a name="c-language-specification"></a>C#-Sprachspezifikation

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Siehe auch
[Erstellen über die Befehlszeile mit „csc.exe“](../../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md)
[C#-Programmierhandbuch](../../../csharp/programming-guide/index.md)
[Methoden](../../../csharp/programming-guide/classes-and-structs/methods.md)
[In einem C#-Programm](../../../csharp/programming-guide/inside-a-program/index.md)

