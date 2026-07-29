![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2026%2B-blue.svg)
![Script](https://img.shields.io/badge/Home%20Assistant-Script-success.svg)
![License](https://img.shields.io/badge/License-Community-green.svg)


> [!TIP]
> **EntityGuard documentation is available in two languages.**
>
> 🇬🇧 **English** (current page) • 🇳🇱 **[Nederlandse versie](README.nl.md)**


# 🏠 EntityGuard

### Reliable Home Assistant Script

Over the past months, I developed a small script for Home Assistant. You have to keep yourself busy somehow. 😉

That script is called **EntityGuard**.

Every Home Assistant user has experienced it at some point: you tell a device to do something, but for one reason or another... nothing happens. You tell a light, switch, or fan to turn on, yet it simply ignores the command.

Normally, Home Assistant assumes that once a service call has been sent, the job is done. Whether the device actually performed the requested action is not always verified.

**EntityGuard** changes that.

After every action, the script verifies that the device has actually reached the requested state. If it hasn't, EntityGuard automatically retries the action. It supports multiple retry attempts, configurable delays, and different backoff strategies between attempts.

If the action still fails, EntityGuard can send a notification and records exactly what happened. Thanks to extensive logging, you can easily see which action was executed, how long it took, how many attempts were required, and why it ultimately failed.

EntityGuard supports a wide range of Home Assistant domains, including lights, switches, fans, covers, locks, media players, scenes, and many more. Adding new domains or additional state checks is straightforward.

In short: **EntityGuard makes sure Home Assistant doesn't give up when a device decides to be stubborn.** It is a flexible, highly configurable script designed to make your automations significantly more reliable.

---

# 📑 Contents

* [✨ Features](#-features)
* [📦 Installation](#-installation)
* [🧩 Supported Domains](#-supported-domains)
* [⚙️ Parameters](#️-parameters)
* [🚀 Example](#-example)
* [📡 Logging](#-logging)
* [💡 Use Cases](#-use-cases)
* [👨‍💻 Author](#-author)
* [📄 License](#-license)

---

# ✨ Features

✅ Executes an action on a supported entity

✅ Verifies that the requested state has actually been reached

✅ Automatically retries failed actions

✅ Supports multiple backoff strategies

* Linear
* Constant
* Exponential

✅ Configurable **State Delay**

✅ Optional notification when execution fails

✅ Fires an `entityguard_done` event containing detailed results

✅ Suitable for logging to:

* Google Sheets
* Dashboards
* Grafana
* Custom automations

✅ Compatible with modern Home Assistant Actions (2026+)

---

# 📦 Installation

1. Download `entityguard.yaml` from this repository.

2. Copy the file to:

```text
/config/scripts/
```

3. Add the script to your `scripts.yaml`, or merge its contents into your existing script configuration.

4. Reload your scripts via:

> **Settings → Developer Tools → YAML Configuration Reloading → Scripts**

or simply restart Home Assistant.

After reloading, the script will be available as:

```text
script.entityguard
```

---

# 🧩 Supported Domains

EntityGuard currently supports, among others:

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

Additional Home Assistant domains can easily be added.

---

# ⚙️ Parameters

| Parameter      | Description                              | Default  |
| -------------- | ---------------------------------------- | -------- |
| `description`  | Short description used for logging       | —        |
| `entity_id`    | Target entity                            | —        |
| `action`       | Action to execute (e.g. `light.turn_on`) | —        |
| `data`         | Optional service data                    | `{}`     |
| `retries`      | Maximum retry attempts                   | `3`      |
| `timeout`      | Timeout per attempt                      | `15 sec` |
| `retry_delay`  | Base delay between retries               | `5 sec`  |
| `backoff_mode` | Linear / Constant / Exponential          | `linear` |
| `state_delay`  | Delay before checking the state          | `2 sec`  |
| `notify`       | Send notification if execution fails     | `true`   |

---

# 🚀 Example

```yaml
action:
  - action: script.entityguard
    data:
      description: "Living room light on"
      entity_id: light.erker_licht
      action: light.turn_on
      retries: 3
      timeout: 15
      retry_delay: 5
      backoff_mode: exponential
```

---

# 📡 Logging

After every execution, EntityGuard automatically fires the following event:

```text
entityguard_done
```

The event includes, among others:

| Variable       | Description                     |
| -------------- | ------------------------------- |
| `description`  | Action description              |
| `entity`       | Target entity                   |
| `action`       | Executed Home Assistant action  |
| `result`       | OK or FAILED                    |
| `duration`     | Total execution time            |
| `attempt`      | Successful attempt number       |
| `retries`      | Maximum retries configured      |
| `timeout`      | Configured timeout              |
| `state_delay`  | Delay before state verification |
| `backoff_mode` | Backoff strategy used           |

This information can easily be used for:

* Google Sheets logging
* Dashboards
* Statistics
* Debugging
* Monitoring
* Custom automations

---

# 💡 Use Cases

EntityGuard is especially useful for automations where reliability matters:

* Smart lighting
* Roller shutters
* Door locks
* HVAC systems
* Ventilation
* Alarm systems
* Pumps
* Media equipment
* Mission-critical automations

---

# 💭 Developer's Note

## Why EntityGuard is a script instead of a plug-and-play integration

EntityGuard was intentionally designed as a **Home Assistant script**, not as a plug-and-play integration. Not because an integration isn't possible, but because I believe Home Assistant is more than a collection of ready-made solutions.

To me, Home Assistant is a toolbox rather than an app store. Understanding your own automations is what makes the platform truly powerful.

That's why EntityGuard is built as a tool—not a black box.

With EntityGuard you can:

* see exactly what happens;
* understand the underlying logic;
* modify the script;
* extend it;
* debug it;
* improve it.

A fully packaged integration would hide much of that. That doesn't fit the philosophy behind this project.

---

## AI is a tool, not a substitute for understanding

Modern AI tools such as ChatGPT, Claude, and Copilot are excellent assistants for writing code, explaining YAML, and brainstorming solutions.

But ultimately, AI doesn't know your Home Assistant installation.

It doesn't know:

* how your Zigbee network behaves;
* when your Wi-Fi introduces delays;
* which devices occasionally miss a state update;
* what exceptions exist in your automations;
* or why one particular light always responds just a little slower than the others.

Those insights only come from testing, measuring, tweaking, and trying again.

That's where the real strength of Home Assistant lies.

---

## Why I prefer scripts

A script shows exactly what's happening.

You can inspect every line, understand it, and adapt it to your own setup. In doing so, you learn not only how EntityGuard works, but also how Home Assistant itself works.

A script allows you to:

* understand what's happening;
* troubleshoot problems;
* optimize behavior;
* extend functionality;
* experiment;
* explore new ideas.

It turns you from a Home Assistant user into a Home Assistant builder.

---

## In the end, it's about having fun

For me, Home Assistant is about more than home automation. It's about curiosity.

Experimenting, testing, building, and improving things step by step is simply enjoyable.

EntityGuard was created with exactly that mindset.

Not as a closed solution that does everything for you, but as a tool you can understand, customize, and continue to develop.

I hope it not only helps you build more reliable automations, but also inspires you to keep experimenting yourself.

---

# 👨‍💻 Author

**Henk Hoekema**

**Version**

`v2026.7.3`

[<img src="coffee.png" width="300">](https://paypal.me/henkhoekema)
[![Venmo](https://img.shields.io/badge/Venmo-Supported-blue?style=for-the-badge)](https://paypal.me/henkhoekema)


---

# 🤝 Contributing

Home Assistant thrives because people build, learn, and share together. I certainly don't have all the answers, so ideas, improvements, bug reports, and pull requests are always welcome.

---

# 📄 License

Free to use, but entirely at your own risk.
