# Canva Linux App

> Native Linux desktop application for Canva's design platform

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Rust](https://img.shields.io/badge/rust-1.75%2B-orange.svg)](https://www.rust-lang.org)
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

```text
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
git clone https://github.com/ath1t/canva-linux-app.git
cd canva-linux-app

# Run development server (hot reload)
cargo tauri dev
```

### Build

```bash
# Production build
cargo tauri build

# Specific Linux target
cargo build --release --target x86_64-unknown-linux-gnu
cargo build --release --target aarch64-unknown-linux-gnu
```

### Test

```bash
cargo test
cargo clippy -- -D warnings
cargo fmt --check
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
CANVA_CLIENT_ID=your_client_id
CANVA_CLIENT_SECRET=your_client_secret
CANVA_REDIRECT_URI=http://localhost:1420/callback

RUST_LOG=debug,tauri=info
WEBKIT_DISABLE_DMABUF_RENDERER=1
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
4. Run tests: `cargo test`
5. Run lint: `cargo clippy -- -D warnings && cargo fmt --check`
6. Commit with [Conventional Commits](https://www.conventionalcommits.org/)
7. Push and open a Pull Request

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## Security

- **Token Storage**: Encrypted in system keyring (libsecret)
- **Network**: HTTPS only, certificate pinning for updates
- **Sandbox**: Tauri capability model, minimal permissions
- **Updates**: Ed25519 signed, verified before install

Report security issues to: security@canva-linux.app

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
