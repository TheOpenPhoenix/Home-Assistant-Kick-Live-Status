# Kick Live Status

[![Version](https://img.shields.io/badge/dynamic/yaml?url=https%3A%2F%2Fraw.githubusercontent.com%2FTheOpenPhoenix/Home-Assistant-Kick-Live-Status%2Fmain%2Fkick_live_status%2Fconfig.yaml&query=%24.version&label=version&color=blue)](https://github.com/TheOpenPhoenix/Home-Assistant-Kick-Live-Status/blob/main/kick_live_status/CHANGELOG.md)
![Supports aarch64 Architecture](https://img.shields.io/badge/aarch64-yes-green.svg)
![Supports amd64 Architecture](https://img.shields.io/badge/amd64-yes-green.svg)
[![Home Assistant Add-on](https://img.shields.io/badge/Home%20Assistant-Add--on-41BDF5?logo=homeassistant&logoColor=white)](https://www.home-assistant.io/addons/)
[![License: MIT](https://img.shields.io/github/license/TheOpenPhoenix/Home-Assistant-Kick-Live-Status)](https://github.com/TheOpenPhoenix/Home-Assistant-Kick-Live-Status/blob/main/kick_live_status/LICENSE)
[![Open your Home Assistant instance and show the add add-on repository dialog with this repository pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2FTheOpenPhoenix/Home-Assistant-Kick-Live-Status)

> **Disclaimer:** This is a third-party community add-on and is not affiliated with, endorsed by, or associated with Kick.com.

Monitor Kick.com streamers and expose their live status as sensors in Home Assistant.

## How it works

This add-on polls the Kick.com API at a configurable interval and creates a `binary_sensor` entity for each monitored channel. The sensor turns **ON** when the streamer is live and **OFF** when they're offline.

## Configuration

| Option | Description | Default |
|--------|-------------|---------|
| `client_id` | Your Kick.com app Client ID | (required) |
| `client_secret` | Your Kick.com app Client Secret | (required) |
| `channels` | List of Kick channel slugs to monitor | `[]` |
| `scan_interval` | Polling interval in seconds (30–300) | `60` |
| `max_backoff` | Maximum exponential backoff interval in seconds | `43200` (12 hours) |

### Getting your Kick API credentials

1. Go to [kick.com/settings/developer](https://kick.com/settings/developer)
2. Create a new app (or use an existing one)
3. Copy the **Client ID** and **Client Secret**

## Entities

For each channel in your list, the add-on creates:

**`binary_sensor.kick_<channel_slug>`**

| Attribute | Description |
|-----------|-------------|
| `stream_title` | Current stream title |
| `viewer_count` | Number of current viewers |
| `category` | Stream category (e.g. "Grand Theft Auto V") |
| `thumbnail` | Stream thumbnail URL |
| `started_at` | Stream start time (ISO 8601) |
| `channel_url` | Direct link to the Kick channel |

## Example automations

```yaml
# Send a notification when a streamer goes live
automation:
  - alias: "Kick streamer went live"
    trigger:
      - platform: state
        entity_id: binary_sensor.kick_<your_channel>
        to: "on"
    action:
      - service: notify.mobile_app
        data:
          title: >
            🟢 {{ state_attr(trigger.entity_id, 'friendly_name') }} is LIVE!
          message: >
            {{ state_attr(trigger.entity_id, 'stream_title') }}
```
