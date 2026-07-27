🏠 EntityGuard
Home Assistant Script Blueprint
Een krachtige, betrouwbare script‑blueprint voor Home Assistant die acties uitvoert op entities
en controleert of de gewenste status daadwerkelijk wordt bereikt.
Inclusief retries, backoff‑logica, state‑delay, notificaties en uitgebreide logging.

✨ Functionaliteit
Voert een actie uit op elke ondersteunde entity
Controleert of de gewenste eindstatus bereikt wordt
Probeert meerdere keren bij mislukking
Ondersteunt linear, constant en exponential backoff
Wacht op vertraagde statusupdates
Stuurt optioneel een notificatie bij mislukking
Logt alle resultaten via een event
Werkt met alle moderne Home Assistant actions (2026+)

🧩 Ondersteunde domeinen
  switch
  light
  input_boolean
  fan
  group
  homeassistant
  scene
  media_player
  cover
  lock
  vacuum
  climate
  humidifier
  dehumidifier
  water_heater
  alarm_control_panel

⚙️ Parameters
Parameter	       Beschrijving	                                        Default
description	     Korte omschrijving voor logging	—
entity_id	       Entity waarop de actie wordt uitgevoerd	—
action	         Uit te voeren actie (bijv. light.turn_on)	—
data	           Optionele service‑data	{}
retries	         Aantal pogingen	                                    3
timeout	         Maximale wachttijd per poging	                      15 sec
retry_delay	     Basis wachttijd tussen pogingen	                    5 sec
backoff_mode	   Wachttijd‑methode	linear
state_delay	     Wacht na actie voordat status wordt gecontroleerd	  2 sec
notify	         Notificatie bij mislukking	                          true


📥 Installatie
Via URL
Gebruik de link naar dit bestand in Home Assistant:
  Instellingen → Automatiseringen & Scènes → Blueprints → Importeren → URL

Via YAML‑upload
  Download entityguard.yaml en importeer via:

Instellingen → Automatiseringen & Scènes → Blueprints → Importeren → YAML‑upload

🚀 Voorbeeld‑gebruik
yaml
action:
  - action: script.entityguard
    data:
      description: "Lamp aanzetten"
      entity_id: light.erker_licht
      action: light.turn_on
      retries: 3
      backoff_mode: exponential
📡 Logging
Elke uitvoering stuurt een event:
  entityguard_klaar

Met o.a.:
  entity
  action
  resultaat
  duur
  poging
  backoff‑mode
  state_delay
  Ideaal voor dashboards, Google Sheets of debugging.

🧑‍💻 Auteur
Blueprint gemaakt door Henk Hoekema
Home Assistant (NL) community

📄 Licentie
Vrij te gebruiken binnen de Home Assistant‑community.
