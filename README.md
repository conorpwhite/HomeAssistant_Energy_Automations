# Home Assistant Energy Automations

This repository contains configuration files for Home Assistant to support automation based on Nordpool energy spot prices. The automation includes:
- Steering heating and cooling appliances using SmartThings integration for use with e.g. Samsung heatpumps
- Steering lighting using HomeKit integrated devices also works with IKEA Home Smart connected devices via Homekit integration

## Folder Structure
- `configuration/nordpool/`: Contains the configuration for Nordpool energy spot prices.
- `configuration/smartthings/`: Contains the configuration for SmartThings integration.
- `configuration/homekit/`: Contains the configuration for HomeKit integration.

## Getting Started
1. Clone the repository.
2. Copy the configuration files to your Home Assistant `config` directory.
3. Update the configurations with your specific details.
4. Restart Home Assistant to apply the configurations.

## Configuration Files
- `nordpool.yaml`: Configuration for Nordpool energy spot prices.
- `smartthings.yaml`: Configuration for SmartThings integration.
- `homekit.yaml`: Configuration for HomeKit integration.
