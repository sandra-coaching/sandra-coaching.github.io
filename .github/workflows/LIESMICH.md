# Automatischer Upload zu netcup

Bei jedem Push nach `main` lädt GitHub den Inhalt dieses Ordners per FTPS auf den
Webspace. Nichts davon läuft auf einem fremden Rechner: Die Zugangsdaten liegen
verschlüsselt als „Secrets“ im Repository und tauchen weder im Code noch im
Protokoll auf.

## Einmalig einzurichten – vier Secrets

GitHub → Repository → **Settings** → **Secrets and variables** → **Actions** →
**New repository secret**:

| Name | Inhalt |
|---|---|
| `FTP_SERVER` | Adresse des FTP-Servers, z. B. `ganzbeidir-coaching.de` |
| `FTP_USER` | Benutzername des FTP-Zugangs aus Plesk |
| `FTP_PASSWORD` | zugehöriges Passwort |
| `FTP_ORDNER` | Zielordner, bei netcup meist `/httpdocs/` |

## Erst testen, dann scharf schalten

GitHub → **Actions** → „Website zu netcup hochladen“ → **Run workflow** →
Haken bei **Probelauf** → starten. Der Lauf zeigt dann nur an, welche Dateien er
anfassen würde, und lädt nichts hoch. Stimmt der Zielordner, denselben Lauf ohne
Haken starten.

## Wenn etwas schiefgeht

* **„530 Login incorrect“** – Benutzername oder Passwort stimmen nicht.
* **Dateien landen im falschen Ordner** – `FTP_ORDNER` anpassen. Je nach
  FTP-Zugang ist das `/httpdocs/`, `/` oder `/ganzbeidir-coaching.de/httpdocs/`.
* **Verbindung bricht ab** – `protocol: ftps` in `hochladen.yml` auf
  `ftps-legacy` ändern.

Beim ersten Lauf werden alle Dateien übertragen. Danach merkt sich die Aktion in
einer Datei `.ftp-deploy-sync-state.json` auf dem Server, was schon oben liegt,
und lädt nur noch Geändertes.
