# PlayerDataSync

Synchronisiert Spielerdaten (Inventar, XP, Herzen, Tränkeffekte, Fortschritte u.v.m.) netzwerkweit über eine gemeinsame MySQL/MariaDB-Datenbank. Wenn ein Spieler von Server A zu Server B wechselt, werden alle Daten nahtlos mitgenommen.

---

## Voraussetzungen

| Anforderung | Version |
|---|---|
| Paper / Purpur | 1.20.4+ |
| Java | 17+ |
| MySQL oder MariaDB | 8.0+ / 10.6+ |

Kein BungeeCord/Velocity nötig — das Plugin läuft auf jedem einzelnen Spigot/Paper-Server und koordiniert sich über die Datenbank.

---

## Installation

1. JAR auf **jeden Server** im Netzwerk in den `plugins/`-Ordner legen
2. Server starten — `plugins/PlayerDataSync/config.yml` wird erstellt
3. Datenbankverbindung in der Config eintragen
4. `server-name` auf jedem Server **einzigartig** setzen (z.B. `lobby`, `citybuild`, `farmserver`)
5. Server neu starten

> Die Datenbank-Tabelle wird beim ersten Start automatisch erstellt.

---

## Commands

Alle Commands erfordern die Berechtigung `playerdatasync.admin`.  
Alias: `/datasync` und `/synced` funktionieren genauso wie `/pdsync`.

### `/pdsync reload`
Lädt die `config.yml` neu, ohne den Server neu starten zu müssen.

---

### `/pdsync save <spieler>`
Speichert die aktuellen Daten eines Online-Spielers manuell in der Datenbank.

```
/pdsync save Notch
```

---

### `/pdsync load <spieler>`
Lädt die Daten eines Online-Spielers manuell aus der Datenbank und wendet sie sofort an.

```
/pdsync load Notch
```

---

### `/pdsync info <spieler>`
Zeigt Informationen zu einem Online-Spieler: UUID, ob Daten in der DB vorhanden sind und ob gerade ein Ladevorgang läuft.

```
/pdsync info Notch
```

---

### `/pdsync status`
Zeigt den aktuellen Plugin-Status: Version, Servername, Anzahl Online-Spieler und aktive Ladevorgänge.

---

## Berechtigungen

| Permission | Standard | Beschreibung |
|---|---|---|
| `playerdatasync.admin` | OP | Zugriff auf alle `/pdsync`-Befehle |
| `playerdatasync.bypass` | Niemand | Umgeht die Inventar-Sperre während des Ladens |

---

## Konfiguration (`config.yml`)

### Server-Name

```yaml
# Einzigartiger Name dieses Servers im Netzwerk
# Wird in der Datenbank gespeichert um zu wissen, von welchem Server die Daten kommen
server-name: "lobby"
```

> Jeder Server im Netzwerk muss einen anderen `server-name` haben.

---

### Datenbankverbindung

```yaml
database:
  host: "localhost"       # MySQL-Serveradresse
  port: 3306              # MySQL-Port
  database: "minecraft_sync"  # Name der Datenbank (muss existieren)
  username: "root"        # Datenbank-Benutzer
  password: "changeme"    # Passwort

  # Connection Pool (HikariCP) - für die meisten Server ausreichend
  pool-size: 10           # Max. gleichzeitige Verbindungen
  connection-timeout: 30000   # Verbindungs-Timeout in ms
  idle-timeout: 600000        # Leerlauf-Timeout in ms (10 Min)
  max-lifetime: 1800000       # Max. Verbindungslebensdauer in ms (30 Min)
```

---

### Was wird synchronisiert?

```yaml
sync:
  inventory: true       # Haupt-Inventar (27 Slots + Hotbar)
  ender-chest: true     # Enderkiste (27 Slots)
  armor: true           # Rüstung (Helm, Brust, Hose, Schuhe)
  offhand: true         # Nebenhand-Slot (Schild etc.)
  experience: true      # XP-Level und Fortschrittsbalken
  health: true          # Aktuelle und maximale Herzen
  food: true            # Hungerbalken, Sättigung und Erschöpfung
  effects: true         # Aktive Tränkeffekte (Typ, Dauer, Stufe)
  game-mode: false      # Spielmodus (Survival/Creative/Adventure) — meist false
  flight: false         # Flugmodus und Flugerlaubnis
  fire-ticks: false     # Feuer-Status
  advancements: true    # Fortschritte / Achievements
  statistics: false     # Spielstatistiken (deaktiviert — sehr umfangreich)
```

**Empfehlung für Netzwerke:**
- `game-mode: false` lassen, wenn verschiedene Server unterschiedliche Modi haben (z.B. Lobby = Adventure, Citybuild = Survival)
- `statistics: false` lassen, da diese sehr viele Daten produzieren und selten netzwerkweit gebraucht werden

---

### Lade-Einstellungen

Wenn ein Spieler den Server betritt, wartet das Plugin kurz bis die Daten des vorherigen Servers gespeichert sind, bevor es sie anwendet. Das verhindert Race-Conditions beim schnellen Server-Wechsel.

