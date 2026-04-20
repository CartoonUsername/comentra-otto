# Otto Listing Prompts

---

## Prompt 1: Produkt ↔ Otto-Kanal verknüpfen

Auf der Produktdetailseite (Hauptartikel) fehlt die Möglichkeit, einen Kanal (z.B. Otto) zu verknüpfen.

Aufgabe:
- Auf der Produkt-Detailseite eine "Kanäle"-Sektion hinzufügen
- Zeigt alle aktiven Kanäle des Tenants (aus channels-Tabelle)
- Für Otto: Eingabefeld für die Otto Product Reference (otto_product_reference) und Otto Category
- Verknüpfung wird in channel_products-Tabelle gespeichert (product_id + channel_id + externe Referenz als JSONB)
- Falls channel_products noch nicht existiert: Migration erstellen mit Feldern: id, tenant_id, product_id, channel_id, external_id, external_data (JSONB), status, created_at
- Status-Badge: "Verknüpft", "Nicht verknüpft", "Wird hochgeladen"

Tenant: 6163af6f-dadf-446b-af24-c0f4498351e5
Otto Channel ID: b3fe12fe-5af9-4fd2-b147-f80690510499

Schau dir zuerst das Schema von products, product_variants und channels an.

---

## Prompt 2: Otto Listing Upload (Produkt mit Varianten hochladen)

Baue den Otto Listing Upload via Otto Partner API v4.

Aufgabe:
- Neuer BullMQ Worker: workers/otto-product-upload.worker.ts (falls noch nicht vorhanden, updaten)
- API-Endpunkt: POST /api/[tenantId]/channels/otto/upload-product
- Liest Produkt + Varianten aus products/product_variants
- Mappt auf Otto API v4 Format (Products endpoint: POST https://api.otto.market/v4/products)
- Pflichtfelder Otto: sku, category, brandId, title, description, attributes (Merkmale), pricing, availability
- Varianten werden als separate SKUs mit parent-Referenz hochgeladen
- Nach erfolgreichem Upload: external_id in channel_products speichern, status → "live"
- Fehler pro Variante einzeln loggen in sync_logs-Tabelle

Otto API Docs: https://api.otto.market/docs
Credentials aus channels-Tabelle (channel_id: b3fe12fe-5af9-4fd2-b147-f80690510499), Token via client_credentials grant.

UI: Button "Zu Otto hochladen" auf der Kanal-Verknüpfungs-Sektion (aus Prompt 1).

Schau dir zuerst lib/marketplace/otto/client.ts und workers/otto-product-upload.worker.ts an.
