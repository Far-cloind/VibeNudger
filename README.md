# VibeNudger

VibeNudger is a multi-repository project for monitoring Windows audio session levels and forwarding them to wearable devices.

VibeNudger 是一个多仓库项目，用于采集 Windows 音频会话电平，并将结果转发到可穿戴设备端展示。

## Stack

`VibeNudger-win` -> `VibeNudger-plugin` -> `VibeNudger-vela`

- `VibeNudger-win`: Windows desktop monitor built with C# and NAudio. It samples audio-session levels and exposes a local HTTP IPC service.
- `VibeNudger-plugin`: AstroBox plugin that polls the local IPC service and forwards audio-level messages to the wearable side.
- `VibeNudger-vela`: Xiaomi Vela quick app that receives forwarded messages and renders level, waveform, and optional vibration feedback.

## Repositories

- [VibeNudger-win](https://github.com/Far-cloind/VibeNudger-win): Windows audio-session monitor and IPC service
- [VibeNudger-plugin](https://github.com/Far-cloind/VibeNudger-plugin): AstroBox bridge plugin
- [VibeNudger-vela](https://github.com/Far-cloind/VibeNudger-vela): Xiaomi Vela wearable app

## Architecture

1. `VibeNudger-win` monitors the default render device and exposes local endpoints such as `GET /api/level` and `GET /api/selected`.
2. `VibeNudger-plugin` polls `http://127.0.0.1:38421` and converts the sampled data into wearable-facing messages.
3. `VibeNudger-vela` receives those messages on-device and displays audio level, state, waveform, and optional vibration feedback.

## Quick Start

### 1. Run the Windows host

In `VibeNudger-win`:

```powershell
dotnet run --project .\VibeNudger.App\
```

### 2. Build and install the AstroBox plugin

In `VibeNudger-plugin`:

```bash
pnpm install
pnpm build
```

Copy the generated `dist` output into your AstroBox plugin directory.

### 3. Build and deploy the Vela app

In `VibeNudger-vela`:

```bash
npm install
npm run build
```

Install the generated package onto the target Xiaomi Vela device.

## Current Status

- Windows-side audio session monitoring is implemented.
- Local IPC for session discovery and level sampling is implemented.
- AstroBox bridge forwarding is implemented.
- Xiaomi Vela device-side rendering is implemented.

This repository is the top-level entry point and documentation hub. Source code lives in the component repositories listed above.

## Release Order

1. Publish `VibeNudger-win`
2. Publish `VibeNudger-plugin`
3. Publish `VibeNudger-vela`
4. Keep this repository as the landing page, overview, and setup guide

## Roadmap

- Add screenshots, demo GIFs, and a full setup walkthrough.
- Improve the Windows app with tray mode, startup registration, and event-driven refresh paths.
- Clarify compatibility for supported AstroBox and Xiaomi Vela environments.
- Add tagged releases and changelogs across all component repositories.

## License

MIT. See [LICENSE](./LICENSE).
