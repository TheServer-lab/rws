# RWS — Rework Share

> A Discord-backed CLI package manager. Package, publish, and install projects using a simple ID — no external links, no hosting required.

---

## What is RWS?

RWS lets you share entire code projects through a short ID like `rws_hoanzjuy`. Anyone with the tool installed can pull your project instantly. It uses a Discord server as a storage backend — no servers to host, no accounts to create on third-party platforms.

```
rws publish         → uploads your project, gives you an ID
rws install <id>    → anyone downloads and extracts it instantly
```

---

## Features

- 📦 Packages entire directories into `.rwsp` files
- 🔗 Share projects with a single short ID
- ✅ SHA-256 integrity verification on every install
- ⚠️ Executable file detection with install warnings
- 🔄 Automatic retry + Discord rate limit handling
- 💾 Local cache to minimize API calls
- 📝 Install and publish logs at `~/rws/logs.txt`

---

## Installation

### Windows

1. Download the latest `rws-installer-kit.zip` from [Releases](../../releases)
2. Extract it anywhere
3. Double-click `BUILD_INSTALLER.bat`
4. Follow the prompts — it compiles and installs `rws.exe` automatically
5. Open a **new** terminal and run `rws`

**Requirements:** [Python 3.10+](https://www.python.org/downloads/) (tick *"Add Python to PATH"* during install)

---

## Usage

### Authentication

```bash
rws register <username>     # create an account
rws login <username>        # log in
rws logout                  # log out
rws whoami                  # show current user
```

### Publishing

```bash
# Package + publish the current directory
rws publish

# Publish with a custom name and version
rws publish --name myproject --version 2.0.0

# Publish an existing .rwsp file
rws publish myproject.rwsp
```

### Installing

```bash
# Install a package by ID
rws install rws_hoanzjuy

# Preview contents before installing
rws install rws_hoanzjuy --preview

# Install, skipping any executable files
rws install rws_hoanzjuy --no-exec
```

### Browsing

```bash
rws info rws_hoanzjuy       # show package metadata
rws list                    # list all published packages
```

---

## How It Works

```
rws publish
  └─ Scans directory
  └─ Compresses into .rwsp (ZIP-based format)
  └─ Splits into 7.5 MB chunks
  └─ Uploads chunks to Discord #storage
  └─ Posts registry entry to Discord #registry
  └─ Returns a project ID

rws install <id>
  └─ Looks up ID in Discord #registry
  └─ Downloads all chunks from #storage
  └─ Reassembles + verifies SHA-256 checksum
  └─ Extracts to ~/rws/projects/<name>
```

### Storage Architecture

| Channel | Purpose |
|---|---|
| `#accounts` | One message per user account |
| `#storage` | One message per file chunk |
| `#registry` | One header + URL pages per package |

---

## File Structure

After installation, RWS creates:

```
~/rws/
├── session.json          current login session
├── registry_cache.json   cached package registry
├── accounts_cache.json   cached user accounts
├── logs.txt              install/publish/error log
├── projects/             installed packages live here
├── cache/                misc cache
└── temp/                 temporary build files
```

---

## Security

- Passwords are hashed with **bcrypt** before being stored
- Plaintext passwords never leave your machine
- Every package is verified with **SHA-256** before extraction
- Executable files (`.exe`, `.bat`, `.cmd`, `.ps1`) trigger a warning prompt
- Use `--no-exec` to automatically skip executables in scripts

---

## Limitations (v1)

- Depends on Discord API availability
- Max ~7.5 MB per chunk (Discord file size limit)
- No package encryption
- No dependency management
- No version update flow (republishing creates a new ID)

---

## Roadmap (v2+)

- [ ] Dependency system
- [ ] Version updates (`rws update <id>`)
- [ ] Package signing
- [ ] `rws search <query>`
- [ ] Web interface

---

## License

SOCL