```yaml
loading:
  # Wie oft soll auf fertige Daten gewartet werden?
  max-retries: 40

  # Wartezeit zwischen Versuchen in Millisekunden
  # Gesamte Wartezeit: max-retries * retry-delay-ms = 40 * 75ms = 3 Sekunden
  retry-delay-ms: 75

  # Ab wann gilt ein Data-Lock als veraltet? (z.B. nach Server-Absturz)
  # Einheit: Sekunden
  stale-lock-timeout: 10

  # Spieler-Inventar während des Ladens sperren (verhindert Item-Verlust)
  freeze-on-loading: true
```

| Option | Erklärung |
|---|---|
| `max-retries` | Erhöhen wenn Spieler beim schnellen Wechsel manchmal leere Inventare sehen |
| `retry-delay-ms` | Kleinere Werte = schnelleres Laden, höhere Serverlast |
| `stale-lock-timeout` | Nach einem Server-Absturz wird der Lock nach dieser Zeit ignoriert |
| `freeze-on-loading` | Verhindert dass Spieler Items wegwerfen während das Inventar noch lädt |

---

### Nachrichten

Alle Nachrichten unterstützen `&`-Farbcodes. `{player}` wird durch den Spielernamen ersetzt.

```yaml
messages:
  prefix: "&8[&6DataSync&8] &r"
  loading: "&7Spielerdaten werden geladen..."
  loaded: "&aSpielerdaten erfolgreich geladen!"
  error-loading: "&cFehler beim Laden! Bitte wende dich an einen Admin."
  error-timeout: "&cTimeout - Daten konnten nicht geladen werden!"
  no-permission: "&cKeine Berechtigung!"
  reload-success: "&aKonfiguration neu geladen!"
  save-success: "&aDaten von &e{player} &aerfolgreich gespeichert!"
  load-success: "&aDaten von &e{player} &aerfolgreich geladen!"
  player-not-found: "&cSpieler &e{player} &cnicht gefunden!"
```

---

### Debug-Modus

```yaml
# Gibt ausführliche Informationen in die Konsole aus
# Nur für Fehlersuche aktivieren
debug: false
```

---

## Vollständige `config.yml`

```yaml
server-name: "lobby"

database:
  host: "localhost"
  port: 3306
  database: "minecraft_sync"
  username: "root"
  password: "changeme"
  pool-size: 10
  connection-timeout: 30000
  idle-timeout: 600000
  max-lifetime: 1800000

sync:
  inventory: true
  ender-chest: true
  armor: true
  offhand: true
  experience: true
  health: true
  food: true
  effects: true
  game-mode: false
  flight: false
  fire-ticks: false
  advancements: true
  statistics: false

loading:
  max-retries: 40
  retry-delay-ms: 75
  stale-lock-timeout: 10
  freeze-on-loading: true

messages:
  prefix: "&8[&6DataSync&8] &r"
  loading: "&7Spielerdaten werden geladen..."
  loaded: "&aSpielerdaten erfolgreich geladen!"
  error-loading: "&cFehler beim Laden! Bitte wende dich an einen Admin."
  error-timeout: "&cTimeout - Daten konnten nicht geladen werden!"
  no-permission: "&cKeine Berechtigung!"
  reload-success: "&aKonfiguration neu geladen!"
  save-success: "&aDaten von &e{player} &aerfolgreich gespeichert!"
  load-success: "&aDaten von &e{player} &aerfolgreich geladen!"
  player-not-found: "&cSpieler &e{player} &cnicht gefunden!"

debug: false
```

---

## Wie funktioniert die Synchronisierung?

```
Spieler wechselt von Server A → Server B

Server A (beim Quit):
  1. Setzt data_ready = FALSE in der Datenbank (Lock)
  2. Speichert alle Daten asynchron
  3. Setzt data_ready = TRUE (Lock aufheben)

Server B (beim Join):
  1. Prüft ob data_ready = TRUE
  2. Falls FALSE: wartet (retry-delay-ms) und prüft erneut
  3. Nach max-retries oder wenn TRUE: Daten laden & anwenden
  4. Spieler wird entfroren
```

Der Lock verhindert dass Server B veraltete Daten lädt während Server A noch speichert.

---

## Häufige Probleme

**Spieler haben beim Wechsel ein leeres Inventar**  
→ `max-retries` erhöhen oder `retry-delay-ms` erhöhen (mehr Wartezeit geben)

**`Datenbankverbindung fehlgeschlagen` beim Start**  
→ Host, Port, Datenbankname, Benutzername und Passwort prüfen  
→ Sicherstellen dass die Datenbank existiert und der Benutzer Zugriffsrechte hat

**Daten werden nicht übertragen zwischen bestimmten Servern**  
→ Prüfen ob beide Server dieselbe `database`-Config nutzen  
→ `server-name` auf jedem Server muss einzigartig sein

**`Veralteter Data-Lock erkannt` in der Konsole**  
→ Normal nach einem Server-Absturz — das Plugin hat den hängenden Lock automatisch ignoriert  
→ `stale-lock-timeout` anpassen falls das zu oft oder zu früh passiert
