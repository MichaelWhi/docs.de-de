---
title: "Generierung von &#196;nderungen in SQL | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 2188a39d-46ed-4a8b-906a-c9f15e6fefd1
caps.latest.revision: 3
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 3
---
# Generierung von &#196;nderungen in SQL
In diesem Abschnitt wird erläutert, wie ein SQL\-Änderungsgenerierungsmodul für den Anbieter \(von SQL:1999\-kompatiblen Datenbanken\) entwickelt wird.  Mit diesem Modul wird eine Änderungsbefehlsstruktur in die entsprechenden INSERT\-, UPDATE\- oder DELETE\-Anweisungen von SQL übersetzt.  
  
 Informationen zur Generierung von bestimmten Anweisungen in SQL finden Sie unter [SQL\-Generierung](../../../../../docs/framework/data/adonet/ef/sql-generation.md).  
  
## Übersicht über Änderungsbefehlsstrukturen  
 Das SQL\-Änderungsgenerierungsmodul generiert datenbankspezifische SQL\-Änderungsanweisungen anhand einer bestimmten eingegebenen DbModificationCommandTree.  
  
 Eine DbModificationCommandTree ist eine Objektmodelldarstellung eines DML\- Änderungsvorgangs \(ein Einfüge\-, Update\- oder Löschvorgang\), der von DbCommandTree erbt.  Es gibt drei Implementierungen von DbModificationCommandTree:  
  
-   DbInsertCommandTree  
  
-   DbUpdateCommandTree  
  
-   DbDeleteCommandTree  
  
 DbModificationCommandTree und dessen von [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] erzeugten Implementierungen entsprechen immer einem einzelnen Zeilenvorgang.  In diesem Abschnitt werden diese Typen mit ihren Einschränkungen in .NET Framework 3.5 beschrieben.  
  
 ![Diagramm](../../../../../docs/framework/data/adonet/ef/media/558ba7b3-dd19-48d0-b91e-30a76415bf5f.gif "558ba7b3\-dd19\-48d0\-b91e\-30a76415bf5f")  
  
 DbModificationCommandTree verfügt über eine Zieleigenschaft, die den Zielsatz für den Änderungsvorgang darstellt.  Die Ausdruckseigenschaft des Ziels, mit der der Eingabesatz definiert wird, ist immer DbScanExpression.  Ein DbScanExpression kann entweder eine Tabelle, eine Ansicht oder einen mit einer Abfrage definierten Datensatz darstellen, sofern die definierende Abfrage der Metadateneigenschaft des Ziels nicht NULL ist.  
  
 Ein DbScanExpression, der eine Abfrage darstellt, kann nur dann einen Anbieter als Änderungsziel erreichen, wenn der Satz im Modell mit einer definierenden Abfrage definiert wurde, jedoch keine Funktion für den entsprechenden Änderungsvorgang bereitgestellt wurde.  Anbieter \(z. B. SqlClient\) können ein solches Szenario möglicherweise nicht unterstützen.  
  
 DbInsertCommandTree stellt einen einzeiligen Einfügevorgang dar, der als Befehlsstruktur ausgedrückt wird.  
  
```  
public sealed class DbInsertCommandTree : DbModificationCommandTree {  
   public IList<DbModificationClause> SetClauses { get }  
   public DbExpression Returning { get }  
}  
```  
  
 DbUpdateCommandTree stellt einen einzeiligen Aktualisierungsvorgang dar, der als Befehlsstruktur ausgedrückt wird.  
  
 DbDeleteCommandTree stellt eine einzeiligen Löschvorgang dar, der als Befehlsstruktur ausgedrückt wird.  
  
### Einschränkungen für Struktureigenschaften von Änderungsbefehlen  
 Die folgenden Informationen und Einschränkungen gelten für die Struktureigenschaften von Änderungsbefehlen.  
  
#### Returning in DbInsertCommandTree und DbUpdateCommandTree  
 Wenn Returning nicht NULL ist, gibt er an, dass der Befehl einen Reader zurückgibt.  Andernfalls sollte der Befehl einen Skalarwert zurückgeben, der die Anzahl der betroffenen \(eingefügten oder aktualisierten\) Zeilen angibt.  
  
 Der Returning\-Wert legt eine Projektion der Ergebnisse fest, die auf Grundlage der eingefügten oder aktualisierten Zeile zurückzugeben sind.  Es kann sich lediglich um den Typ DbNewInstanceExpression handeln, der eine Zeile darstellt, deren Argumente jeweils ein DbPropertyExpression für einen DbVariableReferenceExpression sind, der einen Verweis auf das Ziel der entsprechenden DbModificationCommandTree darstellt.  Bei den von den DbPropertyExpressions dargestellten Eigenschaften, die in der Returning\-Eigenschaft verwendet werden, handelt es sich stets um im Speicher generierte oder berechnete Werte.  In DbInsertCommandTree ist Returning nicht NULL, wenn mindestens eine Eigenschaft der Tabelle, in die die Zeile eingefügt wird, als im Speicher generiert oder berechnet festgelegt ist \(in SSDL als StoreGeneratedPattern.Identity oder StoreGeneratedPattern.Computed markiert\).  In DbUpdateCommandTrees ist Returning nicht NULL, wenn mindestens eine Eigenschaft der Tabelle, in der die Zeile aktualisiert wird, als im Speicher berechnet festgelegt ist \(in SSDL als StoreGeneratedPattern.Computed markiert\).  
  
