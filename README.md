# maude (MyClaude)

`maude` is a lightweight wrapper for the [Claude Code](https://github.com/anthropics/claude-code) CLI that enables it to run against local OpenAI-compatible inference endpoints, such as Ollama.

By automating the configuration of `ANTHROPIC_BASE_URL` and model overrides, `maude` allows you to use the official Claude CLI with local models without manually managing environment variables.

## Features

- **Interactive Setup**: Select from a list of known endpoints and available models via an interactive menu.
- **Ollama Integration**: Includes health checks for Ollama and can attempt to start the Ollama service automatically on macOS and Linux.
- **Transparent Proxy**: All arguments passed after `--` are forwarded directly to the `claude` CLI.
- **Dynamic Configuration**: Automatically maps local models to Claude's internal model tiers (Haiku, Sonnet, Opus) to prevent fallback errors.
- **Config File**: Override defaults and add endpoints via `~/.config/maude.conf` without editing the script.
- **Session Stats**: Optional verbose mode reports wall time and token counts after each session.
- **Self-Updating**: Built-in `--update` flag replaces the running script with the latest version from the repo.

## Installation

### Quick Install (One-liner)

```bash
curl -fsSL https://raw.githubusercontent.com/cloudxabide/maude-code/refs/heads/main/Scripts/maude \
  -o /tmp/maude && install -m 0755 /tmp/maude ~/bin/maude && rm -f /tmp/maude
```

### Via the built-in flag

If you already have `maude` installed:

```bash
maude --install   # installs latest to ~/bin/maude
maude --update    # replaces the running script in-place
```

### Manual (first) Install
1. Clone this repository.
2. ```bash install -m 0755 Scripts/maude ~/bin/maude```

## Usage

### Interactive Mode
Simply run `maude` to enter the interactive endpoint and model selection menu:
```bash
maude
```

### Explicit Configuration
Skip the menus by specifying the endpoint and model directly:
```bash
maude -e localhost:11434 -m llama3 -- [claude-args]
```

### Command Reference

| Flag | Description |
|------|-------------|
| `-e, --endpoint HOST:PORT` | Inference endpoint (default: `localhost:11434`) |
| `-m, --model MODEL` | Model name; skips the model selection menu |
| `-v, --verbose` | Print setup details and post-session stats (wall time, token counts, output rate) |
| `--install` | Download the latest script to `~/bin/maude` |
| `--update` | Replace the currently-running script with the latest from the repo |
| `--` | Pass remaining arguments directly to `claude` |

## Configuration

### State file: `~/.local/state/maude/maude.state`

Created automatically on first run. Controls the endpoint menu and install destination:

```bash
KNOWN_ENDPOINTS=(
  "localhost:11434"
  "10.10.12.251:11434"
)
DEFAULT_ENDPOINT="localhost:11434"
MAUDE_INSTALL_URL="https://raw.githubusercontent.com/cloudxabide/maude-code/refs/heads/main/Scripts/maude"
MAUDE_INSTALL_DEST="${HOME}/bin/maude"
```

### Config file: `~/.config/maude.conf`

Optional. Sourced as bash on every run. Recognised variables:

```bash
DEFAULT_ENDPOINT="host:port"   # override the default endpoint
DEFAULT_MODEL="model-name"     # skip the model menu entirely
EXTRA_ENDPOINTS=(              # appended to the endpoint menu
  "gpu-box:11434"
)
```

Example — make a remote GPU box the default and add it to the menu:

```bash
# ~/.config/maude.conf
DEFAULT_ENDPOINT="10.10.12.251:11434"
EXTRA_ENDPOINTS=(
  "10.10.12.251:11434"
)
```

With this config, hitting Enter at the endpoint prompt selects `10.10.12.251:11434` automatically. `EXTRA_ENDPOINTS` is merged with the built-in list from the state file, so `localhost:11434` remains available as a numbered option.

## Prerequisites

- **Claude Code CLI**: Must be installed and available in your PATH.
- **Local LLM Server**: An OpenAI-compatible API (e.g., Ollama).
- **`curl`**: Required for endpoint checks and self-install/update.
- **`jq`** *(recommended)*: Used for model list parsing; falls back to `python3` if not present.
- **`python3`**: Fallback for model list parsing when `jq` is unavailable.
