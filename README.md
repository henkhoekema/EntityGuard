![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2026%2B-blue.svg)
![Script](https://img.shields.io/badge/Home%20Assistant-Script-success.svg)
![License](https://img.shields.io/badge/License-Community-green.svg)

# 🏠 EntityGuard

### Betrouwbaar Home Assistant Script

De afgelopen tijd heb ik, als pensionado, een slim hulpje voor Home Assistant ontwikkeld. Je moet toch wat... 😉.

Dat hulpje heet **EntityGuard**.

Iedere Home Assistant-gebruiker kent het wel: je geeft een apparaat de opdracht om iets te doen, maar om de één of andere reden gebeurt er niets. Je zegt bijvoorbeeld: *"Ga aan!"*, maar de lamp, schakelaar of ventilator lijkt je compleet te negeren.

Normaal gesproken gaat Home Assistant ervan uit dat de opdracht succesvol is verzonden. Of het apparaat de opdracht ook daadwerkelijk heeft uitgevoerd, wordt niet altijd gecontroleerd.

**EntityGuard** doet dat wél.

Na iedere actie controleert het script of het apparaat daadwerkelijk de gewenste status heeft bereikt. Is dat niet het geval, dan probeert EntityGuard de opdracht automatisch opnieuw uit te voeren. Daarbij kan het meerdere pogingen doen, met een instelbare wachttijd en verschillende backoff-strategieën tussen de pogingen.

Lukt het uiteindelijk nog steeds niet? Dan kan EntityGuard een notificatie versturen én wordt precies vastgelegd wat er is gebeurd. Dankzij de uitgebreide logging kun je achteraf eenvoudig zien welke actie is uitgevoerd, hoe lang deze duurde, hoeveel pogingen nodig waren en waarom een actie eventueel is mislukt.

EntityGuard werkt met een groot aantal Home Assistant-domeinen, waaronder verlichting, schakelaars, ventilatoren, covers, sloten, media-spelers, scènes en nog veel meer. Nieuwe domeinen en extra controles zijn bovendien eenvoudig toe te voegen.

Kortom: **EntityGuard zorgt ervoor dat Home Assistant niet direct opgeeft wanneer een apparaat even koppig doet.** Het is een flexibel script met veel instellingen en mogelijkheden om je automatiseringen betrouwbaarder te maken.


---

# 📑 Inhoud

* [✨ Functionaliteit](#-functionaliteit)
* [📦 Installatie](#-installatie)
* [🧩 Ondersteunde domeinen](#-ondersteunde-domeinen)
* [⚙️ Parameters](#️-parameters)
* [🚀 Voorbeeld](#-voorbeeld)
* [📡 Logging](#-logging)
* [💡 Toepassingen](#-toepassingen)
* [👨‍💻 Auteur](#-auteur)
* [📄 Licentie](#-licentie)

---

# ✨ Functionaliteit

✅ Voert een actie uit op een ondersteunde entity

✅ Controleert of de gewenste eindstatus daadwerkelijk is bereikt

✅ Probeert automatisch opnieuw bij mislukking (Retries)

✅ Ondersteunt meerdere backoff-methoden

* Linear
* Constant
* Exponential

✅ Ondersteunt een instelbare **State Delay**

✅ Optionele notificatie bij mislukte uitvoering

✅ Stuurt een event (`entityguard_klaar`) met alle resultaten

✅ Geschikt voor logging naar bijvoorbeeld:

* Google Sheets
* Dashboards
* Grafana
* Eigen automatiseringen

✅ Compatibel met moderne Home Assistant Actions (2026+)

---

# 📦 Installatie

1. Download het bestand `entityguard.yaml` uit deze repository.

2. Plaats het bestand in de map:

```text
/config/scripts/
```

3. Voeg het script toe aan je `scripts.yaml`, of neem de inhoud op in je bestaande scriptconfiguratie.

4. Herlaad de scripts via:

> **Settings → Developer tools → YAML configuration reloading → Scripts**

of herstart Home Assistant.

Na het herladen is het script beschikbaar als:

```text
script.entityguard
```

---

# 🧩 Ondersteunde domeinen

EntityGuard ondersteunt onder andere:

* switch
* light
* input_boolean
* fan
* group
* homeassistant
* scene
* media_player
* cover
* lock
* vacuum
* climate
* humidifier
* dehumidifier
* water_heater
* alarm_control_panel

Nieuwe Home Assistant-domeinen kunnen eenvoudig worden toegevoegd.

---

# ⚙️ Parameters

| Parameter      | Beschrijving                                | Default  |
| -------------- | ------------------------------------------- | -------- |
| `description`  | Korte omschrijving voor logging             | —        |
| `entity_id`    | Entity waarop de actie wordt uitgevoerd     | —        |
| `action`       | Uit te voeren actie (bijv. `light.turn_on`) | —        |
| `data`         | Optionele service-data                      | `{}`     |
| `retries`      | Maximum aantal pogingen                     | `3`      |
| `timeout`      | Wachttijd per poging                        | `15 sec` |
| `retry_delay`  | Basis wachttijd tussen pogingen             | `5 sec`  |
| `backoff_mode` | Linear / Constant / Exponential             | `linear` |
| `state_delay`  | Wachttijd vóór statuscontrole               | `2 sec`  |
| `notify`       | Verstuur notificatie bij mislukking         | `true`   |

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

```text
entityguard_klaar
```

Dit event bevat onder andere:

| Variabele      | Omschrijving                         |
| -------------- | ------------------------------------ |
| `description`  | Omschrijving van de actie            |
| `entity`       | Uitgevoerde entity                   |
| `action`       | Uitgevoerde Home Assistant action    |
| `result`       | OK of MISLUKT                        |
| `duration`     | Totale duur van de uitvoering        |
| `poging`       | Poging waarop de actie succesvol was |
| `retries`      | Maximaal aantal retries              |
| `timeout`      | Ingestelde timeout                   |
| `state_delay`  | Wachttijd voor statuscontrole        |
| `backoff_mode` | Gebruikte backoff-methode            |

Deze informatie kan direct worden gebruikt voor:

* Google Sheets logging
* Dashboards
* Statistieken
* Debugging
* Monitoring
* Automatiseringen

---

# 💡 Toepassingen

EntityGuard is ideaal voor situaties waarin betrouwbaarheid belangrijk is, zoals:

* Slimme verlichting
* Rolluiken
* Deursloten
* HVAC-installaties
* Ventilatie
* Alarmsystemen
* Pompen
* Media-apparatuur
* Kritieke automatiseringen

---

# 👨‍💻 Auteur

**Henk Hoekema**

**Versie**

`v2026.7.3`

---

# 🤝 Bijdragen

Verbeteringen, bugreports en pull requests zijn altijd welkom.

---

# 📄 Licentie

Vrij te gebruiken, maar volledig op eigen risico.