#### SetClauses in DbInsertCommandTree und DbUpdateCommandTree  
 SetClauses legt die Liste der **SET**\-Klauseln zum Einfügen oder Aktualisieren fest, mit denen der Einfüge\- oder Aktualisierungsvorgang definiert wird.  
  
```  
The elements of the list are specified as type DbModificationClause, which specifies a single clause in an insert or update modification operation. DbSetClause inherits from DbModificationClause and specifies the clause in a modification operation that sets the value of a property. Beginning in version 3.5 of the .NET Framework, all elements in SetClauses are of type SetClause.   
```  
  
 Property legt die zu aktualisierende Eigenschaft fest.  Es handelt sich stets um einen DbPropertyExpression für einen DbVariableReferenceExpression, der einen Verweis auf das Ziel der entsprechenden DbModificationCommandTree darstellt.  
  
 Value legt den neuen Wert für die Aktualisierung der Eigenschaft fest.  Er ist entweder vom Typ DbConstantExpression oder DbNullExpression.  
  
#### Predicate in DbUpdateCommandTree und DbDeleteCommandTree  
 Predicate legt das Prädikat fest, mit dem die zu aktualisierenden oder zu löschenden Member der Zielauflistung festgelegt werden.  Es handelt sich um eine aus der folgenden Teilmenge von DbExpressions erstellte Ausdrucksstruktur:  
  
-   DbComparisonExpression der Art Equals, wobei das rechte untergeordnete Element ein DbPropertyExression \(wie unten eingeschränkt\) und das linke untergeordnete Element ein DbConstantExpression ist.  
  
-   DbConstantExpression  
  
-   DbIsNullExpression für einen DbPropertyExpression, wie unten eingeschränkt  
  
-   Ein DbPropertyExpression für einen DbVariableReferenceExpression, der einen Verweis auf das Ziel der entsprechenden DbModificationCommandTree darstellt.  
  
-   DbAndExpression  
  
-   DbNotExpression  
  
-   DbOrExpression  
  
