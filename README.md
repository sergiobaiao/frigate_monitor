# Home Assistant Add-on: Custom App (Template)

A generic add-on wrapper to run *your* app inside Home Assistant OS across multiple hosts, with updates delivered from GHCR.

## How it works
- The add-on image is built from a Home Assistant base image (multi-arch).
- You provide a command (and optional env vars) in the add-on configuration.
- The service wrapper reads `/data/options.json` and starts your app.
- Use `host_network: true` to avoid port mapping headaches across hosts.

## Quick start
1. Fork this repo and edit `addons/custom-app/config.yaml`:
   - Set `version`, `slug`, `name`, and `image: ghcr.io/<your-username>/custom-app-addon-{arch}:<version>`.
2. (Optional) Edit `addons/custom-app/Dockerfile` to install/copy your app.
3. Push to GitHub with `GHCR` permissions enabled, then the workflow will build multi-arch and push images.
4. In Home Assistant → **Add-on Store** → **⋮ → Repositories**: add your repo URL.
5. Install **Custom App (Template)**, open **Configuration**, set `COMMAND`, optional `ENV_VARS`, `WORKDIR`, and **Start**.

## Passing environment variables
- Put key-value pairs as strings in `ENV_VARS`, like `["TZ=America/Sao_Paulo", "API_KEY=..."]`.
- They will be exported before your command runs.

## Logs & troubleshooting
- View logs in the add-on UI (Supervisor → Add-ons → Custom App → **Log**).
- The wrapper echoes the final command before exec.
- If your app binds to a port, ensure it listens on `0.0.0.0` (host networking is enabled).

## Security
- `host_network: true` is enabled by default for simplicity. Tighten if you prefer explicit port mappings.
- Remove any unneeded privileges from `config.yaml` (`NET_ADMIN`, etc.).

---

### Structure

```
repository.json
addons/
  custom-app/
    config.yaml
    build.yaml
    Dockerfile
    rootfs/
      etc/
        services.d/
          app/
            run
        cont-init.d/
          00-tune
.github/
  workflows/
    build-publish.yml
```
