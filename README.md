![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2026%2B-blue.svg)
![Script](https://img.shields.io/badge/Home%20Assistant-Script-success.svg)
![License](https://img.shields.io/badge/License-Community-green.svg)


> [!TIP]
> **EntityGuard documentation is available in two languages.**
>
> 🇬🇧 **English** (current page) • 🇳🇱 **[Nederlandse versie](Readme.nl.md)**


# EntityGuard

EntityGuard is a Home Assistant script that I developed to make service calls more reliable.

While building automations, I kept running into the same issue: Home Assistant sends a command to a device, but it doesn't verify whether the device actually performed the requested action. Sometimes a light stays off, a fan doesn't start, or a cover simply doesn't move.

EntityGuard solves that problem.

After every action, the script checks whether the desired state has been reached. If not, it automatically retries the action. You can configure the number of retries, the delay between attempts, and the backoff strategy. If the action ultimately fails, EntityGuard can send a notification and log all execution details for further analysis.

The script supports many Home Assistant domains and is easy to extend.

---

## Features

- Verifies that a device actually reached the requested state.
- Automatically retries failed actions.
- Supports multiple backoff strategies (`constant`, `linear`, and `exponential`).
- Can send a notification when an action ultimately fails.
- Fires an `entityguard_done` event containing execution details.
- Ideal for logging, dashboards, statistics, and debugging.

---

## Installation

1. Place the script in:

```text
/config/scripts/
```

2. Add it to your `scripts.yaml` file or copy the contents into your existing scripts.

3. Reload your scripts via **Developer Tools → YAML Reloading**, or restart Home Assistant.

The script will then be available as:

```text
script.entityguard
```

---

## Supported Domains

EntityGuard works with many Home Assistant domains, including:

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

| Parameter | Description | Default |
|-----------|-------------|---------|
| `description` | Short description of the action | – |
| `entity_id` | Target entity | – |
| `action` | Service to execute | – |
| `data` | Optional service data | `{}` |
| `retries` | Maximum number of retry attempts | `3` |
| `timeout` | Timeout per attempt | `15` sec |
| `retry_delay` | Delay between retry attempts | `5` sec |
| `backoff_mode` | `constant`, `linear`, or `exponential` | `linear` |
| `state_delay` | Extra delay before state verification | `2` sec |
| `notify` | Send notification on failure | `true` |

---

## Example

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

After every execution, EntityGuard fires an `entityguard_done` event containing information such as:

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

This makes it easy to build your own automations or use the data for dashboards, statistics, or troubleshooting.

---

## Why a Script?

I deliberately chose to build EntityGuard as a script rather than a fully packaged integration.

The script is transparent: you can see exactly what it does, adapt it to your own setup, and learn from it at the same time. For me, this approach fits the way I like to use Home Assistant: transparent, easy to understand, and easy to modify.

---

## Download

With EntityGuard, I hope to contribute not only to the Home Assistant community, but also to a good cause. Instead of charging a fixed price, I simply ask for a small donation through Ko-fi. All proceeds are donated to the Dutch Cancer Society (KWF Kankerbestrijding). This repository contains the project's documentation, examples, and issue tracker.

👉 https://ko-fi.com/s/ab65ae7595

You'll receive the complete script, including documentation with plenty of examples.

---

## Author

**Henk Hoekema**

Version: **v2026.7.3**

---

## License

You are free to use and modify EntityGuard for personal use. Use it at your own risk.


