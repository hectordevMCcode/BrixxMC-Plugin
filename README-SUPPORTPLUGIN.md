# StaffTools — Berechtigungen

## LuckPerms Befehle

```
/lp group <gruppe> permission set <permission> true
```

---

## Übersicht

| Permission | Befehl | Standard |
|---|---|---|
| `stafftools.use` | `/support`, `/report` | **alle Spieler** |
| `stafftools.dispatch` | `/dispatch` | OP |
| `stafftools.clearchat` | `/clearchat` | OP |
| `stafftools.mute` | `/mute`, `/unmute` | OP |
| `stafftools.freeze` | `/supportfreeze` | OP |
| `stafftools.alert` | `/admin-alert` | OP |
| `stafftools.staffchat` | `/staffchat`, `/sc` | OP |

### Vanish — Nutzen
| Permission | Befehl |
|---|---|
| `stafftools.vanish.mod` | `/mv` |
| `stafftools.vanish.admin` | `/av` |
| `stafftools.vanish.owner` | `/ov` |
| `stafftools.vanish.dev` | `/dv` |

### Vanish — Sehen (wer kann wen vanished sehen)
| Permission | Kann sehen |
|---|---|
| `stafftools.vanish.see.mod` | Mod-Vanish (`/mv`) |
| `stafftools.vanish.see.admin` | Admin-Vanish (`/av`) |
| `stafftools.vanish.see.owner` | Owner-Vanish (`/ov`) + Dev-Vanish (`/dv`) |

---

## Empfohlene Gruppen-Einstellungen

### mod
```
/lp group mod permission set stafftools.dispatch true
/lp group mod permission set stafftools.clearchat true
/lp group mod permission set stafftools.mute true
/lp group mod permission set stafftools.freeze true
/lp group mod permission set stafftools.staffchat true
/lp group mod permission set stafftools.vanish.mod true
/lp group mod permission set stafftools.vanish.see.mod true
```

### Admin
```
/lp group admin permission set stafftools.dispatch true
/lp group admin permission set stafftools.clearchat true
/lp group admin permission set stafftools.mute true
/lp group admin permission set stafftools.freeze true
/lp group admin permission set stafftools.alert true
/lp group admin permission set stafftools.staffchat true
/lp group admin permission set stafftools.vanish.mod true
/lp group admin permission set stafftools.vanish.admin true
/lp group admin permission set stafftools.vanish.see.mod true
/lp group admin permission set stafftools.vanish.see.admin true
```

### Owner
```
/lp group owner permission set stafftools.dispatch true
/lp group owner permission set stafftools.clearchat true
/lp group owner permission set stafftools.mute true
/lp group owner permission set stafftools.freeze true
/lp group owner permission set stafftools.alert true
/lp group owner permission set stafftools.staffchat true
/lp group owner permission set stafftools.vanish.mod true
/lp group owner permission set stafftools.vanish.admin true
/lp group owner permission set stafftools.vanish.owner true
/lp group owner permission set stafftools.vanish.see.mod true
/lp group owner permission set stafftools.vanish.see.admin true
/lp group owner permission set stafftools.vanish.see.owner true
```

### Developer
```
/lp group developer permission set stafftools.dispatch true
/lp group developer permission set stafftools.clearchat true
/lp group developer permission set stafftools.mute true
/lp group developer permission set stafftools.freeze true
/lp group developer permission set stafftools.alert true
/lp group developer permission set stafftools.staffchat true
/lp group developer permission set stafftools.vanish.dev true
/lp group developer permission set stafftools.vanish.see.mod true
/lp group developer permission set stafftools.vanish.see.admin true
/lp group developer permission set stafftools.vanish.see.owner true
```

---

## Vanish-Hierarchie

```
Spieler  → sieht niemanden
Mod      → sieht: Mods
Admin    → sieht: Mods + Admins
Owner    → sieht: alle
Dev      → sieht: alle
```
