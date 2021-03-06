<a href="https://paypal.me/miry">
  <img align="right" alt="Donate with PayPal" style="float:right" width="200" src="https://raw.githubusercontent.com/stefan-niedermann/paypal-donate-button/master/paypal-donate-button.png">
</a>

[![patrons](http://img.shields.io/liberapay/goal/miry.svg?logo=liberapay)](https://liberapay.com/miry/)
[![Gitter](https://badges.gitter.im/Lords-and-Knights-Bot/community.svg)](https://gitter.im/Lords-and-Knights-Bot/community)

# Online Games Bot Engine

A simple bot for auto-building and gain resources via missions. Tested on **Linux** and **MacOS**. Available a container version to run on different platforms.
Supports the "Lords and Knights" 2020 version.

# Lords and Knights

Website: https://lordsandknights.com
Naive bot solution to optimize your nights and days.
Spent more time on what is matters than do a routine job.

## Features

### Automate buildings

Do continuous buildings in any order that you specify. Feel free to use my strategy from [config/servers.yml.example](config/servers.yml.example).
Use Free speedup building if it is available.

### Research

Do research in **Library** or **University**. Dumb approach - the first available research.

### Missions

Continuously send troops to missions. **WARNING**: Disable it manually, if you have a war or require forces for something.

## Quick Usage

Run the bot on your machine or in cloud, you should [install Docker](https://docs.docker.com/install/).
The container has all required packages to run bots: Chrome and Ruby environment.

Provide settings via environment variable `SERVERS_JSON`.

```shell
$ export SERVERS_JSON=$(cat <<JSON
{
  "miry_de": {
    "timeout": 2,
    "email": "<your@example.com>",
    "password": "<your password>",
    "server_url": "https://lordsandknights.com",
    "server_name": "Germanien III (DE) - empfohlen",
    "actions": [
      "build_first",
      "send_troops_to_missions",
      "research"
    ],
    "buildings": [
      "Sawmill",
      "Stonecutter",
      "Forge",
      {
        "Ore store": {
          "level": 3
        }
      },
      {
        "Wood store": {
          "level": 3
        }
      },
      {
        "Stone store": {
          "level": 3
        }
      },
      "Lumberjack",
      "Quarry",
      "Ore mine",
      "Library",
      "Wood store",
      "Stone store",
      "Ore store",
      {
        "Tavern": {
          "level": 4
        }
      },
      "Farm",
      "Arsenal",
      "Market",
      "Fortifications",
      "Wood Storage",
      "Stone Storage",
      "Ore Storage",
      "Townhall",
      "Barracks",
      "University",
      "Fortress Wall",
      "Marketplace",
      "Tavern Quarter"
    ]
  }
}
JSON
$ docker run -e SERVERS_JSON="${SERVERS_JSON}" miry/online_games_bot make run.daemon
```

> HINT: You can use `docker run` option `--env-file <file>`, where you can store environment variables.

The process would run forewer. When there is some problem with bot, it would stop for 10 minutes, and then starts again.
It is good, if you loged in in same time and do modifications onself.

## Setup environment for MacOS

1. Make an account for docker, after you’ve done that download docker. (https://docs.docker.com/v17.12/docker-for-mac/install/) Make sure to pick the stable version.
2. Make sure you have downloaded the bot. (https://github.com/miry/online_games_bots) Have the folder on the desktop to make it easy to find.
3. Open the bot folder, click the config folder and open (servers.yml) with your preferred text editor and fill it out with the correct information. After you’ve done that save the file and close it. (If you don’t have a text editor I recommend Atom) (https://atom.io/)
4. Go to your terminal and follow theses steps.
5. cd desktop (Press enter)
6. cd online_games_bots-master/ (shortcut key after you typed online press tab it should fill out the name) (press enter)
7.  docker run -e LOG_LEVEL=debug -v $(pwd)/config:/app/config -v $(pwd)/tmp:/app/tmp miry/online_games_bot bundle exec ruby runner.rb (Press enter)
8. If you need help please join the discord. (https://discord.gg/VaEtdbC)

## Settings

The config file located in `config/servers.yml`. The structure is simple. First you should specify the custom server name example `lak_us` and bot.

```yaml
lak_us:
  :bot: lords_and_kinghts_v3
```

Now we know what kind of game you want to connect. For Lords and Knights we need to specify the server.

```yaml
lak_us:
  :bot: lords_and_kinghts_v3
  :server_name: Germanien III (DE) - empfohlen
  :server_url: http://lordsandknights.com
```

- `server_name` is the text name that you can copy from the login page after you inpuit the credentials.
- `server_url` is the login url

### Actions

- `build_first` - Create a first building in the list. The order of buidlings get from `:buildings`
- `send_troops_to_missions` - Send troops to missions for each castle.
- `research` - Research 1 topic in Library or University. If there is research in progress, does not add a new to the queue.
- `send_troops_from_all_castles` - Send troops to missions cross all castles and fortress (faster than `send_troops_to_missions`). Requires Mass Functions enabled after you conquer few castles.
- `events` - Collect the prizes for completed events.
Example:

```yaml
de3:
  :email: user@example.com
  :password: securepassword
  :server_name: Germanien III (DE) - empfohlen
  :actions:
    - build_first
    - send_troops_to_missions
    - research
    - events
```

### Buildings

Setup a list allowed to build buildings and the order.
Depends on ui version it could be different number for buildings.

```yaml
miry_us:
  :email: user@mailinator.com
  :password: securepassword
  :bot: lords_and_kinghts_v3
  :server_name: Germanien III (DE) - empfohlen
  :server_url: http://lordsandknights.com
  :actions:
    - build_first
    - send_troops_to_missions
    - research
  :buildings:
    - Quarry
    - Lumberjack
    - Ore store
    - Wood store
    - Stone store
    - Ore mine
    - Keep
    - Farm
    - Fortifications
    - Arsenal
    - Library
    - Tavern
    - Market
```

You can specify max required level for building. It gives more granular upgrades and provides different kind of strategies.

```yaml
:buildings:
  - Ore store:
      level: 3
  - Wood store:
      level: 3
  - Stone store:
      level: 3

  - Lumberjack:
      level: 5
  - Quarry:
      level: 5
  - Ore mine:
      level: 5

  - Library:
      level: 2

  - Lumberjack:
      level: 6
  - Quarry:
      level: 6
  - Ore mine:
      level: 6
  - Ore store
  - Wood store
  - Stone store
```

Check `config/servers.yml.example` for more options and strategies.

## Logging

To increase log level, you need to specify the `LOG_LEVEL` env variable. Possible values: debug, info, warn, error


# Alternatives

- [LoK: War Bot](https://lordsandknightsbot.com/)
