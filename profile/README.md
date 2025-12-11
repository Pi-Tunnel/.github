# PiTunnel

Open-source tunnel solution that securely exposes services on your local network to the internet. A self-hosted alternative similar to Cloudflare Tunnel.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![npm](https://img.shields.io/npm/v/pi-tunnel-client)](https://www.npmjs.com/package/pi-tunnel-client)
[![npm](https://img.shields.io/npm/v/pi-tunnel-server)](https://www.npmjs.com/package/pi-tunnel-server)

## Packages

| Package | Description | Install |
|---------|-------------|---------|
| [pi-tunnel-client](https://www.npmjs.com/package/pi-tunnel-client) | PiTunnel Client | `npm install -g pi-tunnel-client` |
| [pi-tunnel-server](https://www.npmjs.com/package/pi-tunnel-server) | PiTunnel Server (Should be installing via bash script) | `npm install -g pi-tunnel-server` |

## Features

- **Web Tunnel**: HTTP/HTTPS traffic proxying
- **TCP Tunnel**: SSH, RDP, MySQL, PostgreSQL, FTP and other protocols
- **Custom Domain**: Use your own domain instead of auto-generated subdomain
- **WebSocket Support**: Full bidirectional WebSocket proxy (including React/Vite/Next.js HMR)
- **Dynamic Port**: Automatic port opening based on target port
- **Cross-Platform Client**: Windows, macOS and Linux support
- **Auto Reconnect**: Automatic reconnection when connection drops
- **System Service**: Auto-start on system boot
- **RESTful API**: API for tunnel management
- **Statistics**: Request count, bandwidth usage

## Architecture

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

## Quick Start

### 1. Server Setup (One-Line Install)

```bash
curl -fsSL https://raw.githubusercontent.com/Pi-Tunnel/Server/refs/heads/main/setup.sh -o /tmp/setup.sh && sudo bash /tmp/setup.sh
```

This will automatically:
- Install Node.js and dependencies
- Configure PiTunnel Server interactively
- Set up systemd service (auto-start on boot)
- Configure firewall rules
- Generate authentication token
- Create `piserver` CLI command

### 2. DNS Configuration

Add a wildcard DNS record for auto-generated subdomains:
```
*.tunnel.yourdomain.com  A  YOUR_SERVER_IP
```

For custom domains, point your domain directly to the server:
```
myapp.example.com  A  YOUR_SERVER_IP
```

### 3. Client Setup

```bash
# Install client globally
npm install -g pi-tunnel-client

# Login to server
piclient login

# Start tunnel
piclient start
# Target: 127.0.0.1:3000
```

### 4. Access

```
http://your-tunnel-name.tunnel.yourdomain.com
```

## Use Cases

### Web Development (React, Vite, Next.js)

```bash
# Run development server locally
npm run dev  # port 3000

# Start tunnel
piclient start
# Type: Web
# Target: 127.0.0.1:3000

# Now accessible at http://my-app.tunnel.domain.com
# HMR (Hot Reload) works automatically!
```

### SSH Access

```bash
piclient start
# Type: TCP
# Protocol: SSH
# Target: 127.0.0.1:22

# Connect remotely:
ssh user@my-tunnel.tcp.tunnel.domain.com
```

### Database Sharing

```bash
piclient start
# Type: TCP
# Protocol: MySQL
# Target: 127.0.0.1:3306

# Connect remotely:
mysql -h my-tunnel.tcp.tunnel.domain.com -u user -p
```

### API Webhook Testing

```bash
# Run Express/Flask/Django API
python app.py  # port 8080

# Start tunnel
piclient start
# Target: 127.0.0.1:8080

# Test webhook:
curl http://my-api.tunnel.domain.com/webhook
```

### Remote Desktop (RDP)

```bash
piclient start
# Type: TCP
# Protocol: RDP
# Target: 127.0.0.1:3389

# Connect from Windows:
mstsc /v:my-tunnel.tcp.tunnel.domain.com
```

### Custom Domain

Use your own domain instead of auto-generated subdomain:

```bash
piclient start
# Type: Web
# Target: 127.0.0.1:3000
# Domain Type: Custom domain
# Custom domain: myapp.example.com

# Now accessible at http://myapp.example.com
```

Or via CLI:
```bash
piclient connect -n myapp -s ws://server:8081 -t localhost:3000 --custom-domain myapp.example.com
```

## Client Commands

| Command | Description |
|---------|-------------|
| `piclient login` | Login to server |
| `piclient logout` | Logout from server |
| `piclient start` | Start new tunnel (interactive) |
| `piclient start -b` | Start in background |
| `piclient stop` | Stop tunnel (interactive) |
| `piclient stop --all` | Stop all tunnels |
| `piclient status` | Status and statistics |
| `piclient list` | List saved connections |
| `piclient install` | Auto-start on system boot |
| `piclient uninstall` | Remove auto-start |

## Server Commands

After server installation, use `piserver` to manage:

| Command | Description |
|---------|-------------|
| `piserver start` | Start server |
| `piserver stop` | Stop server |
| `piserver restart` | Restart server |
| `piserver status` | Show server status |
| `piserver logs` | View live logs |
| `piserver config` | Show configuration |
| `piserver token` | Show auth token |

## Port Structure

| Port | Usage |
|------|-------|
| 80 | HTTP Tunnel traffic |
| 8081 | WebSocket (Client connections) |
| 8082 | API (Tunnel management) |
| Dynamic | Automatically opened based on client target port |

## API Reference

Server provides RESTful API (protected with token):

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Server status (public) |
| `/tunnels` | GET | List active tunnels |
| `/tunnels/:name` | GET | Tunnel details |
| `/tunnels/:name` | DELETE | Stop tunnel |
| `/tunnels/:name/restart` | POST | Restart tunnel |
| `/stats` | GET | General statistics |

```bash
# Example: List tunnels
curl -H "X-Auth-Token: your-token" http://server:8082/tunnels
```

For detailed API documentation, see [server/README.md](https://github.com/Pi-Tunnel/Server#readme).

## Repositories

- **Server**: [github.com/Pi-Tunnel/Server](https://github.com/Pi-Tunnel/Server)
- **Client**: [github.com/Pi-Tunnel/Client](https://github.com/Pi-Tunnel/Client)

## Security

- All connections are authenticated with token
- API endpoints are protected
- HTTPS recommended for production
- Keep your token secure

## Firewall Configuration

```bash
# Basic ports
sudo ufw allow 80/tcp      # HTTP
sudo ufw allow 8081/tcp    # WebSocket
sudo ufw allow 8082/tcp    # API

# Dynamic ports (for HMR support)
sudo ufw allow 3000:9000/tcp
```

## Requirements

- **Node.js**: 18.0.0 or higher
- **Server**: VPS/server with public IP
- **DNS**: Wildcard DNS support

## Platform Support

| Platform | Client | Server | System Service |
|----------|--------|--------|----------------|
| Windows | ✅ | ✅ | Task Scheduler |
| macOS | ✅ | ✅ | LaunchDaemon |
| Linux | ✅ | ✅ | systemd |

## License

MIT

## Contributing

Pull requests are welcome! Please open an issue first to discuss the proposed changes.
