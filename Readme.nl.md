![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2026%2B-blue.svg)
![Script](https://img.shields.io/badge/Home%20Assistant-Script-success.svg)
![License](https://img.shields.io/badge/License-Community-green.svg)


> [!TIP]
> **De documentatie van EntityGuard is beschikbaar in twee talen.**
>
> 🇳🇱 **Nederlands** (huidige pagina) • 🇬🇧 **[English version](README.md)**


# EntityGuard

EntityGuard is een script voor Home Assistant dat ik heb ontwikkeld om acties betrouwbaarder uit te voeren.

Tijdens het automatiseren liep ik regelmatig tegen hetzelfde probleem aan: Home Assistant stuurt een opdracht naar een apparaat, maar controleert niet of die opdracht ook daadwerkelijk is uitgevoerd. Soms blijft een lamp uit, reageert een ventilator niet of beweegt een cover simpelweg niet.

EntityGuard lost dat op.

Na iedere actie controleert het script of de gewenste toestand is bereikt. Is dat niet het geval, dan probeert het de actie automatisch opnieuw. Je bepaalt zelf hoeveel pogingen worden gedaan, hoe lang ertussen gewacht wordt en welke backoff-strategie wordt gebruikt. Mocht een actie uiteindelijk toch mislukken, dan kan EntityGuard een melding sturen en alle gegevens loggen voor verdere analyse.

Het script ondersteunt veel verschillende Home Assistant-domeinen en is eenvoudig uit te breiden.

---

## Mogelijkheden

- Controleert of een apparaat daadwerkelijk de gewenste toestand heeft bereikt.
- Probeert een actie automatisch opnieuw wanneer dat niet het geval is.
- Ondersteunt verschillende backoff-strategieën (constant, linear en exponential).
- Kan een melding sturen wanneer een actie definitief mislukt.
- Stuurt een `entityguard_done` event met alle uitvoeringsgegevens.
- Geschikt voor logging, dashboards, statistieken en debugging.

---

## Installatie

1. Plaats het script in:

```text
/config/scripts/
```

2. Voeg het script toe aan `scripts.yaml` of kopieer de inhoud naar je bestaande scripts.

3. Herlaad de scripts via **Developer Tools → YAML Reloading** of herstart Home Assistant.

Daarna is het script beschikbaar als:

```text
script.entityguard
```

---

## Ondersteunde domeinen

EntityGuard werkt onder andere met:

- `light`
- `switch`
- `fan`
- `cover`
- `lock`
- `media_player`
- `scene`
- `climate`
- `vacuum`
- `humidifier`
- `dehumidifier`
- `water_heater`
- `alarm_control_panel`

---

## Parameters

| Parameter | Beschrijving | Standaard |
|-----------|--------------|-----------|
| `description` | Korte omschrijving | – |
| `entity_id` | Doel-entity | – |
| `action` | Uit te voeren service | – |
| `data` | Extra servicegegevens | `{}` |
| `retries` | Maximum aantal pogingen | `3` |
| `timeout` | Timeout per poging | `15` sec |
| `retry_delay` | Wachttijd tussen pogingen | `5` sec |
| `backoff_mode` | `constant`, `linear` of `exponential` | `linear` |
| `state_delay` | Extra wachttijd vóór de statuscontrole | `2` sec |
| `notify` | Verstuur melding bij mislukking | `true` |

---

## Voorbeeld

```yaml
action:
  - action: script.entityguard
    data:
      description: Living room light on
      entity_id: light.erker_licht
      action: light.turn_on
      retries: 3
      timeout: 15
      retry_delay: 5
      backoff_mode: exponential
```

---

## Logging

Na iedere uitvoering wordt het event `entityguard_done` verstuurd. Dit bevat onder andere:

- `description`
- `entity`
- `action`
- `result`
- `duration`
- `attempt`
- `retries`
- `timeout`
- `state_delay`
- `backoff_mode`

Hiermee kun je eenvoudig eigen automatiseringen bouwen of gegevens gebruiken voor dashboards, statistieken of troubleshooting.

---

## Waarom een script?

Ik heb er bewust voor gekozen om EntityGuard als script te bouwen en niet als een volledig verpakte integratie.

Het script is transparant: je kunt precies zien wat er gebeurt, het aanpassen aan je eigen wensen en er tegelijkertijd van leren.
Voor mij sluit deze aanpak aan bij de manier waarop ik Home Assistant graag gebruik: transparant, begrijpelijk en eenvoudig aan te passen.

---

## Download

EntityGuard is verkrijgbaar via Ko-fi.

👉 https://ko-fi.com/s/0a98a2c638

Je ontvangt het complete script, inclusief documentatie met tal van voorbeelden.
Je steun wordt zeer gewaardeerd en helpt me EntityGuard verder te verbeteren en uit te breiden.

---

## Auteur

**Henk Hoekema**

Versie: **v2026.7.3**

---

## Licentie

Je bent vrij om EntityGuard te gebruiken en aan te passen voor eigen gebruik. Gebruik is volledig op eigen risico.




