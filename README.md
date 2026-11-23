# Canva Linux App

> Native Linux desktop application for Canva's design platform

[![Build Status](https://github.com/canva-linux/canva-linux-app/workflows/CI/badge.svg)](https://github.com/canva-linux/canva-linux-app/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Rust](https://img.shields.io/badge/rust-1.75+-orange.svg)](https://www.rust-lang.org)
[![Tauri v2](https://img.shields.io/badge/Tauri-v2-blue.svg)](https://tauri.app)

## Overview

Canva Linux App brings the full Canva design experience to Linux as a native desktop application. Built with **Tauri v2** (Rust + WebView), it provides:

- 🎨 **Full Canva Editor** - All design tools, templates, and features
- 🔐 **Secure Authentication** - OAuth 2.0 PKCE with system keyring storage
- 📁 **Native File Integration** - Open/save dialogs, drag-drop, `.canva` file association
- 📤 **Export Pipeline** - PNG, JPG, PDF, MP4, GIF with native progress
- 🔔 **System Integration** - Tray icon, global shortcuts, notifications
- 🌐 **Offline Mode** - Service Worker caching, background sync
- 🔄 **Auto-Updates** - Multiple channels (Flatpak, Snap, custom)
- ♿ **Accessibility** - WCAG 2.1 AA, screen reader support

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Tauri v2 Application                   │
├─────────────────────────────────────────────────────────────┤
│  Frontend (WebView)          │  Backend (Rust)              │
│  ─────────────────────       │  ──────────────────          │
│  • Canva Web Editor          │  • Auth Manager (OAuth PKCE) │
│  • Injected Tauri APIs       │  • Canva API Client          │
│  • Service Worker (Offline)  │  • File System (Portals)     │
│  • TypeScript Bindings       │  • System Integration        │
│                              │    - Tray, Shortcuts         │
│                              │    - Notifications           │
│                              │  • Export Pipeline           │
│                              │  • Cache Manager             │
│                              │  • Updater                   │
└─────────────────────────────────────────────────────────────┘
```

## Documentation

| Document | Description |
|----------|-------------|
| [Requirements Analysis](docs/architecture/01-requirements-analysis.md) | Functional & non-functional requirements |
| [Technology Evaluation](docs/architecture/02-technology-evaluation.md) | Electron vs Tauri vs Flutter vs PWA |
| [ADR-001: Technology Choice](docs/architecture/03-adr-001-technology-choice.md) | Tauri v2 selection rationale |
| [System Architecture](docs/architecture/04-system-architecture.md) | Component diagram, data flows, security |
| [Risk Assessment](docs/architecture/05-risk-assessment.md) | Risks, mitigations, go/no-go criteria |
| [Implementation Phases](docs/architecture/06-implementation-phases.md) | 16-week plan, handoff, gates |
| [Project Structure](docs/specs/01-project-structure.md) | Repository layout, modules, config |
| [Frontend Spec](docs/specs/02-frontend-spec.md) | Injection layer, APIs, Service Worker |
| [Backend Spec](docs/specs/03-backend-spec.md) | Rust modules, commands, API client |
| [API Integration](docs/specs/04-api-integration.md) | Canva REST API, WebSocket, errors |
| [Packaging Spec](docs/specs/05-packaging-spec.md) | Flatpak, Snap, AppImage, deb, rpm, Arch |
| [Testing Spec](docs/specs/06-testing-spec.md) | Unit, integration, E2E, performance, a11y |

## Quick Start

### Prerequisites

```bash
# Ubuntu/Debian
sudo apt update && sudo apt install -y \
  libwebkit2gtk-4.1-dev \
  libayatana-appindicator3-dev \
  librsvg2-dev \
  libssl-dev \
  pkg-config \
  libgtk-4-dev \
  libadwaita-1-dev

# Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"

# Node.js
curl -fsSL https://fnm.vercel.app/install | bash
fnm install --lts

# Tauri CLI
cargo install tauri-cli --version "^2.0"
```

### Development

```bash
# Clone
git clone https://github.com/canva-linux/canva-linux-app.git
cd canva-linux-app

# Configure Canva API credentials
cp .env.example .env
# Edit .env with your Canva OAuth credentials

# Run development server (hot reload)
cargo tauri dev

# Or frontend only
cd frontend && pnpm dev
```

### Build

```bash
# Production build (all targets)
cargo tauri build

# Specific target
cargo build --release --target x86_64-unknown-linux-gnu
cargo build --release --target aarch64-unknown-linux-gnu
```

### Package

```bash
# All formats
./scripts/package.sh

# Specific format
./scripts/package.sh flatpak
./scripts/package.sh snap
./scripts/package.sh appimage
./scripts/package.sh deb
./scripts/package.sh rpm
```

### Test

```bash
# Unit tests
cargo test
cd frontend && pnpm test

# Integration tests
cargo test --test integration_test

# E2E tests
cd tests/e2e && pnpm test

# Lint
cargo clippy -- -D warnings
cargo fmt --check
cd frontend && pnpm lint
```

## Distribution

| Format | Install Command | Auto-Update |
|--------|-----------------|-------------|
| **Flatpak** | `flatpak install flathub com.canva.linux` | ✅ Flathub |
| **Snap** | `sudo snap install canva-linux` | ✅ Snapd |
| **AppImage** | Download → `chmod +x` → Run | ⚠️ Custom |
| **Debian/Ubuntu** | `sudo apt install ./canva-linux_1.0.0_amd64.deb` | ⚠️ APT |
| **Fedora/RHEL** | `sudo dnf install ./canva-linux-1.0.0-1.x86_64.rpm` | ⚠️ DNF |
| **Arch** | `yay -S canva-linux` | ⚠️ pacman |

## Configuration

### Environment Variables

```bash
# Required for development
CANVA_CLIENT_ID=your_client_id
CANVA_CLIENT_SECRET=your_client_secret
CANVA_REDIRECT_URI=http://localhost:1420/callback

# Optional
RUST_LOG=debug,tauri=info
WEBKIT_DISABLE_DMABUF_RENDERER=1  # For some GPU/driver combos
```

### User Config (`~/.config/canva-linux/config.json`)

```json
{
  "theme": "system",
  "language": "en",
  "shortcuts": {
    "new-design": "Ctrl+Shift+N",
    "export": "Ctrl+Shift+E",
    "quick-capture": "Ctrl+Shift+C"
  },
  "offline": {
    "enabled": true,
    "maxCacheSize": "500MB"
  },
  "updates": {
    "channel": "stable",
    "autoDownload": true,
    "autoInstall": false
  }
}
```

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/amazing-feature`
3. Make your changes
4. Run tests: `cargo test && cd frontend && pnpm test`
5. Run lint: `cargo clippy -- -D warnings && cd frontend && pnpm lint`
6. Commit with [Conventional Commits](https://www.conventionalcommits.org/)
7. Push and open a Pull Request

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## Security

- **Token Storage**: Encrypted in system keyring (libsecret)
- **Network**: HTTPS only, certificate pinning for updates
- **Sandbox**: Tauri capability model, minimal permissions
- **Updates**: Ed25519 signed, verified before install

Report security issues to: security@canva-linux.app

See [SECURITY.md](SECURITY.md) for full policy.

## Roadmap

### v1.0 (MVP)
- [x] Architecture design
- [ ] Tauri app shell
- [ ] OAuth authentication
- [ ] Canva API integration
- [ ] Native file dialogs
- [ ] Export pipeline
- [ ] System tray + shortcuts
- [ ] Flatpak/Snap/AppImage packaging
- [ ] Flathub/Snapcraft publication

### v1.1
- [ ] Real-time collaboration (cursors, comments)
- [ ] Offline mode with sync
- [ ] Auto-update (custom channel)
- [ ] Brand Kit integration
- [ ] Plugin system foundation

### v2.0
- [ ] Native Linux menus (GTK4/Adwaita)
- [ ] Headless CLI for automation
- [ ] Wayland-native protocols
- [ ] Multi-window support
- [ ] Canva Apps/Extensions support

## License

MIT License - see [LICENSE](LICENSE) for details.

## Acknowledgments

- [Tauri](https://tauri.app) - Amazing framework
- [Canva](https://canva.com) - Design platform
- [WebKitGTK](https://webkitgtk.org) - Web engine
- [xdg-desktop-portal](https://github.com/flatpak/xdg-desktop-portal) - Desktop integration

---

**Built with ❤️ for the Linux design community**

*Not affiliated with Canva Pty Ltd. Canva is a trademark of Canva Pty Ltd.*