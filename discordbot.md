# Discord Bot Commands

Stand: 03.08.2026

Diese Datei listet alle Slash-Commands aus dem Ordner `commands` auf.

## Rechte-Logik

Der Bot prueft Commands zentral ueber `config/command-access.json`.

Grundsaetzlich duerfen Server-Owner, Bot-Owner, High-Team-Rollen und Discord-Administratoren die zentrale Pruefung bestehen. Einzelne Commands haben danach noch eigene Code-Pruefungen. Wenn bei einem Command eine zusaetzliche Pruefung steht, muss diese ebenfalls passen.

## Rollen

| Name | Discord-Rolle |
|---|---|
| Owner | <@&1500124312672272444> |
| Head-Admin / Giveaway-Manager | <@&1500125104707731538> |
| Admin | <@&1500125218356858981> |
| Developer | <@&1500146127721205812> |
| Moderation | <@&1500125323558125749> |
| Team | <@&1500136933177688177> |
| Supporter | <@&1500125451434201168> |
| Test-Supporter | <@&1500125556648316969> |
| TicketSupport | <@&1510983585094570085> |
| Blacklist-Admin-Rolle im Code | <@&1499880164920918106> |

## Command-Uebersicht

| Command | Wer darf ihn ausfuehren? | Beschreibung |
|---|---|---|
| `/accesecommands` | Owner, Admin, Developer. Discord-Sichtbarkeit: Administrator. | Zeigt, wer welche Commands nutzen darf. |
| `/add` | Team, Supporter, Test-Supporter. Discord-Sichtbarkeit: ManageMessages. | Fuegt einen Benutzer zu einem Ticket hinzu. Funktioniert nur im Ticket-Channel. |
| `/alert` | Team, Supporter, Test-Supporter. Discord-Sichtbarkeit: ManageMessages. | Sendet eine Alert-Nachricht im Ticket. Funktioniert nur im Ticket-Channel. |
| `/ban` | Owner, Admin, Developer, Moderation. Zusaetzlich braucht der Nutzer `BanMembers`. | Bannt einen Spieler vom Server. |
| `/bewerbungcooldown` | Owner, Admin, Developer. Zusaetzlich braucht der Nutzer `Administrator`. | Setzt den Cooldown einer JSON-Bewerbung. |
| `/bewerbungs panel` | Owner, Admin, Developer. Zusaetzlich braucht der Nutzer `Administrator`. | Postet das Bewerbungs-Panel mit Dropdown. |
| `/bewerungfind` | Team, Owner, Admin, Developer. Discord-Sichtbarkeit: ManageMessages. | Findet Bewerbungen eines Users. |
| `/blacklist` | Zentrale Config: Owner, Admin, Developer. Im Code: Server-Owner oder Blacklist-Admin-Rolle <@&1499880164920918106>. | Verwaltet Links, blockierte Links und gebannte Woerter. |
| `/clear` | Team, Supporter, Test-Supporter. Zusaetzlich braucht der Nutzer `ManageMessages`. | Loescht eine bestimmte Anzahl Nachrichten. |
| `/close` | Zentrale Config: Team, Supporter, Test-Supporter. Im Code: Administrator, Team oder TicketSupport. | Schliesst ein Ticket direkt. Funktioniert nur im Ticket-Channel. |
| `/close_request` | Team, Supporter, Test-Supporter. | Beantragt das Schliessen eines Tickets. Der Ticket-Ersteller oder das Team kann entscheiden. |
| `/configsetup` | Owner, Admin, Developer. Im Code: Server-Owner, Bot-Owner oder Administrator. | Startet den Admin-Setup-Assistenten. |
| `/dispatch list` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. | Zeigt alle offenen Dispatches. |
| `/dispatch close` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. | Schliesst einen Dispatch per Dispatch-ID. |
| `/forward` | Team, Supporter, Test-Supporter. Zusaetzlich muss der Nutzer im Ticket als Team-Aktion berechtigt sein. | Leitet ein Ticket zu einer anderen Kategorie weiter. |
| `/giveaway` | Owner, Admin, Developer, Head-Admin / Giveaway-Manager. Discord-Sichtbarkeit: ManageGuild. | Startet ein Giveaway. |
| `/help` | Jeder. | Zeigt nur Commands, fuer die der Nutzer Rechte hat. |
| `/kick` | Owner, Admin, Developer, Moderation. Zusaetzlich braucht der Nutzer `KickMembers`. | Kickt einen Spieler vom Server. |
| `/lockdown` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. Im Code: Administrator. | Sperrt oder entsperrt den aktuellen Channel. |
| `/mc link` | Jeder. | Verknuepft den Discord-Account mit einem Minecraft-Account. |
| `/mc profil` | Jeder. | Zeigt ein verknuepftes Minecraft-Profil. |
| `/mc stats` | Jeder. | Zeigt Minecraft-Statistiken eines Spielers. |
| `/mc inventar` | Jeder. | Zeigt das letzte bekannte Inventar eines Spielers. |
| `/mcroles` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. Im Code: Owner, Admin oder Developer. | Zeigt alle MC-Rang-zu-Discord-Rollen-Zuordnungen. |
| `/panel_ticket` | Owner, Admin, Developer. Zusaetzlich braucht der Nutzer `Administrator`. | Sendet das Ticket-Panel in den aktuellen Kanal. |
| `/playermsg` | Team, Supporter, Test-Supporter. Discord-Sichtbarkeit: ManageMessages. | Sendet einem User eine Embed-Nachricht per DM. |
| `/rechte` | Owner, Admin, Developer. Discord-Sichtbarkeit: Administrator. | Verwaltet die Rechte fuer Commands in `config/command-access.json`. |
| `/remove` | Team, Supporter, Test-Supporter. Discord-Sichtbarkeit: ManageMessages. | Entfernt einen Benutzer aus einem Ticket. Funktioniert nur im Ticket-Channel. |
| `/report` | Team, Supporter, Test-Supporter. Discord-Sichtbarkeit: ManageMessages. | Meldet einen Spieler mit Grund und optionalem Bild. |
| `/roles` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. | Zeigt alle Rollen des Servers mit IDs. |
| `/teamlist setup` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. Im Code: Administrator. | Setzt den Channel fuer die Team-Liste und sendet das Embed. |
| `/teamlist add-role` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. Im Code: Administrator. | Fuegt eine Rolle zur Team-Liste hinzu. |
| `/teamlist remove-role` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. Im Code: Administrator. | Entfernt eine Rolle aus der Team-Liste. |
| `/teamlist set-embed` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. Im Code: Administrator. | Setzt Titel, Beschreibung oder Farbe des Teamlisten-Embeds. |
| `/teamlist update` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. Im Code: Administrator. | Aktualisiert die Team-Liste manuell. |
| `/teamlist status` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. Im Code: Administrator. | Zeigt die aktuelle Teamlisten-Konfiguration. |
| `/teamtop list` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. | Zeigt Teammitglieder nach Punkten sortiert. |
| `/teamtop info` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. | Zeigt die Punkte-Detailansicht eines Teammitglieds. |
| `/teamtop supportlogs` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. | Zeigt die letzten 50 Support-Eintraege. |
| `/ticket_alert` | Team, Supporter, Test-Supporter. Discord-Sichtbarkeit: ManageMessages. | Sendet eine Ankuendigung an alle Teilnehmer eines Tickets. Funktioniert nur im Ticket-Channel. |
| `/ticket-create` | Team, Supporter, Test-Supporter. Discord-Sichtbarkeit: ManageMessages. | Erstellt ein Support-Ticket fuer einen angegebenen User. |
| `/ticket_id` | Team, Supporter, Test-Supporter. Discord-Sichtbarkeit: ManageMessages. | Zeigt die ID des aktuellen Tickets. Funktioniert nur im Ticket-Channel. |
| `/ticketfinde` | Team, Supporter, Test-Supporter. Discord-Sichtbarkeit: ManageMessages. | Sucht Ticket-Transkripte nach User oder Ticket-ID. |
| `/ticketsperre add` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. Im Code: Administrator, Team oder TicketSupport. | Gibt einem Nutzer eine Ticket-Sperre. |
| `/ticketsperre remove` | Standardrecht fuer nicht konfigurierte Commands: ManageMessages. Im Code: Administrator, Team oder TicketSupport. | Entfernt eine Ticket-Sperre. |
| `/timeout` | Owner, Admin, Developer, Moderation. Zusaetzlich braucht der Nutzer `ModerateMembers`. | Setzt einen Spieler zeitlich stumm. |
| `/unclaim` | Team, Supporter, Test-Supporter. Discord-Sichtbarkeit: ManageMessages. | Gibt ein uebernommenes Ticket wieder frei. Nur der aktuelle Bearbeiter kann das Ticket freigeben. |
| `/userinfos` | Team, Supporter, Test-Supporter. Discord-Sichtbarkeit: ManageMessages. | Zeigt Informationen ueber einen Benutzer. |
| `/verification panel` | Owner, Admin, Developer. Zusaetzlich braucht der Nutzer `Administrator`. | Sendet ein Verifikations-Panel in den aktuellen Channel. |
| `/warn` | Owner, Admin, Developer, Moderation. Zusaetzlich braucht der Nutzer `ModerateMembers`. | Verwarnt einen Spieler. |
| `/wellkommsetup` | Owner, Admin, Developer. Im Code: Server-Owner, Bot-Owner oder Administrator. | Legt den Welcome-Kanal fest und sendet optional eine Vorschau. |

## Hinweise

- Commands ohne Eintrag in `config/command-access.json` fallen zentral auf `ManageMessages` zurueck.
- `help` und `mc` sind laut Rechte-Datei fuer alle Nutzer erlaubt.
- Bei Ticket-Commands reicht die Rolle allein oft nicht: Der Command muss meist im passenden Ticket-Channel ausgefuehrt werden.
- Die Datei `config/command-access.json` enthaelt Rollen-IDs. Diese Markdown-Datei zeigt sie lesbarer als Discord-Rollen-Mentions.
