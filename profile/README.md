<div align="center">

# 🌐 Pi-Tunnel

**Open-source tunnel solution for securely exposing local services to the internet**

*Self-hosted alternative to Cloudflare Tunnel*

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![npm](https://img.shields.io/npm/v/pi-tunnel-client)](https://www.npmjs.com/package/pi-tunnel-client)

</div>

---

## 📦 Repositories

| Repository | Description |
|------------|-------------|
| [**Server**](https://github.com/Pi-Tunnel/Server) | PiTunnel Server - Install on your VPS |
| [**Client**](https://github.com/Pi-Tunnel/Client) | PiTunnel Client - Install on your local machine |

---

## ✨ Features

- 🌐 **Web Tunnel** - HTTP/HTTPS traffic proxying
- 🔌 **TCP Tunnel** - SSH, RDP, MySQL, PostgreSQL, FTP and other protocols
- ⚡ **WebSocket Support** - Full bidirectional proxy (React/Vite/Next.js HMR works!)
- 🔄 **Auto Reconnect** - Automatic reconnection when connection drops
- 🖥️ **Cross-Platform** - Windows, macOS, Linux support
- 🛠️ **System Service** - Auto-start on system boot
- 📊 **Statistics** - Request count, bandwidth usage

---

## 🏗️ Architecture

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│    Browser      │────▶│  PiTunnel       │────▶│   PiTunnel      │
│                 │     │  Server         │     │   Client        │
│ tunnel.domain   │◀────│  (Public)       │◀────│   (Local)       │
└─────────────────┘     └─────────────────┘     └────────┬────────┘
                                                         │
                                                         ▼
                                                ┌─────────────────┐
                                                │  Local Service  │
                                                │  (localhost)    │
                                                └─────────────────┘
```

---

## 🚀 Quick Start

### 1. Server Setup (One-Line Install)

```bash
curl -fsSL https://raw.githubusercontent.com/Pi-Tunnel/Server/refs/heads/main/setup.sh -o /tmp/setup.sh && sudo bash /tmp/setup.sh
```

### 2. DNS Configuration

```
*.tunnel.yourdomain.com  A  YOUR_SERVER_IP
```

### 3. Client Setup

```bash
npm install -g pi-tunnel-client
piclient login
piclient start
```

### 4. Access

```
http://your-tunnel-name.tunnel.yourdomain.com
```

---

## 💡 Use Cases

| Use Case | Type | Example |
|----------|------|---------|
| Web Development | Web | React, Vite, Next.js with HMR |
| Remote Access | TCP | SSH, RDP |
| Database Sharing | TCP | MySQL, PostgreSQL |
| Webhook Testing | Web | Stripe, GitHub webhooks |

---

## 🖥️ Platform Support

| Platform | Client | Server |
|----------|--------|--------|
| Windows | ✅ | ✅ |
| macOS | ✅ | ✅ |
| Linux | ✅ | ✅ |

---

<div align="center">

**[📖 Full Documentation](https://github.com/Pi-Tunnel/Server#readme)** · **[🐛 Report Bug](https://github.com/Pi-Tunnel/Server/issues)** · **[💡 Request Feature](https://github.com/Pi-Tunnel/Server/issues)**

</div>
