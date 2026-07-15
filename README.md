# omarchy-dots

Mein Omarchy-4-Setup (Channel `dev`), reproduzierbar auf einer zweiten Maschine.

## Auf einer neuen Kiste

```bash
git clone <repo-url> ~/.omarchy-dots && ~/.omarchy-dots/bootstrap --dry-run   # erst schauen
~/.omarchy-dots/bootstrap                                                      # dann machen
```

Voraussetzungen: Omarchy auf Channel `dev`, laufendes Hyprland, sudo, und für das
AI-Usage-Widget muss `claude` auf der Maschine eingeloggt sein.

## Was drin ist

| Datei | Zweck |
|---|---|
| `bootstrap` | Repo → System: kopiert die Dateien aus `manifest`, ruft dann `omarchy-setup-clone`, lädt Hyprland neu |
| `sync-back` | System → Repo: holt lokale Änderungen zurück, damit man sie committen kann |
| `manifest` | Welche Datei wohin gehört (von beiden Skripten gelesen) |
| `bin/omarchy-setup-clone` | Installiert/repariert Rise-Bar, Wetter-Pin, scrolloverview-Plugin. Idempotent, `--dry-run` |
| `bin/omarchy-update-preview` | Zeigt vor `omarchy update`, was käme — inkl. Prüfung auf `hyprland\|quickshell\|qt6\|mesa` |
| `config/hypr/` | hypridle (Idle 10/20 Min), bindings (SUPER+A Overview), autostart (hyprpm reload), input (3-Finger-Geste) |

## Kopien, keine Symlinks

Omarchy-Migrationen machen `sed -i` auf Dateien in `~/.config` (163 von 313 schreiben
dorthin). `sed -i` ersetzt einen Symlink durch eine normale Datei — ein Repo-Link würde
also stillschweigend gekappt, und das Repo zeigt danach etwas anderes als das System.
Darum wird kopiert. Preis: nach lokalen Änderungen `./sync-back` laufen lassen.

## Bekannte Bruchstellen

- **Rise-Updater überschreibt die Wetter-Stadt** in `WeatherWidget.qml` + `WeatherPanel.qml`
  → `omarchy-setup-clone` erneut laufen lassen (patcht nur das, was verrutscht ist).
- **Hyprland-Update killt das hyprpm-Plugin** (gegen exakte Version kompiliert)
  → `hyprpm update`.

Stadt fürs Wetter ist per Env überschreibbar: `WEATHER_CITY=Wien omarchy-setup-clone`.
