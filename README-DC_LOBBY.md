# MCBotBridge

Verbindet einen Paper-Minecraft-Server mit einem Discord-Bot. Spieler können ihren Discord-Account verknüpfen, Rang-Änderungen werden automatisch an Discord gemeldet und Lobby-Schutzfunktionen lassen sich direkt in der Config aktivieren.

---

## Voraussetzungen

| Plugin | Typ | Zweck |
|---|---|---|
| [LuckPerms](https://luckperms.net/) | **Pflicht** | Rang-Verwaltung & Rang-Events |
| [Vault](https://www.spigotmc.org/resources/vault.34315/) | Optional | Kontostand an Discord melden |
| [PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/) | Optional | Beliebige Placeholder mitsenden |
| [Citizens](https://www.spigotmc.org/resources/citizens.13811/) | Optional | NPC-Nutzung für Unverifizierte blockieren |

**Server:** Paper 1.21+  
**Java:** 17+

---

## Installation

1. JAR in den `plugins/`-Ordner des Servers legen
2. Server starten — `plugins/MCBotBridge/config.yml` wird erstellt
3. `bot-api-url` und `api-key` in der Config eintragen
4. Server neu starten

---

## Commands

### `/verify`
Startet die Discord-Verknüpfung. Der Spieler erhält einen Code, den er im Discord-Server mit `/mc link <code>` einlöst.

| | |
|---|---|
| **Berechtigung** | `mcbotbridge.verify` (Standard: alle Spieler) |
| **Nur für** | Spieler (nicht Konsole) |

---

### `/syncrank [spieler]`
Sendet den aktuellen LuckPerms-Rang des Spielers manuell an Discord. Ohne Argument wird der eigene Rang synchronisiert.

| | |
|---|---|
| **Berechtigung eigener Rang** | `mcbotbridge.syncrank` (Standard: alle Spieler) |
| **Berechtigung andere Spieler** | `mcbotbridge.syncrank.others` (Standard: OP) |

---

## Berechtigungen

| Permission | Standard | Beschreibung |
|---|---|---|
| `mcbotbridge.verify` | Alle Spieler | `/verify` nutzen |
| `mcbotbridge.syncrank` | Alle Spieler | Eigenen Rang synchronisieren |
| `mcbotbridge.syncrank.others` | OP | Rang anderer Spieler synchronisieren |

---

## Konfiguration (`config.yml`)

### Verbindung zum Bot

```yaml
# URL des Discord-Bots (lokal oder über ngrok/öffentliche URL)
bot-api-url: "http://localhost:3001"

# Muss identisch mit MC_API_KEY in der .env des Bots sein!
api-key: "dein_geheimer_schluessel"
```

---

### Vault & PlaceholderAPI

```yaml
# Kontostand des Spielers beim Join/Quit mitsenden (benötigt Vault)
send-balance: true

# PlaceholderAPI-Werte die an Discord gemeldet werden sollen
placeholders:
  - "%essentials_afk%"
  - "%player_level%"
  - "%luckperms_prefix%"
```

---

### Server-Events

```yaml
# Server-Start und Server-Stop an den Discord-Bot melden
send-server-events: true
```

---

### Spawn-Punkt

Spieler werden beim Joinen und/oder nach dem Tod automatisch hierhin teleportiert.

```yaml
spawn:
  enabled: false               # Teleport beim Join aktivieren
  teleport-on-death: false     # Teleport nach dem Tod beim Respawn
  world: "world"               # Weltname
  x: 0.5
  y: 64.0
  z: 0.5
  yaw: 0.0                     # Blickrichtung (0=Süd, 90=West, 180=Nord, 270=Ost)
  pitch: 0.0
```

---

### Willkommens-Titel

Großer Titel der beim Joinen auf dem Bildschirm erscheint. Farbcodes mit `&` möglich.

```yaml
welcome:
  enabled: false
  title: "&6Willkommen bei"
  subtitle: "&aDein Server"
  fade-in: 20      # Einblendzeit in Ticks (20 = 1 Sekunde)
  stay: 60         # Anzeigedauer in Ticks
  fade-out: 20     # Ausblendzeit in Ticks
```

---

### Lobby-Schutz

Schützt die Lobby vor ungewollten Aktionen. Alle Optionen wirken global (für alle Spieler).

```yaml
lobby:
  no-hunger: false       # Hunger verlieren deaktivieren
  no-damage: false       # Schaden/Herzen verlieren deaktivieren
  no-block-break: false  # Blöcke abbauen deaktivieren
```

---

### Discord-Verifikation

Prüft beim Join ob ein Spieler seinen Discord-Account verknüpft hat.

```yaml
verification:
  # Verifikation beim Join prüfen
  check-on-join: false

  # Unverifizierte Spieler einfrieren (können sich nicht bewegen)
  # Wird automatisch aufgehoben sobald der Spieler verifiziert ist
  freeze-unverified: false
  frozen-message: "&cDu bist eingefroren! Verknüpfe deinen Discord-Account mit &e/verify&c."

  # Nachrichten nach dem Verifikations-Check
  verified-message: "&a✅ Dein Discord-Account ist mit Minecraft verknüpft!"
  unverified-message: "&c❌ Dein Discord-Account ist nicht verknüpft! Tippe &e/verify &cingame."

  # Unverifizierte Spieler können keine Citizens-NPCs benutzen (benötigt Citizens)
  block-npcs: false
  unverified-npc-message: "&cDu musst deinen Discord-Account verknüpfen! Tippe &e/verify&c."
```

#### Freeze-System

Wenn `freeze-unverified: true` gesetzt ist:
- Spieler werden sofort beim Join eingefroren
- Nach dem API-Check: verifiziert → automatisch entfroren
- Nicht verifiziert → bleibt eingefroren und erhält die `frozen-message`
- Sobald der Spieler sich über Discord verifiziert, wird der Freeze automatisch aufgehoben

---

## Vollständige `config.yml`

```yaml
bot-api-url: "http://localhost:3001"
api-key: "dein_geheimer_schluessel"

send-balance: true

placeholders:
  - "%essentials_afk%"
  - "%player_level%"
  - "%luckperms_prefix%"

send-server-events: true

spawn:
  enabled: false
  teleport-on-death: false
  world: "world"
  x: 0.5
  y: 64.0
  z: 0.5
  yaw: 0.0
  pitch: 0.0

welcome:
  enabled: false
  title: "&6Willkommen bei"
  subtitle: "&aDein Server"
  fade-in: 20
  stay: 60
  fade-out: 20

lobby:
  no-hunger: false
  no-damage: false
  no-block-break: false

verification:
  check-on-join: false
  freeze-unverified: false
  frozen-message: "&cDu bist eingefroren! Verknüpfe deinen Discord-Account mit &e/verify&c."
  verified-message: "&a✅ Dein Discord-Account ist mit Minecraft verknüpft!"
  unverified-message: "&c❌ Dein Discord-Account ist nicht verknüpft! Tippe &e/verify &cingame."
  block-npcs: false
  unverified-npc-message: "&cDu musst deinen Discord-Account verknüpfen! Tippe &e/verify&c."
```

---

## Farbcodes

Alle Nachrichten-Felder unterstützen `&`-Farbcodes:

| Code | Farbe | Code | Farbe |
|---|---|---|---|
| `&0` | Schwarz | `&8` | Dunkelgrau |
| `&1` | Dunkelblau | `&9` | Blau |
| `&2` | Dunkelgrün | `&a` | Grün |
| `&3` | Türkis | `&b` | Aqua |
| `&4` | Dunkelrot | `&c` | Rot |
| `&5` | Lila | `&d` | Pink |
| `&6` | Gold | `&e` | Gelb |
| `&7` | Grau | `&f` | Weiß |
| `&l` | **Fett** | `&o` | *Kursiv* |
| `&n` | Unterstrichen | `&m` | ~~Durchgestrichen~~ |
| `&r` | Reset | `&k` | Magisch |
