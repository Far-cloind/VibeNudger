# VibeNudger

[中文版](./README.md) | [English](./README.en.md)

VibeNudger 是一个多仓库项目，用于采集 Windows 音频会话电平，并将结果转发到可穿戴设备端展示。

## 项目作用

- 监控 Windows 上按进程划分的音频会话电平
- 提供本地 IPC 接口，用于会话发现和电平采样
- 通过 AstroBox 桥接插件转发采样结果
- 在小米 Vela 设备端展示实时电平、波形和可选震动反馈

## 仓库结构

`VibeNudger-win` -> `VibeNudger-plugin` -> `VibeNudger-vela`

- `VibeNudger-win`：基于 C# 与 NAudio 的 Windows 桌面监视器，负责采样音频会话并暴露本地 HTTP IPC。
- `VibeNudger-plugin`：AstroBox 插件，负责轮询本地 IPC 并把音频电平消息转发到设备端。
- `VibeNudger-vela`：小米 Vela 快应用，负责接收消息并渲染电平、波形和震动反馈。

## 子仓库

| 仓库 | 角色 | 状态 |
| --- | --- | --- |
| [VibeNudger-win](https://github.com/Far-cloind/VibeNudger-win) | Windows 音频会话监视器与 IPC 服务 | Active |
| [VibeNudger-plugin](https://github.com/Far-cloind/VibeNudger-plugin) | AstroBox 桥接插件 | Active |
| [VibeNudger-vela](https://github.com/Far-cloind/VibeNudger-vela) | 小米 Vela 设备端应用 | Active |

## 架构

1. `VibeNudger-win` 监控默认渲染设备，并提供 `GET /api/level`、`GET /api/selected` 等本地接口。
2. `VibeNudger-plugin` 轮询 `http://127.0.0.1:38421`，并把采样结果转换成设备端可用消息。
3. `VibeNudger-vela` 在设备侧接收消息，展示音频电平、状态、波形和可选震动反馈。

## 演示素材

### Windows 监视器

[![VibeNudger Windows monitor](https://github.com/Far-cloind/VibeNudger-win/blob/main/screenshot.png?raw=1)](https://github.com/Far-cloind/VibeNudger-win)

### AstroBox 插件

[![VibeNudger AstroBox plugin](https://github.com/Far-cloind/VibeNudger-plugin/blob/main/screenshot.png?raw=1)](https://github.com/Far-cloind/VibeNudger-plugin)

### 小米 Vela 应用

[![VibeNudger Vela main page](https://github.com/Far-cloind/VibeNudger-vela/blob/main/page1.png?raw=1)](https://github.com/Far-cloind/VibeNudger-vela)

[![VibeNudger Vela waveform page](https://github.com/Far-cloind/VibeNudger-vela/blob/main/page2.png?raw=1)](https://github.com/Far-cloind/VibeNudger-vela)

## 快速开始

### 1. 运行 Windows 主机端

在 `VibeNudger-win` 中执行：

```powershell
dotnet run --project .\VibeNudger.App\
```

### 2. 构建并安装 AstroBox 插件

在 `VibeNudger-plugin` 中执行：

```bash
pnpm install
pnpm build
```

将生成的 `dist` 目录内容复制到 AstroBox 插件目录。

### 3. 构建并部署 Vela 应用

在 `VibeNudger-vela` 中执行：

```bash
npm install
npm run build
```

将生成的安装包部署到目标小米 Vela 设备。

## 许可证

MIT，见 [LICENSE](./LICENSE)。
