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

---

# 💭 Developer's Note

## Waarom EntityGuard een script is en geen kant-en-klare integratie

EntityGuard is bewust ontwikkeld als een **Home Assistant-script** en niet als een plug-and-play integratie. Niet omdat het technisch onmogelijk is, maar omdat ik geloof dat Home Assistant meer is dan alleen een verzameling kant-en-klare oplossingen.

Home Assistant is voor mij geen app-store, maar een gereedschapskist. Juist het begrijpen van je eigen automatiseringen maakt het platform zo krachtig.

Daarom is EntityGuard ontworpen als gereedschap, niet als een afgesloten product.

Met EntityGuard:

* zie je wat er gebeurt;
* begrijp je de achterliggende logica;
* kun je het script aanpassen;
* kun je het uitbreiden;
* kun je het debuggen;
* en kun je het verder verbeteren.

Een volledig afgeschermde integratie zou veel van die mogelijkheden verbergen. Dat past niet bij de filosofie achter dit project.

---

## AI is een hulpmiddel, geen vervanging voor inzicht

Moderne AI-tools zoals ChatGPT, Claude en Copilot zijn uitstekende hulpmiddelen bij het schrijven van code, het uitleggen van YAML of het bedenken van oplossingen.

Maar uiteindelijk kent AI jouw Home Assistant-omgeving niet.

AI weet bijvoorbeeld niet:

* hoe jouw Zigbee-netwerk zich gedraagt;
* welke WiFi-vertragingen je soms hebt;
* welke apparaten af en toe een statusupdate missen;
* welke uitzonderingen jouw automatiseringen bevatten;
* waarom een bepaalde lamp nét iets later reageert dan verwacht.

Dat zijn ervaringen die alleen ontstaan door testen, meten, aanpassen en opnieuw proberen.

Juist daar ligt de kracht van Home Assistant.

---

## Waarom ik voor scripts kies

Een script laat zien wat er gebeurt.

Je kunt iedere regel bekijken, begrijpen en aanpassen aan je eigen situatie. Daardoor leer je niet alleen hoe EntityGuard werkt, maar ook hoe Home Assistant zelf werkt.

Met een script kun je:

* begrijpen wat er gebeurt;
* fouten opsporen;
* optimaliseren;
* uitbreiden;
* experimenteren;
* en nieuwe ideeën uitproberen.

Dat maakt je niet alleen gebruiker van Home Assistant, maar ook bouwer.

---

## Uiteindelijk gaat het om plezier

Home Assistant draait voor mij niet alleen om domotica, maar ook om nieuwsgierigheid.

Het is leuk om te experimenteren, te testen, te bouwen en stap voor stap iets beter te maken.

EntityGuard is vanuit die gedachte ontstaan.

Niet als een gesloten oplossing die alles voor je doet, maar als een hulpmiddel dat je kunt begrijpen, aanpassen en verder ontwikkelen.

Ik hoop dat het je niet alleen helpt om betrouwbaardere automatiseringen te bouwen, maar je ook inspireert om zelf verder te experimenteren.

---

# 👨‍💻 Auteur

**Henk Hoekema**

**Versie**

`v2026.7.3`

[<img src="coffee.png" width="300">](https://paypal.me/henkhoekema)
[![Venmo](https://img.shields.io/badge/Venmo-Supported-blue?style=for-the-badge)](https://paypal.me/henkhoekema)


---

# 🤝 Bijdragen

Home Assistant draait om samen bouwen en van elkaar leren. Ook ik heb niet alle wijsheid in pacht. Daarom zijn ideeën, verbeteringen, bugreports en pull requests altijd van harte welkom.

---

# 📄 Licentie

Vrij te gebruiken, maar volledig op eigen risico.
