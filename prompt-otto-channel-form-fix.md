# Fix: Otto Kanal-Formular — Client ID + Client Secret

## Aufgabe
Finde die Otto Kanal-Formular Komponente und ändere die Felder.

## Suche
Suche nach "Otto Partner Username" oder "Otto Partner Password" in:
- src/components/channels/
- src/app/**/channels/
- src/components/forms/

## Änderungen
- Label "Otto Partner Username" → "Client ID"
- Placeholder → "z.B. 12345678-abcd-efgh-ijkl-123456789012"
- Field name/key → "client_id"

- Label "Otto Partner Password" → "Client Secret"  
- Placeholder → "Dein Otto Client Secret"
- Field name/key → "client_secret"

## Wichtig
Die Werte müssen in der channels-Tabelle unter den Keys 
`client_id` und `client_secret` gespeichert werden (in credentials JSONB oder direkt).

## Deploy
docker compose up --build -d
