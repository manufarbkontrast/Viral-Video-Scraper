# Feldstation — Kompletter Produktions-Workflow

## Überblick

Dieses System nimmt ein erfolgreiches YouTube-Video als Referenz und erstellt daraus ein **komplett neues, eigenständiges deutsches YouTube-Skript** mit fertigem Produktionsdokument.

```
YouTube-URL → Whisper-Transkript → Archive.org Grounding → Neues Skript → 3-Tab Produktion → PDF
```

---

## Schnellstart

```bash
# In Claude Code:
/produce https://www.youtube.com/watch?v=VIDEO_ID
```

Das war's. Der Rest läuft automatisch.

---

## Was passiert im Hintergrund

### Schritt 1: Audio + Transkript

Das Video wird mit `yt-dlp` als Audio heruntergeladen und mit **OpenAI Whisper** (lokal, medium-Modell) transkribiert.

```bash
yt-dlp -x --audio-format wav -o "transcripts/VIDEO_ID.wav" URL
whisper transcripts/VIDEO_ID.wav --model medium --language en --output_format txt
```

**Warum Whisper statt YouTube-Untertitel:**
- Korrekte Satzzeichen und Großschreibung
- Bessere Erkennung von Fachbegriffen (Artnamen, Epochen)
- Funktioniert auch wenn das Video keine Untertitel hat

### Schritt 2: Fakten-Grounding (Archive.org)

Das Thema wird auf Archive.org recherchiert. Ziel: echte, verifizierbare Fakten aus historischen Quellen einweben.

```
https://archive.org/advancedsearch.php?q=THEMA&output=json
```

Beispiele für eingewobene Quellen:
- Anderson & Udden 1905: Mammut-Fossilien in Illinois
- Hibben 1946: Folsom-Speerspitzen in New Mexico
- Lucas 1901: Der Begriff "Aussterben" existiert erst seit 1784

Die Quellen werden **natürlich in den Text eingewoben**, nicht als Fußnoten:
> "1905 grub ein Geologe bei Minooka in Illinois die Schädel von mindestens sechs Mammuts aus — an einer einzigen Stelle."

### Schritt 3: Neues Skript schreiben

Das Transkript dient **nur als Themen-Verständnis**. Das Skript wird komplett neu geschrieben — andere Struktur, anderer Hook, andere Narrative, eigene Beispiele.

**Zielformat:** ~2.000 Wörter (10-12 Minuten Sprechzeit)

### Schritt 4: 3-Tab Produktionsdokument

Output: Eine HTML-Datei mit drei Sektionen, jeweils mit Seitenumbruch.

### Schritt 5: PDF-Export

```bash
# Chrome headless konvertiert zu PDF:
Google Chrome --headless --print-to-pdf="output/pdf/NAME.pdf" file://PFAD/output/NAME.html
```

---

## Die 3 Tabs

### Tab 1: Translation (DE | EN)

```
Remodeling Video: https://www.youtube.com/watch?v=VIDEO_ID
Titel: Deutscher Videotitel hier
```

Dann eine zweispaltige Tabelle:

| Deutsch | English |
|---------|---------|
| Erster Absatz auf Deutsch... | First paragraph in English... |
| Zweiter Absatz... | Second paragraph... |

- Artnamen **fett** markiert
- Ein Absatz pro Tabellenzeile
- Für den Editor als Schnittvorlage + B-Roll-Suche

### Tab 2: VO-Script 11Labs

Deutsches Skript mit ElevenLabs Mood-Tags **vor jedem Satz**:

```
[dramatic] Stell dir vor, du stehst am Rand eines Abgrunds.
[calm] So tief, dass du den Grund nicht sehen kannst.
[soft voice] So tief, dass kein Sonnenstrahl dort unten ankommt.
[solemn] Das ist kein Gedankenexperiment.
[reverent] Dieser Abgrund existiert — und er ist voller Leben.
```

**Aufgeteilt in Teile unter 5.000 Zeichen** (ElevenLabs-Limit):
```
TEIL 1 von 3 — 4200 Zeichen
[Text...]
─────────────────────────
TEIL 2 von 3 — 4600 Zeichen
[Text...]
```

### Tab 3: Editor (EN)

Englischer Text in einzelnen Boxen. Eine Box = ein Absatz/Szene.

**Keine Regie-Anweisungen.** Kein "SCENE 1", kein "CUT TO", kein "[B-ROLL]". Nur reiner Sprechtext.

---

## Skript-Stilregeln

### Hook (max 50 Wörter)
- Erster Satz = filmisches Bild oder provokante Behauptung
- KEINE Begrüßung, KEIN "In diesem Video..."

