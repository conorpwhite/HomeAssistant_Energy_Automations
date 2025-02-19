# This is a newly created Repository in only Draft status. Code is not tested. #
Inspiration for this work is based on this blog post, https://www.auroranrunner.com/2024/03/18/home-assistant-heat-pump-automation-with-cheap-spot-hours-and-github-copilot-doing-the-work and related github confirations here. https://github.com/elsonico/Utilities/tree/master/HomeAssistant

# Home Assistant Energy Automations

This repository contains configuration files for Home Assistant to support automation based on Nordpool energy spot prices. The automation includes:
- Steering heating and cooling appliances using SmartThings integration for use with e.g. Samsung heatpumps
- Steering lighting using HomeKit integrated devices also works with IKEA Home Smart connected devices via Homekit integration

## Folder Structure
- `configuration.yaml`: Main configuration file for Home Assistant.
- `input_text.yaml`: Defines input text variables for Nordpool configuration.
- `configuration/nordpool/`: Contains the configuration for Nordpool energy spot prices.
- `configuration/smartthings/`: Contains the configuration for SmartThings integration.
- `configuration/homekit/`: Contains the configuration for HomeKit integration.

## Getting Started
1. Clone the repository.
2. Copy the configuration files to your Home Assistant `config` directory.
3. Update the configurations with your specific details.
4. Restart Home Assistant to apply the configurations.

## Configuration Files
### configuration.yaml
```yaml
# Define variables for Nordpool configuration
nordpool:
  region: "SE4"
  currency: "SEK"

# Include other configuration files
input_text: !include configuration/input_text.yaml
sensor: !include_dir_merge_list sensors/
automation: !include_dir_merge_list automations/

# Include the Nordpool configuration
sensor: !include configuration/nordpool/nordpool.yaml
```

### input_text.yaml
```yaml
input_text:
  nordpool_region:
    name: Nordpool Region
    initial: "SE4"
  nordpool_currency:
    name: Nordpool Currency
    initial: "SEK"
```

### nordpool.yaml
```yaml
sensor:
  - platform: nordpool
    region: "{{ region | default('SE4') }}"
    currency: "{{ currency | default('SEK') }}"
    price_type: "kWh"
    low_price_cutoff: 0.1
    precision: 3
    VAT: true

automation:
  - alias: "Adjust thermostat based on Nordpool price"
    trigger:
      - platform: time_pattern
        minutes: "/5"
    condition:
      - condition: numeric_state
        entity_id: sensor.nordpool_kwh_sek_se4
        below: 0.1
    action:
      - service: climate.set_temperature
        target:
          entity_id: climate.your_thermostat
        data:
          temperature: 21
```

### smartthings.yaml
```yaml
smartthings:
  token: YOUR_SMARTTHINGS_TOKEN

automation:
  - alias: "Turn on heating when price is low"
    trigger:
      - platform: numeric_state
        entity_id: sensor.nordpool_kwh_sek_se4
        below: 0.05
    action:
      - service: switch.turn_on
        target:
          entity_id: switch.heating
  - alias: "Turn off heating when price is high"
    trigger:
      - platform: numeric_state
        entity_id: sensor.nordpool_kwh_sek_se4
        above: 0.05
    action:
      - service: switch.turn_off
        target:
          entity_id: switch.heating
```

### homekit.yaml
```yaml
homekit:
  filter:
    include_domains:
      - light

automation:
  - alias: "Adjust lighting based on Nordpool price"
    trigger:
      - platform: numeric_state
        entity_id: sensor.nordpool_kwh_sek_se4
        below: 0.05
    action:
      - service: light.turn_on
        target:
          entity_id: light.living_room
  - alias: "Dim lights when price is high"
    trigger:
      - platform: numeric_state
        entity_id: sensor.nordpool_kwh_sek_se4
        above: 0.05
    action:
      - service: light.turn_off
        target:
          entity_id: light.living_room
```

### .gitignore
```plaintext
# Ignore Home Assistant database files
*.db
*.db-shm
*.db-wal

# Ignore Home Assistant log files
home-assistant.log
home-assistant.log.*

# Ignore Home Assistant configuration testing artifacts
*.yaml.save
```
```` ▋
