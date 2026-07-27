# 🏠 EntityGuard
### Betrouwbare Home Assistant Script Blueprint

![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2026%2B-blue.svg)
![Blueprint](https://img.shields.io/badge/Blueprint-Script-success.svg)
![License](https://img.shields.io/badge/License-Community-green.svg)

**EntityGuard** is een krachtige **Home Assistant Script Blueprint** die acties uitvoert op entities en vervolgens controleert of de gewenste status daadwerkelijk is bereikt.

Wanneer een actie mislukt, probeert EntityGuard deze automatisch opnieuw uit te voeren volgens een instelbare retry- en backoff-strategie. Daarnaast ondersteunt de blueprint uitgebreide logging, notificaties en event-uitvoer, waardoor hij ideaal is voor betrouwbare automatiseringen.

---

# 📑 Inhoud

- [✨ Functionaliteit](#-functionaliteit)
- [📦 Installatie](#-installatie)
- [🧩 Ondersteunde domeinen](#-ondersteunde-domeinen)
- [⚙️ Parameters](#️-parameters)
- [🚀 Voorbeeld](#-voorbeeld)
- [📡 Logging](#-logging)
- [💡 Toepassingen](#-toepassingen)
- [👨‍💻 Auteur](#-auteur)
- [📄 Licentie](#-licentie)

---

# ✨ Functionaliteit

✅ Voert een actie uit op een ondersteunde entity

✅ Controleert of de gewenste eindstatus daadwerkelijk is bereikt

✅ Probeert automatisch opnieuw bij mislukking (Retries)

✅ Ondersteunt meerdere backoff-methoden

- Linear
- Constant
- Exponential

✅ Ondersteunt een instelbare **State Delay**

✅ Optionele notificatie bij mislukte uitvoering

✅ Stuurt een event (`entityguard_klaar`) met alle resultaten

✅ Geschikt voor logging naar bijvoorbeeld:

- Google Sheets
- Dashboards
- Grafana
- Eigen automatiseringen

✅ Compatibel met moderne Home Assistant Actions (2026+)

---

# 📦 Installatie

## Methode 1 – Import via URL

Ga naar:

> **Instellingen → Automatiseringen & Scènes → Blueprints → Importeren → URL**

Gebruik vervolgens:

```
https://github.com/<jouw-gebruikersnaam>/<jouw-repository>/blob/main/entityguard.yaml
```

> Vervang de URL zodra de repository online staat.

---

## Methode 2 – YAML upload

1. Download `entityguard.yaml`
2. Open:

> **Instellingen → Automatiseringen & Scènes → Blueprints → Importeren → YAML-upload**

3. Selecteer het bestand.

---

# 🧩 Ondersteunde domeinen

EntityGuard ondersteunt onder andere:

- switch
- light
- input_boolean
- fan
- group
- homeassistant
- scene
- media_player
- cover
- lock
- vacuum
- climate
- humidifier
- dehumidifier
- water_heater
- alarm_control_panel

Nieuwe Home Assistant-domeinen kunnen eenvoudig worden toegevoegd.

---

# ⚙️ Parameters

| Parameter | Beschrijving | Default |
|-----------|--------------|---------|
| `description` | Korte omschrijving voor logging | — |
| `entity_id` | Entity waarop de actie wordt uitgevoerd | — |
| `action` | Uit te voeren actie (bijv. `light.turn_on`) | — |
| `data` | Optionele service-data | `{}` |
| `retries` | Maximum aantal pogingen | `3` |
| `timeout` | Wachttijd per poging | `15 sec` |
| `retry_delay` | Basis wachttijd tussen pogingen | `5 sec` |
| `backoff_mode` | Linear / Constant / Exponential | `linear` |
| `state_delay` | Wachttijd vóór statuscontrole | `2 sec` |
| `notify` | Verstuur notificatie bij mislukking | `true` |

---

# 🚀 Voorbeeld

```yaml
action:
  - action: script.entityguard
    data:
      description: "Lamp woonkamer aan"
      entity_id: light.erker_licht
      action: light.turn_on
      retries: 3
      timeout: 15
      retry_delay: 5
      backoff_mode: exponential
```

---

# 📡 Logging

Na iedere uitvoering wordt automatisch een event verstuurd:

```
entityguard_klaar
```

Dit event bevat onder andere:

| Variabele | Omschrijving |
|------------|--------------|
| `description` | Omschrijving van de actie |
| `entity` | Uitgevoerde entity |
| `action` | Uitgevoerde Home Assistant action |
| `result` | OK of MISLUKT |
| `duration` | Totale duur van de uitvoering |
| `poging` | Poging waarop de actie succesvol was |
| `retries` | Maximaal aantal retries |
| `timeout` | Ingestelde timeout |
| `state_delay` | Wachttijd voor statuscontrole |
| `backoff_mode` | Gebruikte backoff-methode |

Deze informatie kan direct worden gebruikt voor:

- Google Sheets logging
- Dashboards
- Statistieken
- Debugging
- Monitoring
- Automatiseringen

---

# 💡 Toepassingen

EntityGuard is ideaal voor situaties waarin betrouwbaarheid belangrijk is, zoals:

- Slimme verlichting
- Rolluiken
- Deursloten
- HVAC-installaties
- Ventilatie
- Alarmsystemen
- Pompen
- Media-apparatuur
- Kritieke automatiseringen

---

# 👨‍💻 Auteur

**Henk**

Ontwikkeld voor de Nederlandse Home Assistant-community.

**Versie**

`v2026.7.2 Blueprint Edition`

---

# 🤝 Bijdragen

Verbeteringen, bugreports en pull requests zijn altijd welkom.

---

# 📄 Licentie

Vrij te gebruiken binnen de Home Assistant-community.

Gebruik op eigen risico.
