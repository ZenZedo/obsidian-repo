# Zusammenfassung: Das Entity-Relationship-Modell (ER-Modell)

Das **Entity-Relationship-Modell (ERM)** dient dem konzeptionellen Datenbankentwurf. Ziel ist es, einen Ausschnitt der realen Welt (Miniwelt) strukturiert, eindeutig und unabhängig von einem konkreten Datenbankmanagementsystem grafisch darzustellen. a

---

## 1. Bestandteile und grafische Formen (Chen-Notation)

In der klassischen Chen-Notation besitzt jedes Konstrukt eine festgelegte geometrische Form:

| Element | Grafische Form | Beschreibung & Beispiel |
| :--- | :--- | :--- |
| **Entitätstyp** | **Rechteck** | Repräsentiert eine Klasse gleichartiger Objekte der realen Welt (z. B. `Studenten`, `Vorlesungen`). |
| **Schwacher Entitätstyp** | **Doppelt umrandetes Rechteck** | Existenzabhängiges Objekt ohne eigenen global eindeutigen Schlüssel (z. B. `Räume` in einem Gebäude). |
| **Attribut** | **Oval** | Beschreibt eine Eigenschaft einer Entität oder einer Beziehung (z. B. `Name`, `Note`, `SWS`). |
| **Schlüsselattribut** | **Oval (Text unterstrichen)** | Identifiziert jede Ausprägung einer Entität eindeutig (z. B. $\underline{\text{MatrNr}}$). |
| **Teilschlüssel (Diskriminator)** | **Oval (Text gestrichelt unterstrichen)** | Identifiziert eine schwache Entität *innerhalb* der übergeordneten starken Entität (z. B. $\text{RaumNr}$). |
| **Beziehungstyp (Relationship)** | **Raute** | Stellt eine logische Verknüpfung zwischen Entitätstypen dar (z. B. `hören`, `lesen`, `prüfen`). |
| **Identifizierende Beziehung** | **Doppelt umrandete Raute** | Verknüpft eine schwache Entität mit ihrem übergeordneten starken Entitätstyp (z. B. `liegt_in`). |
| **Generalisierung / Spezialisierung** | **Dreieck** (oder Sechseck mit `is-a`) | Stellt Vererbungsbeziehungen dar (z. B. `Professoren` *ist ein* `Angestellter`). |

---

## 2. Zentrale Begriffe und Konzepte

### Stelligkeit (Grad) einer Beziehung
Die Stelligkeit gibt an, wie viele Entitätstypen an einer Beziehung (Raute) beteiligt sind:
* **Unär (1-stellig / rekursiv):** Ein Entitätstyp steht in Beziehung zu sich selbst (z. B. eine Vorlesung *setzt voraus* eine andere Vorlesung). 
  * *Wichtig:* Hier müssen die Kanten zwingend mit **Rollennamen** beschriftet werden (z. B. `Vorgänger` und `Nachfolger`), um die Richtung der Beziehung klarzustellen.
* **Binär (2-stellig):** Der Standardfall zwischen zwei verschiedenen Entitätstypen (z. B. `Studenten` *hören* `Vorlesungen`).
* **Ternär (3-stellig) / $n$-stellig:** Drei (oder mehr) Entitätstypen sind gleichzeitig an einer einzigen Beziehung beteiligt (z. B. `Studenten`, `Vorlesungen` und `Professoren` sind an der Beziehung `prüfen` beteiligt).

### Funktionalität (1:1, 1:N, N:M)
Die Funktionalität beschreibt die maximale Anzahl von Beziehungspartnern auf abstrakter Ebene:
* **1:1:** Jede Entität aus $A$ ist maximal mit einer Entität aus $B$ verknüpft und umgekehrt (z. B. `Professor` *leitet* `Fachgebiet`).
* **1:N:** Eine Entität aus $A$ kann mit beliebig vielen ($N$) Entitäten aus $B$ verknüpft sein, aber jede Entität aus $B$ gehört zu maximal einer ($1$) Entität aus $A$ (z. B. ein `Professor` liest $N$ `Vorlesungen`, jede `Vorlesung` wird von genau $1$ `Professor` gelesen).
* **N:M:** Beliebig viele Entitäten aus $A$ können mit beliebig vielen Entitäten aus $B$ verknüpft sein (z. B. `Studenten` *hören* `Vorlesungen`).
* *Besonderheit bei $n$-stelligen Beziehungen:* Steht bei einer ternären Beziehung an einer Entität $E_3$ eine `1` (z. B. $N:M:1$), bedeutet dies: Eine Kombination aus $E_1$ und $E_2$ bestimmt $E_3$ eindeutig.

