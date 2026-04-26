# PZServerManager

Docker Compose stack for hosting and managing a Project Zomboid dedicated server.
Includes a Go management API, a React SPA, and a SteamCMD-based game server image.

## Install

**Prerequisites:** Docker with Compose plugin, Git.

```sh
git clone --recurse-submodules git@github.com:FakeApate/PZServerManager.git
cd PZServerManager
```

If you cloned without `--recurse-submodules`:

```sh
git submodule update --init --recursive
```

## Usage

Copy and fill in the backend config before the first run:

```sh
cp pzsm/config.example.yaml pzsm/config.yaml
# Edit pzsm/config.yaml -- set steam_web_api_key and steam_collection_id at minimum
```

Start the full stack:

```sh
docker compose up --build
```

The UI is available at `http://localhost:5173`.

To run only the backend and UI (skips the multi-GB game server build):

```sh
docker compose up -d pzsm pzsmui
```

## Configuration

All backend configuration lives in `pzsm/config.yaml` (gitignored, created from
`pzsm/config.example.yaml`). See that file for the full field reference.

The minimum required fields are:

| Field | Description |
|-------|-------------|
| `steam_web_api_key` | Steam Web API key (https://steamcommunity.com/dev/apikey) |
| `steam_collection_id` | Numeric ID of the Steam Workshop collection to manage |
| `database_path` | Path to the SQLite file inside the container |

RCON fields (`rcon_host`, `rcon_password`) are required for the players, world,
console, and whitelist pages. Leave them empty to disable RCON.

## Development

See `pzsm/README.md` (Go backend) and `pzsmui/README.md` (React frontend) for
per-service development instructions.

Useful shortcuts:

```sh
docker compose logs -f pzsm          # backend logs
docker compose logs -f pzsmui        # frontend/nginx logs
```

Auth in development requires `-tags devbypass` on the backend and `dev_user_email`
set in `config.yaml`. See `CLAUDE.md` for the full auth model.

## Ports

| Port | Protocol | Service |
|------|----------|---------|
| 5173 | TCP | pzsmui (nginx, UI) |
| 16261 | UDP | pzserver (game, primary) |
| 16262 | UDP | pzserver (game, secondary) |
| 27015 | TCP | pzserver (RCON) |

## License

MIT License. See [LICENSE](LICENSE).
