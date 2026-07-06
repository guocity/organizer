# Organizer

Organizer is a suite of tools designed to help you scan, structure, extract, and analyze your data locally and securely.

---

## 📱 Organizer for iOS

A local-first, privacy-focused document scanner and manager for iPhone.

### Key Features (iOS)
- 📸 **On-Device Document Scanner**: High-fidelity scanner using your camera to capture receipts, invoices, and documents with auto-edge detection.
- 🧠 **Local OCR & Extraction**: Extract text, classify documents, and fill fields entirely on-device without cloud dependencies.
- 🔒 **Biometric Lock**: Protect sensitive records with Face ID or Touch ID.
- ☁️ **iCloud Sync**: Securely backup and sync your documents across your Apple devices using your personal iCloud account.
- 🚫 **Complete Privacy**: Zero analytics, trackers, or account registration.

*Currently in TestFlight Beta. Visit [organizer.lgnat.com](https://organizer.lgnat.com) for details.*

---

## 🖥️ Organizer for macOS (Desktop Hub)

A local-first desktop hub designed to extract data from websites, store it in structured local SQLite databases, and visualize it using interactive, sandboxed canvases.

It features a built-in AI coding agent (the "harness operator") that can automatically write scrapers, database schemas, data transformation scripts, and frontend visualization boards for you.

### Key Features (macOS)
- 🖥️ **Desktop Native Hub**: Built with Electron, React, and Vite for a smooth, high-performance desktop experience on macOS.
- 🕷️ **Scrape & Ingest Anything**: Executes custom scripts using the `website-api` runtime to scrape or pull data from any webpage, securely using local browser cookies and sessions.
- 💾 **Local-First SQLite Storage**: Stores raw JSON snapshots and transforms them into clean, indexable tables in local SQLite databases (one isolated database file per site).
- 🎨 **Sandboxed Interactive Canvases**: Renders HTML/React micro-apps in a sandboxed environment that query your local SQLite data via a read-only postMessage database bridge.
- 🤖 **AI-Agent Integration**: A built-in coding agent serves as your "harness operator." You can chat with the agent and ask it to author schemas, JS transforms, or canvas visualizations directly on your local workspace.

### System Architecture (macOS)

```
┌────────────────────────────────────────────────────────────────────────┐
│                            Organizer (Electron)                        │
│                                                                        │
│  Renderer (React)                 Main process (Node)                  │
│  ┌──────────────┐                 ┌──────────────────────────────┐     │
│  │ Sidebar      │  IPC            │ services/registry            │     │
│  │  · Chats     │ ◄─────────────► │ services/siteRunner (fetch)  │     │
│  │  · Websites  │ organizer:hub:* │ services/ingest              │     │
│  │  · Canvas    │                 │ services/database (SQLite)   │     │
│  │              │                 │ services/canvasStore         │     │
│  ├──────────────┤                 │ services/harness             │     │
│  │ WebsiteView  │                 └──────────────────────────────┘     │
│  │ CanvasView   │                                                      │
│  │ ChatView ────┼── pi agent (subprocess bridge)                       │
│  └──────────────┘                                                      │
└────────────────────────────────────────────────────────────────────────┘
```

### On-Disk Directory Structure (`~/.organizer`)

All configuration, logs, databases, and workspace files are stored locally in your home directory:

```
~/.organizer/
  config.json                  # Hub settings and registry sources
  agent/                       # AI agent configuration, chat sessions, and auth credentials
  workspace/                   # CWD for the AI coding agent
    AGENTS.md                  # Harness instructions that teach the agent how to build site packs
  sites/
    <siteId>/                  # One "pack" per site containing:
      api/index.js             #   Scraper/Fetcher script
      sql/schema.sql           #   SQLite database table definitions
      sql/transform.js         #   Script mapping scraped JSON to SQL rows
      canvas/                  #   HTML/React visualization code
  data/
    <siteId>.sqlite            # SQLite database file for each site
    raw/<siteId>/<runId>.json  # Unmodified raw JSON snapshots from scrapes
```

### Security & Sandboxing (macOS)

1. **Zero Cloud Storage**: All databases, cookies, passwords, and sessions remain strictly local to your machine under `~/.organizer`.
2. **Canvas Sandbox**: Canvas components are loaded in an iframe with `sandbox="allow-scripts"`. They have no access to the filesystem, Node APIs, or external networks.
3. **SELECT-only DB Access**: Canvases retrieve data exclusively through a secure IPC postMessage bridge. The host enforces read-only `SELECT` validation on all incoming database queries.

### Getting Started & Installation (macOS)

#### Download
Go to the **[Releases](https://github.com/guocity/organizer/releases)** tab and download the latest signed and notarized package:
- **macOS Apple Silicon (ARM64)**: `Organizer-<version>-arm64.dmg` or `Organizer-<version>-arm64-mac.zip`

#### How to Use
1. **Define a Site Pack**: Use the Sidebar to browse local and registry-downloaded sites.
2. **Run Ingest**: Click **Fetch** or **Ingest** to run the scraper script, transform the resulting JSON, and load it into your local SQLite DB.
3. **Ask the Agent**: Need a custom scraper, schema, or chart? Open the chat panel and ask the agent:
   > *"Write a schema and transform script for HackerNews to track story points and author names."*
4. **Render a Canvas**: Select the **Canvas** section in the sidebar to open interactive visualization dashboards mapped directly to your SQLite tables.

---

## License

Proprietary / Private
