# Feldstation — Wildlife Channel Monitor + Ideen-Generator

## Was ist das?

Single-Page Dashboard (index.html) zur Überwachung von 54 Wildlife/Natur-YouTube-Kanälen. Erkennt virale Videos, analysiert Trends und generiert Video-Skripte mit ElevenLabs-Vertonung.

## Architektur

```
index.html  — Komplette App (HTML + CSS + JS, ~3.400 Zeilen)
                ├── YouTube Data API v3  → Channel-Daten + Viral Videos
                ├── Anthropic Claude API → Skript-Generierung
                └── ElevenLabs API       → Text-to-Speech Vertonung
```

Kein Backend. Alles client-side, localStorage für Persistenz.

## Kernkonzepte

### State Management
- Immutables State-Objekt: `appState = Object.freeze({...})`
- Updates nur über `updateState(patch)` → erzeugt neues gefrorenes Objekt
- Alle Daten-Records via `Object.freeze()` (Channels, Videos, Ideas, Scripts)

### Design Tokens
- Domain: "Feldstation" / Naturforschung
- Dark Theme mit benannten Farben: `--abyss`, `--deep`, `--shelf`, `--drift`, `--cavity`
- Signal-Farben: `--signal` (cyan), `--discovery` (amber), `--growth` (grün), `--decline` (rot), `--idea` (lila)
- Text: `--bone`, `--sediment`, `--fossil`
- Fonts: Inter (sans), JetBrains Mono (mono)
- Radius: 4px/6px (scharf, technisch)

### Tabs
1. **Channels** — Tabelle aller 54 Kanäle mit Sortierung, Subscribers, Avg Views, Trend
2. **Viral Feed** — Grid mit viralen Videos, Tier-System (mega 500K+ / hot 100K+ / warm 50K+)
3. **Ideen** — Automatisch generierte Video-Ideen basierend auf Trending-Analyse

### Ideen-Tab (Topic Analysis Engine)
- Extrahiert Keywords + Bigrams aus viralen Video-Titeln
- Stopword-Filter (DE + EN, ~80 Wörter)
- Clustert nach Häufigkeit und dedupliziert (>60% Overlap)
- Top 15 Ideen sortiert nach Score (Videoanzahl × 3 + Views/100K + Recency)
- Generiert Blickwinkel-Vorschläge pro Thema

### Skript-Generator
- Klick auf Ideen-Card → Modal mit Claude API Aufruf
- Modell: `claude-sonnet-4-20250514`, max_tokens: 16.000
- Ziel: 2.000 Wörter reiner Sprechtext
- Cache: localStorage, max 20 Skripte (LRU-Eviction)

### Skript-Stil (STRIKT)
Basiert auf Extinct Zoo / Eclesso Referenzkanälen:
- Du-Form, locker, gebildet, provokant, meinungsstark
- Humor: parenthetisch, selbstironisch, nie forciert
- Hook: max 50 Wörter, filmisch oder Alltagsabsurdität
- Struktur: "Aber"-Wendungen, Eskalation, Fragment-Sätze, Zahlen → Alltagsvergleiche
- Ende: Rückbezug auf Kernthese mit Drehung
- **Verboten:** Begrüßung, CTAs, Clickbait-Floskeln, Pathos, blumige Sprache, KI-Sprache

### Skript-Export Format
- **Übersichtstabelle:** Thema, Wörter, Dauer, Abschnitte, Quell-Videos, Views
- **Voiceover-Abschnitte:** Hook, Intro, Kapitel 1-3, Fazit — jeweils einzeln kopierbar mit Wortzahl + Dauer
- **Alles-kopieren** für Gesamtskript

### ElevenLabs Integration
- Modell: `eleven_multilingual_v2`
- Voice Settings: Stability 0.5, Similarity 0.75, Style 0.4, Speaker Boost on
- Tag-Palette (im Skript-Prompt eingebaut):
  - `GROSSBUCHSTABEN` → Betonung (sparsam, 2-3 pro Absatz)
  - `...` Ellipsen → natürliche Denkpausen
  - `<break time="1.0s" />` → nur zwischen Kapiteln (max 5, gekappt auf 1.5s)
- TTS-Vorbereitung: entfernt Kapitelüberschriften, Markdown, begrenzt Breaks
- Chunking bei >4.800 Zeichen (Split an Absatz/Satz-Grenzen)
- Audio: Player im Modal + MP3-Download

### Stimmen-Auswahl
- Stimmen werden live von ElevenLabs API geladen (Settings → "Laden")
- Ausgewählte Voice-ID in localStorage gespeichert

## API Keys (alle in localStorage)

| Key | Storage | Prefix |
|-----|---------|--------|
| YouTube Data API v3 | `feldstation_api_key` | `AIza...` |
| Anthropic Claude | `feldstation_anthropic_key` | `sk-ant-...` |
| ElevenLabs | `feldstation_elevenlabs_key` | `sk_...` oder `xi-...` |

Alle Keys werden direkt vom Browser an die jeweilige API gesendet (client-side). Nie im Code hardcoden.

## YouTube API Quota
- Tägliches Limit: 10.000 Units
- Channel-Resolve: 1 Unit, Video-Search: 100 Units, Video-Stats: 1 Unit/Batch
- Tracking in localStorage, wird im Footer angezeigt
- Cache-TTL: 4 Stunden

## Kanal-Liste
54 Wildlife-Kanäle hardcoded in `CHANNELS` Array (frozen). Enthält DE, EN, ES, IT Kanäle.

## XSS-Schutz
- `escapeHtml()` für alle innerHTML-Inserts
- `textContent` für AI-generierten Content (Skripte)
- Keine `eval()`, keine dynamischen Script-Tags

## File Structure
```
index.html                              — Die App
.interface-design/system.md             — Design-Token Dokumentation
CLAUDE.md                               — Diese Datei
Kopie von Claude.ai-Prompts-Scriptwriting.docx  — Referenz (nicht im Repo)
Viral Video Channel Scraper.json        — n8n Workflow (inaktiv, nicht im Repo)
```

## Regeln

- **Immutability:** IMMER neue Objekte erzeugen, nie mutieren
- **Design Tokens:** Keine hardcoded Farben, immer CSS-Variablen
- **Keine CTAs im Skript:** Kein "Like", "Abo", "Link in der Beschreibung"
- **Keine KI-Sprache:** Kein Pathos, keine Floskeln, kein generisches Dark-SaaS-UI
- **ElevenLabs-ready:** Skripte müssen direkt in TTS einspeisbar sein
- **Secrets:** Nie in Code, immer localStorage + Settings-Modal
