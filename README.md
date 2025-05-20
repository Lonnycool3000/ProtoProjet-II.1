# ProtoProjet-II.1

A Node-RED based management system for controlling and monitoring a Minecraft server with both Discord integration and physical hardware controls.

## Overview

Crafty_Bot is a comprehensive solution to manage a Minecraft server, providing:

- Real-time server status monitoring in Discord
- Interactive Discord buttons for server control (start/stop/restart)
- LCD display showing server information
- LED indicators for player count
- Physical buttons for server control

The system is built using Node-RED and runs on a Raspberry Pi, interfacing with a Minecraft server managed through Crafty Controller.

## Components

### 1. Discord Interface (Crafty_Bot 01)

- Posts a server status embed message to a Discord channel
- Updates status every 5 seconds with:
  - Server name, IP and port
  - Running state
  - CPU and memory usage
  - Player count and player list
  - World name and version
- Interactive buttons for:
  - Starting the server when offline
  - Stopping the server when online
  - Restarting the server when online

### 2. Button Handler (Crafty_Bot 02)

- Processes button interactions from Discord
- Sends API requests to Crafty Controller to control the server
- Provides feedback messages about operation status
- Has physical button integration via GPIO pins

### 3. Hardware Components

- 16x2 LCD display showing:
  - Server name (line 1)
  - Server status and player count (line 2)
- LED player count indicator (8 LEDs representing 0-8 players)
- Physical control buttons for:
  - Starting the server (GPIO pin 19)
  - Stopping the server (GPIO pin 16)
- LCD backlight toggle button (GPIO pin 21)

## API Integration

The system connects to a Minecraft server management API at `minecraft.lennygodart.org` using the following endpoints:

- `/api/v2/servers/{server-id}/stats` - Get server statistics
- `/api/v2/servers/{server-id}/action/start_server` - Start server
- `/api/v2/servers/{server-id}/action/stop_server` - Stop server
- `/api/v2/servers/{server-id}/action/restart_server` - Restart server

## Setup Requirements

- Raspberry Pi with Node-RED installed
- Discord bot token with proper permissions
- Crafty Controller API token
- 16x2 LCD display (I2C interface)
- 8 LEDs connected to GPIO pins
- 3 push buttons connected to GPIO pins

## GPIO Pin Mapping

### Inputs
- GPIO 16: Stop Server Button
- GPIO 19: Start Server Button
- GPIO 21: LCD Backlight Toggle Button

### Outputs (LED Player Count)
- GPIO 12: LED 8
- GPIO 13: LED 7
- GPIO 14: LED 6
- GPIO 15: LED 5
- GPIO 22: LED 4
- GPIO 17: LED 3
- GPIO 18: LED 2
- GPIO 27: LED 1

## Python Scripts

The system calls the following Python scripts for LCD control:
- `/home/lenny/digilab/lcd/init.py` - Initialize and clear the LCD
- `/home/lenny/digilab/lcd/backlight.py` - Control LCD backlight
- `/home/lenny/digilab/lcd/write.py` - Write text to LCD screen

## Installation

1. Import the Node-RED flows into your Node-RED instance
2. Configure Discord credentials in the Discord nodes
3. Update the API token in the HTTP request nodes
4. Connect the hardware components to the appropriate GPIO pins
5. Ensure Python LCD scripts are in place
6. Deploy the flows

## Flows

### Crafty_Bot 01
Handles server status monitoring, Discord status updates, LCD display, and LED indicators.

### Crafty_Bot 02
Manages button interactions from Discord and physical buttons.

## Security Note

The API tokens in the flow files should be replaced with your own tokens before deploying.