### Ton
- Du-Form durchgehend
- Locker, gebildet, seriös-informativ
- Humor: parenthetisch und selbstironisch
- Wie ein echter Mensch, keine KI-Sprache

### Verbote
- ❌ Begrüßung/Willkommen
- ❌ CTAs (Like, Abo, Links)
- ❌ Clickbait ("Schnall dich an", "darfst du nicht verpassen")
- ❌ Leere Phrasen, Metaphern, Füllwörter
- ❌ Pathos, blumige Sprache
- ❌ "Stell dir vor" als Einstieg
- ❌ Editor-Anweisungen im Text

### Struktur
- Jede Zahl → greifbarer Alltagsvergleich ("4 Meter — länger als ein SUV")
- Spannung durch "Doch/Aber"-Eskalation
- Kurze Fragment-Sätze nach langen Erklärungen
- Fakten in Narration eingebettet, nie als Aufzählung
- Ende: Rückbezug auf Kernthese mit leichter Drehung

---

## ElevenLabs Mood-Tags

### Regeln
1. **Ein Tag pro Satz** (nicht pro Absatz)
2. **Nie zwei gleiche Tags hintereinander**
3. **40% leise Tags** für beste TTS-Qualität
4. **Dynamische Kontraste:** nach [intense] kommt [soft voice]

### Tag-Palette

| Kategorie | Tags |
|-----------|------|
| Ruhig/Sanft | `[calm]` `[soft voice]` `[gentle]` `[quiet]` `[low voice]` `[contemplative]` |
| Informativ | `[matter-of-fact]` `[explaining]` `[informative]` `[steady]` `[measured]` |
| Positiv | `[warm]` `[friendly]` `[bright]` `[wonder]` `[amazed]` `[soft wonder]` `[soft admiration]` |
| Ernst | `[solemn]` `[ominous]` `[grim]` `[somber]` `[softly grim]` `[gentle sadness]` |
| Spannung | `[dramatic]` `[building tension]` `[tense]` `[intense]` `[quiet determination]` `[quiet intensity]` |
| Neugier | `[curious]` `[intrigued]` `[engaged]` `[thoughtful]` `[reflective]` |
| Ehrfurcht | `[reverent]` `[awed]` `[quiet amazement]` |
| Betonung | `[focused]` `[soft emphasis]` `[soft but firm]` `[gentle tension]` |

---

## Ordnerstruktur

```
Youtube Scraper/
├── index.html              ← Dashboard (Feldstation)
├── output/
│   ├── *-production.html   ← Produktionsdokumente
│   └── pdf/
│       └── *.pdf           ← PDF-Exporte
├── transcripts/            ← Whisper-Transkripte (gitignored)
├── docs/
│   └── WORKFLOW.md         ← Diese Datei
└── .interface-design/
    └── system.md           ← Design-Token-System
```

---

## Dashboard (index.html)

Das Dashboard überwacht YouTube-Kanäle und findet virale Videos:

1. **Channels-Tab:** Überwachte Kanäle mit KPIs
2. **Viral Feed:** Videos über dem View-Threshold, mit Links
3. **Ideen-Tab:** Automatisch generierte Video-Ideen aus Trend-Analyse
4. **Skript-Pipeline:** Klick auf Idee → automatisch Transkript holen → Skript generieren → 3-Tab Ergebnis

### APIs im Dashboard
- **YouTube Data API** — Channel-Monitoring, Video-Stats (Multi-Key Rotation)
- **Claude/Anthropic API** — Skript-Generierung
- **ElevenLabs API** — Text-to-Speech direkt im Browser

---

## Referenz-Stil (Piranhas-Beispiel)

So soll ein fertiges Skript klingen:

> Lautlos schob sich eine riesige Würgeschlange durch den Fluss. Schwer. Ruhig. Vollkommen in ihrem Element.

> Dutzende. Dann Hunderte. Kein einzelner Gegner, sondern ein perfekt koordinierter Schwarm.

> Ein Fehler.

> Überleben kennt keine Moral.

---

## Produzierte Skripte

| Datei | Thema | Referenz-Video |
|-------|-------|----------------|
| `mammoth-steppe-production` | Mammutsteppe & was sie zerstörte | Wild Horizons |
| `mammoth-step-production` | Die Knochen unter Illinois | Wild Horizons |
| `megalodon-production` | Warum Megalodon ausstarb | 28M Views |
| `deep-sea-production` | Tiefsee-Kreaturen | 12.7M Views |
| `prehistoric-oceans-production` | Gruseligste Meere der Erdgeschichte | 4.7M Views |
| `terror-birds-production` | Terror Birds | 1.5M Views |
| `snowball-earth-production` | Als Vulkane die Erde einfroren | 2.2M Views |