### Min-Max-Notation
Die `(min, max)`-Notation ist präziser als die reine Funktionalitätsangabe (`1:N`). Sie steht direkt an der Kante zwischen Entität $E$ und Beziehung $R$ und gibt an, an **wie vielen Beziehungen dieses Typs eine einzelne Ausprägung von $E$ mindestens ($\min$) und höchstens ($\max$) teilnehmen muss bzw. darf**:
* $\min = 0$: Optionale Teilnahme (kann teilnehmen, muss aber nicht).
* $\min = 1$ (oder höher): Verpflichtende Teilnahme (totale Partizipation).
* $\max = 1$: Höchstens eine Teilnahme.
* $\max = *$ (oder $N$): Beliebig viele Teilnahmen.
* *Achtung Leserichtung:* Während `1:N` oft über Kreuz gelesen wird, steht `(min, max)` **direkt bei der Entität**, für die die Aussage gilt (z. B. `Student` — `(0,*)` — `hören` — `(0,*)` — `Vorlesung`).

### Schwache Entities (Weak Entities)
Schwache Entitäten sind Objekte, die **existenzabhängig** von einer anderen („starken“) Entität sind und keinen eigenen, global eindeutigen Primärschlüssel besitzen.
* **Beispiel:** Ein `Raum` hat eine `RaumNr` (z. B. `101`). Diese Nummer gibt es aber in Gebäude A und in Gebäude B. Der Raum kann ohne das Gebäude nicht existieren und nicht eindeutig identifiziert werden.
* **Merkmale:**
  * Doppelt umrandetes Rechteck, verbunden über eine doppelt umrandete Raute (`1:N`-Beziehung zum starken Typ).
  * Besitzt einen **Teilschlüssel** (gestrichelt unterstrichen).
  * Der vollständige Primärschlüssel setzt sich später aus dem **Primärschlüssel der starken Entität + dem Teilschlüssel** zusammen (z. B. `{GebNr, RaumNr}`).

### Generalisierung und Spezialisierung (`is-a`)
Dieses Konzept stammt aus der Objektorientierung und beschreibt Vererbungshierarchien:
* **Generalisierung (Bottom-Up):** Gemeinsame Attribute mehrerer Entitätstypen werden in einem übergeordneten Typ (Superklasse) zusammengefasst (z. B. `Professoren` und `Assistenten` haben beide `PersNr` und `Name` $\rightarrow$ Obertyp `Angestellte`).
* **Spezialisierung (Top-Down):** Ein Obertyp wird in spezialisierte Untertypen mit eigenen, spezifischen Attributen aufgeteilt (z. B. `Professoren` erhalten zusätzlich `Rang` und `Raum`).
* **Vererbung:** Untertypen erben automatisch alle Attribute sowie den **Primärschlüssel** des Obertyps.

### Aggregation
In der konzeptionellen Datenmodellierung (insbesondere im erweiterten ER-Modell / EER und UML) gibt es zwei Bedeutungen:
1. **Teil-Ganzes-Beziehung (Part-of):** Ein Objekt setzt sich aus mehreren Einzelteilen zusammen (z. B. ein `Auto` besteht aus `Motor`, `Karosserie` und `Rädern`). Im Gegensatz zur Komposition können die Teile oft auch eigenständig existieren.
2. **Beziehungs-Aggregation:** Eine gesamte Beziehung (inklusive der beteiligten Entitäten) wird gedanklich zu einer abstrakten übergeordneten Entität zusammengefasst, um sie mit einer weiteren Entität in Beziehung zu setzen (falls man eine ternäre Beziehung vermeiden möchte).

---

## 3. Leitfaden: Wie man ein gutes ER-Diagramm erstellt & typische Fehler vermeidet

