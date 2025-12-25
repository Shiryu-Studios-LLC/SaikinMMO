# SaikinMMO (umbrella)

Monorepo wrapper for the Saikin MMO project. This root repo is meant to hold submodules so a single `git clone --recursive` fetches the Unreal game client and the .NET backend together.

## Layout
- `Saikin/` — Unreal Engine 5.7 game project (submodule)
- `DimensionalRift/` — .NET 9 microservices backend (submodule)
- `unreal-mcp/` — MCP helper project (optional, currently local)
- `Documentation/` — design docs
- `Resoruces/` — art/FBX assets

## Cloning
```bash
git clone --recursive https://github.com/Shiryu-Studios-LLC/SaikinMMO.git
```
If you already cloned without submodules:
```bash
git submodule update --init --recursive
```

## Submodule workflow
- Pull latest: `git pull --recurse-submodules`
- Update a submodule to its main branch: `git submodule update --remote Saikin`
- Commit submodule bumps from the root repo so everyone gets the pinned hashes.

## Builds
### Backend
```bash
cd DimensionalRift
docker-compose up --build
```
Gateway: http://localhost:8080 (8443 if you enable HTTPS)

### Unreal client
```bash
"C:\Program Files\EG\UE_5.7\Engine\Build\BatchFiles\Build.bat" SaikinEditor Win64 Development -Project="H:\UEProjects\SaikinMMO\Saikin\Saikin.uproject"
```

## Notes
- Use Git LFS for large assets (FBX/PNG/EXR/etc.) to keep clones lean.
- Keep secrets out of the repo (`.env`, `secrets/`, `vault/data` are ignored).
- Add plugin dependencies to `.uplugin`/`.uproject` files when warnings appear to avoid build noise.
