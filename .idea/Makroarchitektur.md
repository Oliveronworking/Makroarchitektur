# Makroarchitektur Teil 1

## A) Theorie

### Was ist Softwarearchitektur?
* **Definition**: Die Softwarearchitektur ist die grundlegende Struktur eines Systems.
* **Inhalt**: Sie beschreibt die Aufteilung in Komponenten und deren Zusammenspiel.
* **Zweck**: Sie stellt sicher, dass Qualitätsmerkmale wie Wartbarkeit und Erweiterbarkeit eingehalten werden.

### Dokumentation von Softwarearchitektur
* **Methoden**: Dokumentation erfolgt über Texte, Codeauszüge und Diagramme.
* **C4-Modell**: Ein System zur hierarchischen Darstellung (Context, Container, Component, Code).
* **arc42**: Ein Standard-Template zur strukturierten Ablage von Architektur-Entscheidungen.

### Eigenschaften langlebiger Softwarearchitekturen (Lilienthal)
* **Modularität**: Das System besteht aus unabhängigen, austauschbaren Bausteinen.
* **Strukturierung**: Es gibt klare Hierarchien ohne zyklische Abhängigkeiten.
* **Muster**: Konsequente Nutzung bewährter Entwurfsmuster zur Komplexitätsreduktion.

### Was ist ein Modulith?
* **Konzept**: Eine Anwendung, die als ein Ganzes (Monolith) ausgeliefert wird, intern aber streng modular aufgebaut ist.
* **Vorteil**: Einfaches Deployment bei gleichzeitig hoher Ordnung im Code.

### Ports and Adapters Architektur (Hexagonale Architektur)
* **Kern**: Die Geschäftslogik (Domain) ist völlig unabhängig von externen Technologien.
* **Ports**: Definierte Schnittstellen für den Ein- und Ausgang von Daten.
* **Adapter**: Verbinden die Außenwelt (DB, Web-UI) mit den Ports der Logik.

### DDD: Taktische Pattern (Modellgetriebener Entwurf)
* **Entity**: Ein Objekt mit einer dauerhaften Identität (z.B. eine ID).
* **Value Object**: Ein Objekt, das nur durch seine Werte definiert ist (z.B. eine Währung).
* **Aggregate**: Eine Gruppe von zusammengehörigen Objekten, die nach außen als Einheit auftreten.
* **Repository**: Ein Dienst, um Aggregate zu speichern und wieder abzurufen.

---

## B) Architekturanalyse erplite-Backend

### A1) Ordermanagement (Ports and Adapters & DDD)

#### Analyse der Anwendungsfälle
1. **Bestellung aufgeben**:
    * Die Logik prüft die Daten und erstellt eine Order-Entity.
    * Ein Repository-Port wird genutzt, um die Bestellung dauerhaft zu sichern.
2. **Bestellung auf bezahlt setzen**:
    * Das Order-Aggregate ändert intern seinen Status.

#### Architektur-Diagramm
```mermaid
graph LR
    subgraph Adapter_In
    OC[OrderController]
    end
    
    subgraph Domain_Logic
    OS[OrderService]
    OA[Order Aggregate]
    end
    
    subgraph Adapter_Out
    OR[OrderRepository]
    DB[(Datenbank)]
    end

    OC --> OS
    OS --> OA
    OS --> OR
    OR --> DB