Ein sauberes ER-Modell ist die halbe Miete für ein fehlerfreies relationales Datenbankschema. Achte beim Entwurf auf folgende Regeln:

### Schritt-für-Schritt-Vorgehen
1. **Substantive identifizieren:** Suche im Aufgabentext nach zentralen Objekten $\rightarrow$ Kandidaten für **Entitätstypen**.
2. **Eigenschaften zuordnen:** Suche nach beschreibenden Merkmalen dieser Objekte $\rightarrow$ **Attribute** (und lege sofort einen eindeutigen **Primärschlüssel** fest).
3. **Verben identifizieren:** Suche nach Handlungen oder Zusammenhängen zwischen den Objekten $\rightarrow$ **Beziehungstypen**.
4. **Beziehungsattribute prüfen:** Gehört ein Attribut wirklich zu einer Entität oder entsteht es erst durch die Verknüpfung? (Beispiel: Eine `Note` gehört weder dem Studenten noch der Vorlesung allein, sondern zur Beziehung `prüfen`).
5. **Kardinalitäten bestimmen:** Prüfe für jede Kante die `(min, max)`-Werte oder `1:N`-Funktionalitäten anhand der fachlichen Vorgaben.

### Die 6 häufigsten Fehler (und wie man sie vermeidet)

1. **Primärschlüssel vergessen oder nicht unterstreichen:**
   * *Fehler:* Entitäten ohne unterstrichenes Schlüsselattribut modellieren.
   * *Lösung:* Jede starke Entität **muss** mindestens ein unterstrichenes Attribut haben. Gibt es im Text keines (z. B. bei Personen mit gleichem Namen), führe künstliche IDs wie `KundenNr` oder `ID` ein.
2. **Connection Trap (Verbindungsfalle / Redundante Zyklen):**
   * *Fehler:* Ein Dreieck aus Beziehungen modellieren, obwohl sich eine der Beziehungen logisch aus den anderen beiden herleiten lässt (z. B. `Student` besucht `Studiengang`, `Studiengang` gehört zu `Fakultät` – eine zusätzliche direkte Beziehung `Student` gehört zu `Fakultät` ist oft redundant und führt zu Inkonsistenzen).
   * *Lösung:* Prüfe bei jedem geschlossenen Kreis im Diagramm, ob jede Kante wirklich neue, nicht ableitbare Informationen liefert.
3. **Entität vs. Attribut verwechseln:**
   * *Fehler:* Ein komplexes Objekt mit eigenen Eigenschaften als simples Attribut modellieren (z. B. `Verlag` als Text-Attribut bei `Buch`, obwohl auch Adresse und Gründungsjahr des Verlags gespeichert werden sollen).
   * *Lösung:* Sobald ein Merkmal selbst mehrere Eigenschaften hat oder mehrfach vorkommt, mache einen eigenen **Entitätstyp** daraus.
4. **Fremdschlüssel im ER-Diagramm einzeichnen:**
   * *Fehler:* Attribute wie `ProfPersNr` manuell in die Entität `Vorlesungen` schreiben.
   * *Lösung:* Im ER-Modell gibt es **keine Fremdschlüssel**! Verbindungen werden ausschließlich über Rauten (Beziehungen) dargestellt. Fremdschlüssel entstehen erst später bei der Transformation in Tabellen.
5. **Fehlende Rollennamen bei rekursiven Beziehungen:**
   * *Fehler:* Eine Raute von `Mitarbeiter` zurück zu `Mitarbeiter` ohne Beschriftung zeichnen.
   * *Lösung:* Immer die Rollen an beide Linien schreiben (z. B. `Vorgesetzter` und `Untergebener`), sonst ist unklar, wer wen leitet.
6. **Mehrwertige Attribute (Listen) verwenden:**
   * *Fehler:* Ein Attribut `Telefonnummern` anlegen, in dem mehrere Nummern gleichzeitig stehen sollen.
   * *Lösung:* Entweder als mehrwertiges Attribut (doppelt umrandetes Oval) kennzeichnen oder sauberer direkt als eigenen schwachen/starken Entitätstyp modellieren, um die 1. Normalform vorzubereiten.