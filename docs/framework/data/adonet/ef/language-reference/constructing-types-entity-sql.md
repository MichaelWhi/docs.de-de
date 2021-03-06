---
title: "Konstruktionstypen (Entity SQL) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "ESQL"
ms.assetid: 41fa7bde-8d20-4a3f-a3d2-fb791e128010
caps.latest.revision: 2
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 2
---
# Konstruktionstypen (Entity SQL)
[!INCLUDE[esql](../../../../../../includes/esql-md.md)] stellt drei Arten von Konstruktoren zur Verfügung: Zeilenkonstruktoren, Konstruktoren benannter Typen und Auflistungskonstruktoren.  
  
## Zeilenkonstruktoren  
 Zeilenkonstruktoren werden in [!INCLUDE[esql](../../../../../../includes/esql-md.md)] zur Erstellung anonymer, strukturell typisierter Datensätze aus einem oder mehreren Werten verwendet.  Beim Ergebnistyp eines Zeilenkonstruktors handelt es sich um einen Zeilentyp, dessen Feldtypen den Typen der zur Erstellung der Zeile verwendeten Werten entsprechen.  Im folgenden Ausdruck wird beispielsweise ein Wert vom Typ  `Record(a int, b string, c int)` erstellt:  
  
 `ROW(1 AS a, "abc" AS b, a + 34 AS c)`  
  
 Wenn für einen Ausdruck in einem Zeilenkonstruktor kein Alias angegeben ist, wird vom Entity Framework ein Alias generiert.  Weitere Informationen finden Sie im Abschnitt mit Regeln für das Aliasing unter [Bezeichner](../../../../../../docs/framework/data/adonet/ef/language-reference/identifiers-entity-sql.md).  
  
 Die folgenden Regeln gelten für Ausdrucksaliasing in einem Zeilenkonstruktor:  
  
-   Ausdrücke in einem Zeilenkonstruktor können nicht auf andere Aliase im gleichen Konstruktor verweisen.  
  
-   Zwei Ausdrücke im gleichen Zeilenkonstruktor können nicht über den gleichen Alias verfügen.  
  
 Weitere Informationen zu Zeilenkonstruktoren finden Sie unter [ROW](../../../../../../docs/framework/data/adonet/ef/language-reference/row-entity-sql.md).  
  
## Auflistungskonstruktoren  
 Mithilfe von Auflistungskonstruktoren wird in [!INCLUDE[esql](../../../../../../includes/esql-md.md)] aus einer Liste mit Werten eine Instanz eines Multisets erstellt.  Alle Werte im Konstruktor müssen vom beiderseitig kompatiblen Typ `T` sein, und der Konstruktor erstellt eine Auflistung des Typs `Multiset<T>`.  Zum Beispiel erstellt der folgende Ausdruck eine Auflistung von ganzen Zahlen:  
  
 `Multiset(1, 2, 3)`  
  
 `{1, 2, 3}`  
  
 Leere Multiset\-Konstruktoren sind nicht zugelassen, da der Typ der Elemente nicht bestimmt werden kann.  Der folgende Ausdruck ist ungültig:  
  
 `multiset() {}`  
  
 Weitere Informationen finden Sie unter [MULTISET](../../../../../../docs/framework/data/adonet/ef/language-reference/multiset-entity-sql.md).  
  
## Konstruktoren benannter Typen \(NamedType\-Initialisierer\)  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] ermöglicht Typenkonstruktoren \(Initialisierern\) das Erstellen von Instanzen benannter komplexer Typen und Entitätstypen.  Der folgende Ausdruck erstellt z. B. eine Instanz eines `Person`\-Typs.  
  
 `Person("abc", 12)`  
  
 Mit dem folgenden Ausdruck wird eine Instanz eines komplexen Typs erstellt.  
  
 `MyModel.ZipCode(‘98118’, ‘4567’)`  
  
 Mit dem folgenden Ausdruck wird eine Instanz eines geschachtelten komplexen Typs erstellt.  
  
 `MyModel.AddressInfo('My street address', 'Seattle', 'WA', MyModel.ZipCode('98118', '4567'))`  
  
 Mit dem folgenden Ausdruck wird eine Instanz einer Entität mit einem geschachtelten komplexen Typ erstellt.  
  
 `MyModel.Person("Bill", MyModel.AddressInfo('My street address', 'Seattle', 'WA', MyModel.ZipCode('98118', '4567')))`  
  
 Im folgenden Beispiel wird gezeigt, wie eine Eigenschaft eines komplexen Typs mit NULL initialisiert wird.  `MyModel.ZipCode(‘98118’, null)`  
  
 Die Reihenfolge der Argumente des Konstruktors sollten mit der Reihenfolge der Deklaration der Attribute des Typs übereinstimmen.  
  
 Weitere Informationen finden Sie unter [Konstruktoren benannter Typen](../../../../../../docs/framework/data/adonet/ef/language-reference/named-type-constructor-entity-sql.md).  
  
## Siehe auch  
 [Entity SQL\-Referenz](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-reference.md)   
 [Übersicht über Entity SQL](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-overview.md)   
 [Typsystem](../../../../../../docs/framework/data/adonet/ef/language-reference/type-system-entity-sql.md)