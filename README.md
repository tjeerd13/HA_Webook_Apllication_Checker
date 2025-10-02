# Applicatie Status & Notificatie (Home Assistant Blueprint)

Deze Home Assistant blueprint automatiseert het verwerken van webhook-berichten van externe applicaties, houdt statistieken bij via helpers, en stuurt notificaties bij uitval of herstel.

---

## 🔧 Functionaliteit

- **Webhook-verwerking:** Ontvangt berichten via een opgegeven webhook ID.
- **Statistieken:** Houdt bij hoeveel berichten er zijn ontvangen in de laatste 5 minuten, 24 uur en totaal.
- **Controle:** Checkt elke 5 minuten of de laatste update ouder is dan een ingestelde drempel.
- **Notificaties:** Stuurt een melding bij uitval en herstel.
- **Meerdere applicaties:** Ondersteunt meerdere applicaties via een selecteerbare `application_id`.

---

## 📨 Vereiste Webhook Payload

De webhook moet een JSON-payload bevatten met minimaal de volgende velden:

```json
{
  "timestamp": "2025-10-02 11:24:17",
  "message_count_last_5min": 12,
  "message_count_last_24h": 340,
  "message_count_total": 10234,
  "source": "gps_log"
}
```

| Veld                     | Uitleg                                                        |
|--------------------------|--------------------------------------------------------------|
| `timestamp`              | Datum en tijd van het bericht (YYYY-MM-DD HH:MM:SS)          |
| `message_count_last_5min`| Aantal berichten in de laatste 5 minuten                     |
| `message_count_last_24h` | Aantal berichten in de laatste 24 uur                        |
| `message_count_total`    | Totaal aantal berichten sinds start                          |
| `source`                 | Applicatie-ID (moet overeenkomen met `application_id`)       |

---

## 📦 Vereiste Helpers

Voor elke applicatie-ID (bijv. `gps_log`) moeten de volgende helpers worden aangemaakt in YAML:

```yaml
input_datetime:
  gps_log_last_update:
    name: Laatste gps_log update
    has_date: true
    has_time: true
    icon: mdi:crosshairs-gps

input_number:
  gps_log_message_count_5min:
    name: gps_log berichten laatste 5 minuten
    min: 0
    max: 10000
    step: 1
    icon: mdi:crosshairs-gps

  gps_log_message_count_24h:
    name: gps_log berichten laatste 24 uur
    min: 0
    max: 10000
    step: 1
    icon: mdi:crosshairs-gps

  gps_log_message_count_total:
    name: gps_log totaal aantal berichten
    min: 0
    max: 100000
    step: 1
    icon: mdi:crosshairs-gps
```

> **Let op:**  
> Vervang `gps_log` door je eigen `application_id` als je meerdere applicaties gebruikt.

---

## ⚙️ Blueprint Inputs

| Input              | Beschrijving                                                                 |
|--------------------|------------------------------------------------------------------------------|
| `webhook_id`       | ID van de webhook die door het script wordt aangeroepen                      |
| `application_id`   | Unieke naam of ID van de applicatie (bijv. `gps_log`, `sensorhub`)           |
| `notify_device`    | Mobiel apparaat voor pushmeldingen                                           |
| `cooldown`         | Minimale tijd tussen uitvalmeldingen (standaard: 3600 seconden)              |
| `threshold_minutes`| Tijd zonder update waarna een waarschuwing wordt gestuurd (standaard: 10 min)|

---

## 📋 Voorbeeld Gebruik

1. Stel `webhook_id` in op de waarde die je script gebruikt.
2. Kies een `application_id` zoals `gps_log`.
3. Selecteer je mobiele apparaat voor notificaties.
4. Pas eventueel `cooldown` en `threshold_minutes` aan.

---

## 📁 Installatie

Plaats de blueprint in je Home Assistant map:

```
config/blueprints/automation/<jouw_github_gebruikersnaam>/applicatie_status_notificatie.yaml
```