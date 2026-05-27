# 🎣 BrixxFishing — Einrichtungsanleitung

> **Copyright © 2025 Hector Gonzalez Lopez. Alle Rechte vorbehalten.**  
> Dieses Plugin ist urheberrechtlich geschützt. Weitergabe, Kopie oder Modifikation ohne ausdrückliche Genehmigung des Autors ist untersagt.

---

## Inhaltsverzeichnis
1. [Voraussetzungen](#1-voraussetzungen)
2. [Installation](#2-installation)
3. [config.yml – Hauptkonfiguration](#3-configyml--hauptkonfiguration)
4. [fish.yml – Fische konfigurieren](#4-fishyml--fische-konfigurieren)
5. [events.yml – Events konfigurieren](#5-eventsyml--events-konfigurieren)
6. [Spracheinstellungen](#6-spracheinstellungen)
7. [Resource Pack (Texturen)](#7-resource-pack-texturen)
8. [Befehle & Berechtigungen](#8-befehle--berechtigungen)
9. [PlaceholderAPI](#9-placeholderapi)
10. [Häufige Probleme](#10-häufige-probleme)

---

## 1. Voraussetzungen

| Anforderung | Version | Pflicht |
|---|---|---|
| Paper / Spigot | 1.20.4+ | ✅ Ja |
| Java | 17+ | ✅ Ja |
| Vault | beliebig | ✅ Ja (für Economy) |
| VMultiEconomy / anderes Economy-Plugin | 1.5.7+ | ✅ Ja (für Economy) |
| PlaceholderAPI | 2.11+ | ❌ Optional |

> ⚠️ **Wichtig:** Vault **und** das Economy-Plugin (z.B. VMultiEconomy) müssen **vor** BrixxFishing geladen werden. Das Plugin erledigt das automatisch über `softdepend` — stelle sicher, dass alle JARs im `/plugins` Ordner liegen.

---

## 2. Installation

1. Lade folgende Plugins herunter und lege sie in den `/plugins` Ordner:
   - `Vault.jar`
   - `VMultiEconomy-x.x.x.jar` (oder ein anderes Vault-kompatibles Economy-Plugin)
   - `BrixxFishing.jar`

2. Starte den Server einmal → Konfigurationsdateien werden automatisch erstellt.

3. Passe die Konfiguration an (siehe unten).

4. Lade das Plugin neu: `/brixxfish admin reload`

---

## 3. config.yml – Hauptkonfiguration

Pfad: `plugins/BrixxFishing/config.yml`

### Sprache
```yaml
language: de        # Sprache: de (Deutsch), en (Englisch), tr (Türkisch)
```

### Minigame
```yaml
minigame:
  bar-width: 20           # Breite der Angel-Leiste (Positionen)
  cursor-speed: 2         # Ticks zwischen Cursor-Bewegungen (niedriger = schneller = schwieriger)
  green-zone-width: 5     # Breite der grünen Zone (Standard ohne Upgrades)
  bite-timeout: 8         # Sekunden bis der Fisch weg ist (wenn nicht eingeholt)
  timeout-enabled: true   # false = kein Timeout (immer Zeit zum Einholen)
  critical-size: 1        # Positionen um die Mitte die als KRITISCH gelten
  difficulty-by-rarity: true  # Seltenere Fische = schnellerer Cursor
  rarity-speed-bonus:
    uncommon: 0    # Geschwindigkeits-Bonus pro Seltenheit (ticks werden abgezogen)
    rare: 1
    epic: 2
    legendary: 3
  zone-moves: false        # Grüne Zone bewegt sich hin und her
  zone-move-speed: 5       # Ticks zwischen Zone-Bewegungsschritten
```

### Level-System
```yaml
levels:
  base-xp: 100          # XP für Level 1 → 2
  xp-multiplier: 1.15   # XP-Multiplikator pro Level (exponentiell)
  max-level: 0          # Maximales Level (0 = unbegrenzt)
```

### Fisch-Wahrscheinlichkeiten
```yaml
fish-chances:
  common: 50            # % Grundchance für Gewöhnlich
  uncommon: 25          # % Grundchance für Ungewöhnlich
  rare: 15              # % Grundchance für Selten
  epic: 7               # % Grundchance für Episch
  legendary: 3          # % Grundchance für Legendär
  epic-bonus-per-level: 0.1       # +% Epic-Chance pro Level
  legendary-bonus-per-level: 0.05 # +% Legendary-Chance pro Level
```

### Schatztruhen
```yaml
treasure:
  base-chance: 3.0        # % Grundchance auf Schatz statt Fisch
  required-rounds: 3      # Erfolgreiche Minigame-Runden für Schatz-Öffnung
  rewards:                # Mögliche Belohnungen (zufällig ausgewählt)
    - "money:50-200"      # Geld: min-max
    - "DIAMOND:1-3"       # Item: Minecraft-Material-Name:min-max
    - "EMERALD:2-5"
    - "GOLD_INGOT:3-8"
    - "ENCHANTED_BOOK:1"
```

### Zonen-Effizienz (Überfischung)
```yaml
zone-efficiency:
  enabled: true             # Feature aktivieren/deaktivieren
  threshold: 10             # Ab dieser Anzahl Fänge in einer Zone: Warnung
  decrease-per-catch: 2.0   # % Effizienz-Verlust pro Fang in der Zone
  min-efficiency: 10.0      # Minimale Effizienz (%)
  recovery-per-minute: 1.0  # % Effizienz-Erholung pro Minute
  zone-radius: 32           # Radius in Blöcken für eine "Zone"
```

### Economy / Wirtschaft
```yaml
economy:
  enabled: true               # Economy-System aktivieren
  sell-tax: 0.0               # Steuer in % auf Fischverkäufe (0.0 = keine Steuer)
  auto-sell-on-catch: false   # Fisch direkt nach dem Fang verkaufen (kein Item ins Inventar)
  auto-sell-multiplier: 1.0   # Multiplikator für Auto-Sell-Preis
```

### Upgrade-Kosten
```yaml
upgrades:
  slow-cursor:          # Langsamerer Cursor
    max-level: 5
    costs: [100, 250, 500, 1000, 2000]   # Kosten pro Level
  wider-zone:           # Breitere grüne Zone
    max-level: 5
    costs: [150, 350, 700, 1400, 2800]
  treasure-luck:        # Höhere Schatz-Chance
    max-level: 3
    costs: [500, 1500, 4000]
  catch-efficiency:     # Mehr XP pro Fang
    max-level: 5
    costs: [200, 500, 1000, 2000, 4000]
```

### Sounds
```yaml
sounds:
  bite: ENTITY_EXPERIENCE_ORB_PICKUP       # Fisch beißt an
  success: ENTITY_PLAYER_LEVELUP           # Erfolgreicher Fang
  fail: ENTITY_VILLAGER_NO                 # Fehlgeschlagen
  critical: ENTITY_LIGHTNING_BOLT_THUNDER  # Kritischer Treffer
  treasure-opened: UI_TOAST_CHALLENGE_COMPLETE
  level-up: ENTITY_PLAYER_LEVELUP
```

> Alle Sound-Namen: [Minecraft Sound Events](https://minecraft.wiki/w/Sounds.json#Java_Edition_values)

---

## 4. fish.yml – Fische konfigurieren

Pfad: `plugins/BrixxFishing/fish.yml`

### Beispiel-Eintrag
```yaml
fish:
  karpfen:                          # Eindeutige ID (lowercase, keine Leerzeichen)
    display-name: "Karpfen"         # Angezeigter Name
    rarity: COMMON                  # COMMON | UNCOMMON | RARE | EPIC | LEGENDARY
    min-weight: 0.5                 # Mindestgewicht in kg
    max-weight: 4.0                 # Maximalgewicht in kg
    base-xp: 7                      # XP pro Fang (vor Multiplikatoren)
    sell-value: 2.5                 # Verkaufswert pro kg (in Währung)
    icon: COD                       # Minecraft-Material für das Item-Icon
    custom-model-data: 10001        # Custom Model Data für Resource Pack Textur (0 = keine)
    description: "Ein gewöhnlicher Flussfisch."
    condition: null                 # null = immer | "night" = nur nachts fangbar
```

### Seltenheiten & Multiplikatoren
| Seltenheit | XP-Multiplikator | Standard-Farbe |
|---|---|---|
| COMMON | ×1.0 | Weiß |
| UNCOMMON | ×1.5 | Grün |
| RARE | ×2.5 | Cyan |
| EPIC | ×5.0 | Lila |
| LEGENDARY | ×10.0 | Gold |

### Eigene Fische hinzufügen
1. Neuen Eintrag in `fish.yml` erstellen (unique ID)
2. `icon` auf ein gültiges [Minecraft Material](https://jd.papermc.io/paper/1.20/) setzen
3. Bei custom Texturen: `custom-model-data` mit dem Wert aus dem Resource Pack setzen
4. Plugin neu laden: `/brixxfish admin reload`

---

## 5. events.yml – Events konfigurieren

Pfad: `plugins/BrixxFishing/events.yml`

### Beispiel-Eintrag
```yaml
double-payout:
  display-name: "&6&l✦ DOPPELTE AUSZAHLUNG ✦"   # Farb-Codes mit & möglich
  description: "Alle Fische werden doppelt bezahlt"
  sell-multiplier: 2.0          # Verkaufswert ×2
  xp-multiplier: 1.0            # XP-Multiplikator
  luck-multiplier: 1.0          # Seltenheits-Multiplikator
  weight-multiplier: 1.0        # Gewichts-Multiplikator
  treasure-multiplier: 1.0      # Schatz-Multiplikator
  no-timeout: false             # true = kein Timeout im Minigame
  duration: 600                 # Dauer in Sekunden (0 = unbegrenzt)
```

### Events starten/stoppen
```
/brixxfish admin event start double-payout          # Global starten
/brixxfish admin event start double-payout world    # Nur in Welt "world"
/brixxfish admin event stop double-payout           # Stoppen
/brixxfish admin event stopall                      # Alle Events stoppen
/brixxfish admin event list                         # Aktive Events anzeigen
```

> **BossBar:** Beim Start eines Events erscheint automatisch eine **hell-blaue BossBar** mit gold-fettem Text. Der Fortschrittsbalken zählt die verbleibende Zeit herunter. Nach Ablauf endet das Event automatisch.

---

## 6. Spracheinstellungen

Pfad: `plugins/BrixxFishing/messages/messages_de.yml`

Verfügbare Sprachen:
- `de` → Deutsch (`messages_de.yml`)
- `en` → Englisch (`messages_en.yml`)
- `tr` → Türkisch (`messages_tr.yml`)

Sprache ändern in `config.yml`:
```yaml
language: de
```
Dann neu laden: `/brixxfish admin reload`

Die Nachrichten können frei angepasst werden. `&`-Farbcodes werden unterstützt.  
Platzhalter wie `{level}`, `{xp}`, `{fish}` müssen erhalten bleiben.

---

## 7. Resource Pack (Texturen)

Das Plugin unterstützt eigene Texturen über ein Minecraft Resource Pack mit **Custom Model Data**.

### Einrichtung
1. Trage URL und SHA-1-Hash des Resource Packs in `config.yml` ein:
```yaml
resource-pack:
  url: "https://example.com/dein-resourcepack.zip"
  hash: "a1b2c3d4e5f6..."    # SHA-1 Hash (40 Zeichen, Groß- oder Kleinbuchstaben)
```

2. Das Resource Pack wird Spielern beim Login automatisch geschickt.

3. Weise jedem Fisch in `fish.yml` den passenden `custom-model-data`-Wert zu (muss mit dem Resource Pack übereinstimmen):
```yaml
  karpfen:
    icon: COD
    custom-model-data: 10001    # Dieser Wert muss im Resource Pack definiert sein
```

> Der **Fish Catalog** (GUI) zeigt die Texturen korrekt an, sobald `custom-model-data` gesetzt ist und das Resource Pack geladen wurde.

---

## 8. Befehle & Berechtigungen

### Spieler-Befehle
- `/brixxfish` — Öffnet das Hauptmenü. Hier siehst du den aktuellen Status, Zugang zu Upgrades und den Fisch-Katalog.
- `/brixxfish sell` — Öffnet die Verkaufs-GUI, um deine gefangenen Fische zu verkaufen.
- `/brixxfish catalog` — Öffnet den Fisch-Katalog mit allen verfügbaren Fischen und ihren Werten.
- `/brixxfish upgrades` — Öffnet das Upgrade-Menü, in dem du deine Angel verbessern kannst.
- `/brixxfish stats` — Zeigt deine persönlichen Angel-Statistiken an (XP, Level, gefangene Fische usw.).
- `/brixxfish top` — Zeigt die Top-Angler im Server an.

**Alias:** `/fish`, `/fishing` funktionieren ebenfalls als Kurzform.

### Admin-Befehle
- `/brixxfish admin` — Zeigt die Admin-Hilfe mit allen verfügbaren Admin-Unterbefehlen.
- `/brixxfish admin reload` — Lädt das Plugin neu und liest alle Konfigurationsdateien erneut ein.
- `/brixxfish admin setlevel <Spieler> <Level>` — Setzt das Angel-Level eines Spielers manuell.
- `/brixxfish admin give <Spieler> <FischID> [Menge]` — Gibt einem Spieler einen oder mehrere Fische. Beispiel: `/brixxfish admin give Nikita karpfen 3`
- `/brixxfish admin give rod <Typ> <Spieler>` — Gibt einem Spieler eine custom Angeln. Verfügbare Typen: `wood`, `gold`, `standard`, `premium`, `metal`.
- `/brixxfish admin givemoney <Spieler> <Betrag>` — Gibt dem Spieler Geld in der Economy.
- `/brixxfish admin event start <ID> [Welt]` — Startet ein Event global oder nur in der angegebenen Welt.
- `/brixxfish admin event stop <ID>` — Stoppt ein bestimmtes Event.
- `/brixxfish admin event stopall` — Stoppt alle laufenden Events.
- `/brixxfish admin event list` — Zeigt alle aktuell aktiven Events an.

### Erklärung weiterer Admin-Befehle
- `/brixxfish admin reload`
  - Nutze diesen Befehl nach Änderungen an `config.yml`, `fish.yml`, `events.yml` oder den Sprachdateien.
- `/brixxfish admin setlevel`
  - Setzt das Spieler-Level direkt, falls du XP oder Level manuell korrigieren möchtest.
- `/brixxfish admin give rod <Typ> <Spieler>`
  - Gibt eine Rute mit passender `custom_model_data`, so dass dein Resource Pack die richtige Angel-Textur darstellt.
- `/brixxfish admin event start <ID> [Welt]`
  - Beispiel: `/brixxfish admin event start double-payout` startet ein Event auf dem gesamten Server.
  - Beispiel: `/brixxfish admin event start double-payout world_nether` startet das Event nur in der Welt `world_nether`.
- `/brixxfish admin event stopall`
  - Nutze das, wenn mehrere Events gleichzeitig aktiv sind und du schnell alles beenden willst.

### Berechtigungen-Übersicht
- `brixxfishing.use` — Ermöglicht normalen Spielern den Zugriff auf alle Spieler-Befehle und GUI-Funktionen.
- `brixxfishing.admin` — Erlaubt den Zugriff auf die Admin-Hilfe und einige Admin-Unterbefehle.
- `brixxfishing.admin.reload` — Erlaubt das Neuladen des Plugins, ohne OP zu sein.
- `brixxfishing.admin.setlevel` — Erlaubt das Setzen von Spieler-Leveln.
- `brixxfishing.admin.give` — Erlaubt das Geben von Items, Fischen und Geld.

> **Tipp:** Vergib Admin-Rechte am besten nur an vertrauenswürdige Moderatoren. Bei Permissions-Plugins wie LuckPerms kannst du fein steuern, wer Reloads oder Give-Befehle verwenden darf.

---

## 9. PlaceholderAPI

Falls PlaceholderAPI installiert ist, stehen folgende Platzhalter zur Verfügung:

| Platzhalter | Beschreibung |
|---|---|
| `%brixxfishing_level%` | Level des Spielers |
| `%brixxfishing_xp%` | Aktuelle XP |
| `%brixxfishing_heaviest_caught%` | Schwerster Fang |
| `%brixxfishing_top_level_1_name%` | Name des #1-Anglers |
| `%brixxfishing_top_level_1_level%` | Level des #1-Anglers |
| `%brixxfishing_top_level_2_name%` | Name des #2-Anglers |
| `%brixxfishing_top_level_N_name%` | bis N=10 |

---

## 10. Häufige Probleme

### Economy funktioniert nicht / Vault hook fehlgeschlagen
- **Ursache:** VMultiEconomy (oder anderes Economy-Plugin) lädt nicht vor BrixxFishing
- **Lösung:** Stelle sicher, dass `Vault.jar` **und** das Economy-Plugin im `/plugins` Ordner sind. Server neu starten (nicht nur reload).
- **Check:** In der Konsole beim Start sollte stehen: `[BrixxFishing] Vault economy hooked: VMultiEconomy`

### Texturen werden im Fish Catalog nicht angezeigt
- **Ursache:** `custom-model-data` in `fish.yml` fehlt oder das Resource Pack ist nicht geladen
- **Lösung 1:** `custom-model-data` in `fish.yml` für jeden Fisch setzen
- **Lösung 2:** Korrekte URL und SHA-1-Hash in `config.yml` eintragen
- **Lösung 3:** Spieler muss das Resource Pack akzeptieren

### Fisch-Events enden nicht automatisch
- **Ursache:** `duration: 0` in `events.yml` bedeutet unbegrenzt
- **Lösung:** `duration: 600` (oder gewünschte Sekunden) setzen; `/brixxfish admin reload`

### Plugin startet nicht / Fehler in der Konsole
- **Check 1:** Java 17+ installiert? (`java -version`)
- **Check 2:** Paper 1.20.4+? (Spigot **nicht** unterstützt alle Features)
- **Check 3:** Alle Abhängigkeiten vorhanden? (Vault, Economy-Plugin)

### `/top` zeigt nur wenige Spieler
- **Erklärung:** Die Top-Liste zeigt nur **aktuell online** Spieler — das ist eine bekannte Einschränkung des dateibasierten Speichersystems.

---

## Dateistruktur

```
plugins/BrixxFishing/
├── config.yml              ← Hauptkonfiguration
├── fish.yml                ← Fisch-Definitionen
├── events.yml              ← Event-Definitionen
├── messages/
│   ├── messages_de.yml     ← Deutsche Nachrichten
│   ├── messages_en.yml     ← Englische Nachrichten
│   └── messages_tr.yml     ← Türkische Nachrichten
└── playerdata/
    └── <uuid>.yml          ← Spielerdaten (automatisch)
```

---

*© 2025 Hector Gonzalez Lopez — BrixxFishing Plugin. Alle Rechte vorbehalten.*
