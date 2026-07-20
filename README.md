# Minecraft Fabric Server — Create & Create Aeronautics

Dockerised Fabric server pre-configured for the **Create** mod ecosystem
from [Modrinth](https://modrinth.com).

## Quick start

```bash
# 1. Accept the Minecraft EULA (required)
echo "eula=true" > data/eula.txt

# 2. Start the server
docker compose up -d

# 3. Watch logs
docker compose logs -f
```

The server will automatically download Fabric, Fabric API, Create, and the
listed optimisation / QoL mods on first launch.  All data lives in `./data/`.

## Included mods

| Mod | Purpose |
|---|---|
| [Fabric API](https://modrinth.com/mod/fabric-api) | Required modding framework |
| [Create (Fabric)](https://modrinth.com/mod/create-fabric) | Mechanical engineering & contraptions |
| [Create: Dreams & Desires](https://modrinth.com/mod/create-dreams-and-desires) | Create add-on (quality-of-life blocks) |
| [Create: Steam 'n' Rails](https://modrinth.com/mod/create-steam-n-rails) | Create add-on (trains & monorails) |
| [Sodium](https://modrinth.com/mod/sodium) | Rendering engine (performance) |
| [Iris](https://modrinth.com/mod/iris) | Shader support |
| [Lithium](https://modrinth.com/mod/lithium) | Server-side optimisations |
| [FerriteCore](https://modrinth.com/mod/ferrite-core) | Memory usage reduction |
| [Fabric Language Kotlin](https://modrinth.com/mod/fabric-language-kotlin) | Kotlin runtime (Create dependency) |

## What players need to install

**Yes — every player must install the content mods on their own PC.**
Players who join without them will be kicked with a mod mismatch error.

The easiest way is to use a launcher that supports Modrinth packs, such as
[Prism Launcher](https://prismlauncher.org/) or [ATLauncher](https://atlauncher.com/).

### Required (every player)

1. Install **Fabric Loader** for the matching Minecraft version
2. Drop these `.jar` files into `%APPDATA%\.minecraft\mods\`:

| Mod | Download |
|---|---|
| Fabric API | [modrinth.com/mod/fabric-api](https://modrinth.com/mod/fabric-api) |
| Create (Fabric) | [modrinth.com/mod/create-fabric](https://modrinth.com/mod/create-fabric) |
| Fabric Language Kotlin | [modrinth.com/mod/fabric-language-kotlin](https://modrinth.com/mod/fabric-language-kotlin) |
| Create Aeronautics | See [Adding Create Aeronautics](#adding-create-aeronautics) |

> **Create add-ons** (Steam 'n' Rails, Dreams & Desires) — if present on the
> server, players must also install them.  Remove them from `MODRINTH_PROJECTS`
> if you want to keep the client-side list minimal.

### Recommended (FPS boost, optional)

| Mod | Download |
|---|---|
| Sodium | [modrinth.com/mod/sodium](https://modrinth.com/mod/sodium) |
| Iris | [modrinth.com/mod/iris](https://modrinth.com/mod/iris) |
| Lithium | [modrinth.com/mod/lithium](https://modrinth.com/mod/lithium) |

### Server-only (players do NOT need these)

Lithium and FerriteCore only run on the server — players should **not** install them
(they're ignored client-side anyway).

## Adding Create Aeronautics

Create Aeronautics is in active development.  To add it once a stable Modrinth
release is available, append its slug to `MODRINTH_PROJECTS` in
`docker-compose.yml`:

```yaml
MODRINTH_PROJECTS: |
  fabric-api,
  create-fabric,
  create-dreams-and-desires,
  create-steam-n-rails,
  create-aeronautics,          # <-- add this line
  sodium,
  iris,
  lithium,
  ferrite-core,
  fabric-language-kotlin
```

Alternatively, drop the `.jar` into `./data/mods/` and restart:

```bash
cp ~/Downloads/create-aeronautics-*.jar ./data/mods/
docker compose restart
```

## Configuration

- Edit server properties in `./data/server.properties` after the first run.
- Adjust RAM by changing `MEMORY` in `docker-compose.yml` (or `.env`).
- Manage OPs / whitelist via the container console:

```bash
docker compose exec minecraft rcon-cli
```

## Ports

| Port | Protocol | Purpose |
|---|---|---|
| 25565 | TCP | Minecraft Java game traffic |
| 24454 | UDP | Create Aeronautics physics sync (client ↔ server) |

> Open these ports in your firewall / router if friends connect from outside
> your local network.

## Useful commands

```bash
docker compose up -d        # Start in background
docker compose down         # Stop
docker compose logs -f      # Follow logs
docker compose exec minecraft rcon-cli   # RCON console
docker compose restart      # Restart after adding mods
```
