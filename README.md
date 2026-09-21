# Windows in Docker

**Languages:** [English](README.md) · [Українська](README.uk.md)

> By [Boryslav Deputat](https://github.com/boryslavdeputat) - Cloud / SRE / Platform.
> Built with **KLAV (UA AI) / КЛАВ (УКР ШІ)**.
> Sites: [Portfolio](https://boryslavdeputat.com/) · [ClawDBot](https://clawdbot.llc/) · [AI hub](https://boryslavdeputat.github.io/boryslavdeputat/)

Run a full Windows installation inside a Docker container, with KVM
hardware acceleration and a browser-based viewer (noVNC). Built on top of
[dockur/windows](https://github.com/dockur/windows).

On first start it downloads the selected Windows version directly from
Microsoft and installs it unattended — no ISO or product key required to get
started.

## Requirements

- Linux host with `/dev/kvm` available (check with `ls -la /dev/kvm`)
  - If you're running this inside a VM (Hyper-V, Proxmox, etc.), you need
    **nested virtualization** enabled on that VM.
- Docker + Docker Compose

## Usage

```bash
cp .env.example .env
# edit .env and set WINDOWS_PASSWORD (required) and anything else you want
docker compose up -d
```

Then open:
- **Browser viewer:** `http://<host>:8006`
- **RDP:** `<host>:3389` (username: `Docker`, or whatever you set in `.env`)

## Configuration

All settings are in `.env` (copy from `.env.example`):

| Variable | Default | Description |
|---|---|---|
| `WINDOWS_VERSION` | `11` | Windows version — see [dockur/windows versions](https://github.com/dockur/windows#how-do-i-select-a-windows-version) |
| `WINDOWS_USERNAME` | `Docker` | Windows account username |
| `WINDOWS_PASSWORD` | *(required)* | Windows account password |
| `WINDOWS_LANGUAGE` | `English` | Windows install language |
| `WINDOWS_DISK_SIZE` | `64G` | Virtual disk size |
| `WINDOWS_RAM_SIZE` | `4G` | RAM allocated to the VM |
| `WINDOWS_CPU_CORES` | `2` | CPU cores allocated to the VM |

Without `/dev/kvm` the container still runs, just via slow software
emulation (roughly 10x slower) — you'll see a warning in the logs.

## Data persistence

The VM's disk and state live under `./data` on the host, so
`docker compose down` / `up` preserves everything.

## License

This repo just packages [dockur/windows](https://github.com/dockur/windows)
as a docker-compose file — see that project for the underlying license and
details on how it works.