## Generierung von Änderungen in SQL im Beispielanbieter  
 Der [Entity Framework\-Beispielanbieter](http://go.microsoft.com/fwlink/?LinkId=180616) veranschaulicht die Komponenten von ADO.NET\-Datenanbietern, die [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] unterstützen.  Beim Ziel handelt es sich um eine SQL Server 2005\-Datenbank. Die Implementierung erfolgt als übergeordneter Wrapper des ADO.NET 2.0\-Datenanbieters System.Data.SqlClient.  
  
 Das SQL\-Änderungsgenerierungsmodul des Beispielanbieters \(das sich in der Datei SQL **Generation\\DmlSqlGenerator.cs** befindet\) erstellt mit einer eingegebenen DbModificationCommandTree eine einzelne SQL\-Änderungsanweisung, möglicherweise gefolgt von einer Auswahlanweisung, um einen Reader zurückzugeben, sofern dies durch die DbModificationCommandTree festgelegt wurde.  Beachten Sie, dass die Form der generierten Befehle von der SQL Server\-Zieldatenbank abhängig ist.  
  
### Hilfsklassen: ExpressionTranslator  
 ExpressionTranslator dient für alle Struktureigenschaften von Änderungsbefehlen des Typs DbExpression als allgemeines einfaches Konvertierungsprogramm.  Es unterstützt lediglich die Übersetzung der Ausdruckstypen, für die die Eigenschaften der Änderungsbefehlsstruktur eingeschränkt sind und wird anhand dieser Einschränkungen erstellt.  
  
 Im Folgenden wird der Zugriff auf bestimmte Ausdruckstypen erläutert \(Knoten mit trivialen Übersetzungen werden ausgelassen\).  
  
### DbComparisonExpression  
 Wenn der ExpressionTranslator mit den preserveMemberValues **true** erstellt wird und wenn die rechte Konstante ein DbConstantExpression \(anstelle von DbNullExpression\) ist, wird der linke Operand \(eDbPropertyExpressions\) diesem DbConstantExpression zugeordnet.  Dies wird verwendet, wenn eine Select\-Rückgabeanweisung generiert werden muss, um die betroffene Zeile zu identifizieren.  
  
### DbConstantExpression  
 Für jede Konstante, auf die zugegriffen wird, wird ein Parameter erstellt.  
  
### DbPropertyExpression  
 Vorausgesetzt, die Instanz von DbPropertyExpression stellt immer die Eingabetabelle dar, und bei der Generierung wurde kein Alias erstellt \(dies geschieht nur in Updateszenarien unter Verwendung einer Tabellenvariable\), muss kein Alias für die Eingabe angegeben werden. Die Übersetzung wird in der Standardeinstellung auf den Eigenschaftennamen festgelegt.  
  
## Generieren eines SQL\-Insert\-Befehls  
 Der generierte Einfügebefehl folgt für eine bestimmte DbInsertCommandTree im Beispielanbieter einer der beiden folgenden Einfügevorlagen.  
  
 Die erste Vorlage verfügt über einen Befehl, mit dem der Einfügevorgang mithilfe der Werte in der Liste mit SetClauses durchgeführt werden kann, sowie über eine SELECT\-Anweisung, um die in der Returning\-Eigenschaft für die eingefügte Zeile angegebenen Eigenschaften zurückzugeben, sofern die Returning\-Eigenschaft nicht NULL ist.  Das Prädikatelement "@@ROWCOUNT \> 0" hat den Wert "true", wenn eine Zeile eingefügt wurde.  Das Prädikatelement "keyMemberI \= keyValueI &#124; scope\_identity\(\)" nimmt nur dann die Form "keyMemberI \= scope\_identity\(\)" an, wenn keyMemeberI ein im Speicher generierter Schlüssel ist, da scope\_identity\(\) den letzten in eine \(im Speicher generierte\) Identitätsspalte eingefügten Identitätswert zurückgibt.  
  
```  
-- first insert Template  
INSERT <target>   [ (setClauseProperty0, .. setClausePropertyN)]    
VALUES (setClauseValue0, .. setClauseValueN) |  DEFAULT VALUES   
  
[SELECT <returning>   
 FROM <target>   
 WHERE @@ROWCOUNT > 0 AND keyMember0 = keyValue0 AND .. keyMemberI =  keyValueI | scope_identity()  .. AND  keyMemberN = keyValueN]  
```  
  
 Die zweite Vorlage ist erforderlich, wenn die Einfügung das Einfügen einer Zeile festlegt, bei der der Primärschlüssel im Speicher generiert wurde, bei dem es sich jedoch nicht um einen ganzzahligen Typ handelt, sodass er nicht mit scope\_identity\(\) verwendet werden kann.  Diese Vorlage wird auch verwendet, wenn ein im Speicher generierter Verbundschlüssel vorhanden ist.  
  
```  
-- second insert template  
DECLARE @generated_keys TABLE [(keyMember0, … keyMemberN)  
  
INSERT <target>   [ (setClauseProperty0, .. setClausePropertyN)]    
 OUTPUT inserted.KeyMember0, …, inserted.KeyMemberN INTO @generated_keys  
 VALUES (setClauseValue0, .. setClauseValueN) |  DEFAULT VALUES   
  
[SELECT <returning_over_t>   
 FROM @generated_keys  AS g   
JOIN <target> AS t ON g.KeyMember0 = t.KeyMember0 AND … g.KeyMemberN = t.KeyMemberN  
 WHERE @@ROWCOUNT > 0   
```  
  
 Im Folgenden finden Sie ein Beispiel, in dem das im Beispielanbieter enthaltene Modell verwendet wird.  Es wird ein Einfügebefehl aus einer DbInsertCommandTree erstellt.  
  
 Mit folgendem Code wird eine Kategorie eingefügt:  
  
```  
using (NorthwindEntities northwindContext = new NorthwindEntities()) {  
   Category c = new Category();  
   c.CategoryName = "Test Category";  
   c.Description = "A new category for testing";  
   northwindContext.AddObject("Categories", c);  
   northwindContext.SaveChanges();  
}  
```  
  
 Mit diesem Code wird die folgende Befehlsstruktur erzeugt, die an den Anbieter übergeben wird:  
  
```  
DbInsertCommandTree  
|_Parameters  
|_Target : 'target'  
| |_Scan : dbo.Categories  
|_SetClauses  
| |_DbSetClause  
| | |_Property  
| | | |_Var(target).CategoryName  
| | |_Value  
| |   |_'Test Category'  
| |_DbSetClause  
| | |_Property  
| | | |_Var(target).Description  
| | |_Value  
| |   |_'A new category for testing'  
| |_DbSetClause  
|   |_Property  
|   | |_Var(target).Picture  
|   |_Value  
|     |_null  
|_Returning  
  |_NewInstance : Record['CategoryID'=Edm.Int32]  
    |_Column : 'CategoryID'  
      |_Var(target).CategoryID  
```  
  
 Beim vom Beispielanbieter erzeugten Speicherbefehl handelt es sich um die folgende SQL\-Anweisung:  
  
```  
insert [dbo].[Categories]([CategoryName], [Description], [Picture])  
values (@p0, @p1, null)  
select [CategoryID]  
from [dbo].[Categories]  
where @@ROWCOUNT > 0 and [CategoryID] = scope_identity()  
```  
  
## Generieren eines SQL\-Update\-Befehls  
 Der generierte Updatebefehl beruht für eine bestimmte DbUpdateCommandTree auf der folgenden Vorlage:  
  
```  
-- UPDATE Template   
UPDATE <target>   
SET setClauseProprerty0 = setClauseValue0,  .. setClauseProprertyN = setClauseValueN  | @i = 0  
WHERE <predicate>  
  
[SELECT <returning>   
 FROM <target>   
 WHERE @@ROWCOUNT > 0 AND keyMember0 = keyValue0 AND .. keyMemberI =  keyValueI | scope_identity()  .. AND  keyMemberN = keyValueN]  
```  
  
 Die **SET**\-Klausel verfügt nur dann über die falsche **SET**\-Klausel \("@i \= 0"\), wenn keine **SET**\-Klauseln angegeben wurden.  Auf diese Weise wird sichergestellt, dass alle im Speicher berechneten Spalten erneut berechnet werden.  
  
 Nur wenn die Returning\-Eigenschaft nicht NULL ist, wird eine Auswahlanweisung generiert, um die in der Returning\-Eigenschaft angegebenen Eigenschaften zurückzugeben.  
  
 Im folgenden Beispiel wird mithilfe des im Beispielanbieter enthaltenen Modells ein Updatebefehl generiert.  
  
 Mit folgendem Benutzercode wird eine Kategorie aktualisiert:  
  
```  
using (NorthwindEntities northwindContext = new NorthwindEntities()) {  
   Category c = northwindContext.Categories.Where(i => i.CategoryName == "Test Category").First();  
   c.CategoryName = "New test name";  
   northwindContext.SaveChanges();  
}  
```  
  
 Mit diesem Benutzercode wird die folgende Befehlsstruktur erstellt, die an den Anbieter übergeben wird:  
  
```  
DbUpdateCommandTree  
|_Parameters  
|_Target : 'target'  
| |_Scan : dbo.Categories  
|_SetClauses  
| |_DbSetClause  
|   |_Property  
|   | |_Var(target).CategoryName  
|   |_Value  
|     |_'New test name'  
|_Predicate  
| |_  
|   |_Var(target).CategoryID  
|   |_=  
|   |_10  
|_Returning   
```  
  
 Der Beispielanbieter erzeugt den folgenden Speicherbefehl:  
  
```  
update [dbo].[Categories]  
set [CategoryName] = @p0  
where ([CategoryID] = @p1)   
```  
  
### Generieren eines SQL\-Delete\-Befehls  
 Für eine bestimmte DbDeleteCommandTree beruht der generierte DELETE\-Befehl auf der folgenden Vorlage:  
  
```  
-- DELETE Template   
DELETE <target>   
WHERE <predicate>  
```  
  
 Im folgenden Beispiel wird mithilfe des im Beispielanbieter enthaltenen Modells ein Löschbefehl generiert.  
  
 Mit folgendem Benutzercode wird eine Kategorie gelöscht:  
  
```  
using (NorthwindEntities northwindContext = new NorthwindEntities()) {  
   Category c = northwindContext.Categories.Where(i => i.CategoryName == "New test name").First();  
   northwindContext.DeleteObject(c);  
   northwindContext.SaveChanges();  
}  
```  
  
 Mit diesem Benutzercode wird die folgende Befehlsstruktur erstellt, die an den Anbieter übergeben wird.  
  
```  
DbDeleteCommandTree  
|_Parameters  
|_Target : 'target'  
| |_Scan : dbo.Categories  
|_Predicate  
  |_  
    |_Var(target).CategoryID  
    |_=  
    |_10  
```  
  
 Der Beispielanbieter erzeugt den folgenden Speicherbefehl:  
  
```  
delete [dbo].[Categories]  
where ([CategoryID] = @p0)  
```  
  
## Siehe auch  
 [Schreiben eines Entity Framework\-Datenanbieters](../../../../../docs/framework/data/adonet/ef/writing-an-ef-data-provider.md)