---
title: "Rand und Abstand in Windows Forms-Steuerelementen | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "Layout [Windows Forms], Ränder und Abstand"
  - "Margin-Eigenschaft [Windows Forms]"
  - "Padding-Eigenschaft [Windows Forms]"
  - "Windows Forms, Layout"
ms.assetid: 3781b5a1-3085-4072-bed0-44670c23ffdc
caps.latest.revision: 5
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 5
---
# Rand und Abstand in Windows Forms-Steuerelementen
Die präzise Platzierung von Steuerelementen auf dem Formular hat für viele Anwendungen einen hohen Stellenwert.  Der <xref:System.Windows.Forms?displayProperty=fullName>\-Namespace bietet Ihnen hierfür zahlreiche Layoutfunktionen.  Zwei der wichtigsten Funktionen sind die <xref:System.Windows.Forms.Control.Margin%2A>\-Eigenschaft und die <xref:System.Windows.Forms.Control.Padding%2A>\-Eigenschaft.  
  
 Die <xref:System.Windows.Forms.Control.Margin%2A>\-Eigenschaft definiert den Bereich um das Steuerelement, durch den andere Steuerelemente einen bestimmten Abstand von den Rändern des Steuerelements einhalten müssen.  
  
 Die <xref:System.Windows.Forms.Control.Padding%2A>\-Eigenschaft definiert den Bereich innerhalb eines Steuerelements, durch den der Inhalt des Steuerelements \(z. B. der Wert seiner <xref:System.Windows.Forms.Control.Text%2A>\-Eigenschaft\) einen bestimmten Abstand von den Rändern des Steuerelements einhalten muss.  
  
 Die folgende Abbildung zeigt die <xref:System.Windows.Forms.Control.Padding%2A>\-Eigenschaft und die <xref:System.Windows.Forms.Control.Margin%2A>\-Eigenschaft für ein Steuerelement.  
  
 ![Ränder und Abstände bei Windows Forms&#45;Steuerelementen](../../../../docs/framework/winforms/controls/media/vs-winformpadmargin.gif "VS\_WinFormPadMargin")  
  
 Diese Funktion wird zur Entwurfszeit in Visual Studio unterstützt.  Siehe auch [Exemplarische Vorgehensweise: Anordnen von Windows Forms\-Steuerelementen mithilfe von Abständen, Rändern und der AutoSize\-Eigenschaft](http://msdn.microsoft.com/library/3z3f9e8b\(v=vs.110\)).  
  
## Siehe auch  
 <xref:System.Windows.Forms.Control.AutoSize%2A>   
 <xref:System.Windows.Forms.Control.Margin%2A>   
 <xref:System.Windows.Forms.Control.Padding%2A>   
 <xref:System.Windows.Forms.Control.LayoutEngine%2A>   
 <xref:System.Windows.Forms.TableLayoutPanel>   
 <xref:System.Windows.Forms.FlowLayoutPanel>