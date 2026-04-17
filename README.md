# Car-Sales-Marktposition-Agent-Demo

## Hinweis:

Dieser Agent ist für die Demonstration von Microsoft Copilot Studio vorgesehen. Die hier bereitgestellten Daten können "wie sie sind" für Demos oder als Lernvorlag verwendet werden. Es gibt keine Garantie auf Funktion, Vollständigkeit oder Sicherheit. 

## Agent Beschreibung:

Dieser Agent erstellt einen ausführlichen Bericht über die Marktposition einzelner PKW Modelle in spezifischen Märkten. 

## Technische Beschreibung:

### Architektur

Das System besteht aus zwei Copilot Studio Agents und Power Automate Flows, die über Dataverse miteinander kommunizieren:

Benutzer
  │
  ▼
┌──────────────────────────┐
│  Orchestrator-Agent      │  (Sonnet 4.5)
│  - Validiert Eingaben    │
│  - Ermittelt Variante    │
│  - Identifiziert         │
│    Wettbewerber          │
│  - Konsolidiert Report   │
└────────┬─────────────────┘
         │ Topic: "StarteRecherche"
         ▼
┌──────────────────────────┐
│  Power Automate Flow     │
│  CarSalesMarktposition   │
│  - Liest Instruktionen   │
│    aus Dataverse         │
│  - Startet 5 parallele   │
│    Researcher-Aufrufe    │
│  - Wartet auf Abschluss  │
└────────┬─────────────────┘
         │ 5× parallel
         ▼
┌─────────────────────────┐
│  Researcher-Agent       │  (Sonnet 4.6)
│  - Führt Web-Recherche  │
│    pro Kriterium durch  │
│  - Schreibt Ergebnis    │
│    via Flow in Dataverse│
└─────────────────────────┘

### Ablauf

1. **Eingabe:** Der Benutzer gibt Fahrzeugmodell, Markt und optional Modelljahr an.
2. **Vorbereitung:** Der Orchestrator ermittelt die meistverkaufte Variante und 3–5 Wettbewerber.
3. **Delegation:** Ein Power Automate Flow liest die Recherche-Instruktionen aus Dataverse und startet 5 parallele Recherche-Aufträge über den Researcher-Agent.
4. **Recherche:** Jeder Researcher-Aufruf bewertet ein Kriterium (Preis-Leistung, Zuverlässigkeit, Sicherheit, Technologie, TCO) mit Web-Recherche und speichert das Ergebnis in Dataverse.
5. **Report:** Der Orchestrator liest die Ergebnisse aus Dataverse und erstellt einen konsolidierten Management-Report mit Scoring (0–100) pro Kriterium.

### Bewertungskriterien

| Nr | Kriterium                  |
|----|----------------------------|
| 1  | Preis-Leistungs-Verhältnis |
| 2  | Zuverlässigkeit & Qualität |
| 3  | Sicherheit                 |
| 4  | Technologie & Ausstattung  |
| 5  | Gesamtbetriebskosten (TCO) |

### Datenbank

Die Recherche-Instruktionen und Ergebnisse werden in zwei Dataverse-Tabellen gespeichert. Das vollständige Schema ist in [Dataverse-Schema.md](Dataverse-Schema.md) dokumentiert.

### Verwendete Komponenten

- **Microsoft Copilot Studio** – Orchestrator- und Researcher-Agent
- **Power Automate** – Flow-Steuerung und Dataverse-Interaktion
- **Microsoft Dataverse** – Persistierung von Instruktionen und Ergebnissen
- **Web-Browsing** – Beide Agents nutzen Web-Recherche für Quellenanalyse