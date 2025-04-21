---
tags:
  - template
  - organisation
created: 21.04.2025
up: "[[Organisation]]"
---
[https://arc42.de/overview/](https://arc42.de/overview/)
Softwarearchitektur und Solutiondesign schreiben

Perfekt zur Kommunikation und Dokumentation Ihrer Softwarearchitektur.

arc42 gibt praxisorientierte Antworten auf die folgenden zwei Fragen:

- **_Was_** sollen wir über unsere Architektur kommunizieren/dokumentieren?
- **_Wie_** sollen wir kommunizieren/dokumentieren?

Dazu haben wir ein einfaches Template mit 12 Kapiteln entwickelt, in denen Sie alles Wissenswerte über die Architektur unterbringen können. arc42 lässt sich einfach an Ihre spezifischen Bedürfnisse anpassen.

**1. Einführung & Ziele**  
[Grundlegende Anforderungen, insbesondere Qualitätsziele](https://arc42.de/overview/#introduction-goals)

**2. Randbedingungen**  
[Regelungen und externe Randbedingungen](https://arc42.de/overview/#constraints)[

**3. Kontext & Abgrenzung**  
[Externe Systeme und Schnittstellen](https://arc42.de/overview/#context-scope)

**4. Lösungsstrategie**  
[Kernideen und Lösungsansätze](https://arc42.de/overview/#solution-strategy)

**5. Bausteinsicht**  
[Aufbau des Quellcodes, Modularisierung (hierarchisch)](https://arc42.de/overview/#building-block-view)

**6. Laufzeitsicht**  
[Wichtige Laufzeitszenarien](https://arc42.de/overview/#runtime-view)

**7. Verteilungssicht**  
[Hardware, Infrastruktur & Deployment](https://arc42.de/overview/#deployment-view)

**8. Querschnittliche Konzepte**  
[Querschnittsthemen, oft sehr technisch und detailliert](https://arc42.de/overview/#crosscutting-concepts)

**9. Architekturentscheidungen**  
[Wichtige Entscheidungen (nicht anderweitig beschrieben)](https://arc42.de/overview/#architectural-decisions)

**10. Qualitätsanforderungen**  
[Qualitätsbaum, Qualitätsszenarien](https://arc42.de/overview/#quality-requirements)

**11. Risiken & Technische Schulden**  
[Bekannte Probleme und Risiken](https://arc42.de/overview/#risks-technical-debt)

**12. Glossar**  
[Wichtige und spezifische Begriffe ("gemeinsame Sprache")](https://arc42.de/overview/#glossary)

Den Kern der Architekturdokumentation bilden die Kontextabgrenzung (Kap. 3), die drei Sichten (Bausteinsicht, Laufzeitsicht und Verteilungssicht - Kap. 5 - 7), sowie die querschnittlichen Konzepte (Kap. 8).

Die restlichen Kapitel runden die Dokumentation ab. Sie halten u.a. Ziele, wichtige Entscheidungen und Risiken fest. Ein abschließendes Glossar stellt sicher, dass alle über das Gleiche sprechen.

Wenn Sie mehr zu den Kapiteln wissen wollen, lesen Sie weiter oder lernen Sie noch mehr Details über die jeweiligen Links.

---

# Mehr Details[Permalink](https://arc42.de/overview/#mehr-details "Permalink")
![[01-intro-and-goals.png]]

## 1. Einführung und Ziele
Kurze Beschreibung und Extrakt der **requirements**, Die Top3 (bis maximal 5) **Qualitätsziele für die Architektur**, deren Erreichung für die wichtigsten Stakeholder kritisch ist. Eine Übersicht über die wichtigsten **Stakeholder** mit deren Erwartungen bezüglich der Architektur.
[mehr dazu ...](https://docs.arc42.org/section-1/)

## 2. Randbedingungen
![[02-constraints-overview.png]]
Alles, was das Team beim Design und der Implementierung der Architektur einschränkt. Diese Einschränkungen sind manchmal auch außerhalb eines Projekts in der gesamten Organisation gültig.
[mehr dazu ...](https://docs.arc42.org/section-2/)

## 3. Kontextabgrenzung
![[03-context-overview.png]]
Grenzt das System, an dem Sie arbeiten, von externen Kommunikationspartnern (Nachbarsystemen und Benutzern) ab. Spezifiziert die externen Schnittstellen aus Sicht des Business (immer) und aus Sicht der Technologie (optional)

[mehr dazu ...](https://docs.arc42.org/section-3/)

## 4. Lösungsstrategie
![[04-solution-strategy-overview.svg]]
Zusammenfassung der fundamentalen Entwurfsentscheidungen und Lösungsstrategien für die Gesamtarchitektur. Kann Technologieentscheidungen, Top-Level-Zerlegungsstrategie oder Ansätze zur Erreichung der wesentlichen Qualitätsziele beinhalten, bzw. relevante Organisationsentscheidungen.

[mehr dazu ...](https://docs.arc42.org/section-4/)

## 5. Bausteinsicht
![[05-building-block-overview.png]]
Statische Zerlegung des Systems. Die Abstraktion des Sourcecodes, dargestellt als Hierarchie von “White-Boxes” (die wiederum kleinere Black-Boxes beinhalten), bis zu einem angemessenen Detaillierungsgrad.

[mehr dazu ...](https://docs.arc42.org/section-5/)

## 6. Laufzeitsicht
![[06-runtime-overview.png]]
Das Verhalten der Bausteine in Form von dynamischen Szenarien, die die wichtigsten Prozesse oder Features abdecken, Interaktionen an kritischen externen Schnittstellen oder “interessante” interne Abläufe und kritische Ausnahme- oder Fehlerfälle.

[mehr dazu ...](https://docs.arc42.org/section-6/)

## 7. Verteilungssicht
![[07-deployment-overview.png]]
Technische Infrastruktur mit (echten oder virtuellen) Prozessoren, Systemtopologie, und die Abbildung der Software-Bausteine auf diese Infrastruktur.

[mehr dazu ...](https://docs.arc42.org/section-7/)

## 8. Querschnittliche Konzepte
![[08-concepts-overview.png]]
Übergreifende, generelle Prinzipien und Lösungsansätze, die in vielen Teilen der Architektur einheitlich benutzt werden (→ cross-cutting). Konzepte beziehen sich oft auf **mehrere Bausteine**. Hier findet man Themen wie Domänenmodelle, Architekturmuster und -stile, Regeln zur Nutzung bestimmter Technologiestacks, etc.

[mehr dazu ...](https://docs.arc42.org/section-8/)

## 9. Architekturentscheidungen
![[09-decision-overview.png]]
Wichtige, teure oder kritische oder riskante Architekturentscheidungen, die zentrale Bedeutung für das Gesamtsystem haben, mit Begründungen für diese Entscheidungen.

[mehr dazu ...](https://docs.arc42.org/section-9/)

## 10. Qualitätsanforderungen
![[10-q-scenario-overview.png]]
Qualitätsanforderungen in Form von Szenarien, mit einem Qualitätsbaum für den Überblick. Die allerwichtigsten dieser Qualitätsanforderungen sollten schon im Kapitel 1.2. (Qualitätsziele) aufgeführt sein.

[mehr dazu ...](https://docs.arc42.org/section-10/)

## 11. Risiken und technische Schulden
![[11-risk-overview.png]]
Bekannte Risiken und angehäufte technische Schulden. Welche potentiellen Probleme lauern im und um das System? Über welche Schwächen beklagen sich die Entwicklungsteams?  
Icon von Flaticon.com

[mehr dazu ...](https://docs.arc42.org/section-11/)

## 12. Glossar
![[12-glossary-overview.png]]
Wichtige Domänenbegriffe und technische Begriffe, die Stakeholder kennen sollten, wenn sie über die Architektur des Systems diskutieren. Manchmal auch Übersetzungstabellen, wenn in einer mehrsprachigen Umgebung gearbeitet wird.

[mehr dazu ...](https://docs.arc42.org/section-12/)

# Weitere Informationen[Permalink](https://arc42.de/overview/#weitere-informationen "Permalink")

So - jetzt kennen Sie die einzelnen Teile des Templates. Gerne können Sie noch weiterlesen:

Schauen Sie in unsere weiterführende Doku:

- Die ausführliche [Dokumentation des Templates](https://docs.arc42.org), geordnet nach den Sektionen des Templates. Inklusive über 100 Tipps zur Anwendung.
- FAQ - [Häufig gestellte Fragen)](https://faq.arc42.org)
- Unsere (neue) Seite über unser [Quality Model](https://quality.arc42.org).
- Einige der von uns geschriebenen [Bücher](https://arc42.de/books) beschäftigen sich mit arc42, beispielsweise:
    - [arc42 in Aktion](https://arc42.de/books/#arc42-in-aktion)
    - [arc42 by Example, Vol. 1](https://arc42.de/books#arc42-by-example), arc42 anhand von sechs praktischen Beispielen erklärt
    - [arc42 by Example, Vol. 2](https://arc42.de/books#arc42-by-example-vol2), Beispiele aus den Bereichen Echtzeit- und Embedded