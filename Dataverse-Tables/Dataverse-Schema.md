# Dataverse Table Schemas

Publisher-Prefix: `cr1d7_`

## Tabelle 1: `cr1d7_rechercheinstruktionens`

Enthält die Recherche-Kriterien und Instruktionen für den Researcher-Agent.

| Spalte (logisch) | Anzeigename | Typ | Beschreibung |
|---|---|---|---|
| `cr1d7_name` | KriteriumName | String | Name des Kriteriums (Primary Name) |
| `cr1d7_kriteriumnr` | KriteriumNr | Number | Kriterium-Nummer (1–5) |
| `cr1d7_allgemeinerichtlinien` | AllgemeineRichtlinien | String | Allgemeine Recherche-Richtlinien |
| `cr1d7_instruktionen` | Instruktionen | String | Detaillierte Instruktionen pro Kriterium |

### Bekannte Datensätze

| Nr | KriteriumName |
|----|---------------|
| 1 | Preis-Leistungs-Verhältnis |
| 2 | Zuverlässigkeit-und-Qualität |
| 3 | Sicherheit |
| 4 | Technologie-und-Ausstattung |
| 5 | Gesamtbetriebskosten-TCO |

---

## Tabelle 2: `cr1d7_rechercheergebnisses`

Speichert die Recherche-Ergebnisse pro Kriterium und Conversation.

| Spalte (logisch) | Anzeigename | Typ | Beschreibung |
|---|---|---|---|
| `cr1d7_rechercheergebnisseid` | RechercheErgebnisse | String (PK) | Primärschlüssel |
| `cr1d7_cr_conversationid` | cr_conversationid | String | Verknüpfung zur Conversation-Session |
| `cr1d7_cr_kriteriumname` | cr_kriteriumname | String | Name des Kriteriums |
| `cr1d7_cr_kriteriumnr` | cr_kriteriumnr | Number | Kriterium-Nummer (1–5) |
| `cr1d7_cr_ergebnis100k` | cr_ergebnis100k | String | Recherche-Ergebnis (100k Zeichen). Wird vom Researcher geschrieben und vom Orchestrator gelesen. |
| `cr1d7_cr_status` | cr_status | Choice | `519970000` = ausstehend, `519970001` = abgeschlossen |

---

## Datenfluss zwischen den Tabellen

```
RechercheInstruktionens                RechercheErgebnisses
──────────────────────                 ────────────────────
cr1d7_name             ──────────►    cr1d7_cr_kriteriumname
cr1d7_kriteriumnr      ──────────►    cr1d7_cr_kriteriumnr
cr1d7_allgemeinerichtlinien ──► Prompt
cr1d7_instruktionen    ──────► Prompt
                                      cr1d7_cr_conversationid  ◄── System.Conversation.Id
                                      cr1d7_cr_status          ◄── pending → completed
                                      cr1d7_cr_ergebnis100k    ◄── Researcher schreibt
                                      cr1d7_cr_ergebnis100k    ──► Orchestrator liest für Report
```
