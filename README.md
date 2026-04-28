# maude (MyClaude)

`maude` is a lightweight wrapper for the [Claude Code](https://github.com/anthropics/claude-code) CLI that enables it to run against local OpenAI-compatible inference endpoints, such as Ollama.

By automating the configuration of `ANTHROPIC_BASE_URL` and model overrides, `maude` allows you to use the official Claude CLI with local models without manually managing environment variables.

## Features

- **Interactive Setup**: Select from a list of known endpoints and available models via an interactive menu.
- **Ollama Integration**: Includes health checks for Ollama and can attempt to start the Ollama service automatically on macOS and Linux.
- **Transparent Proxy**: All arguments passed after `--` are forwarded directly to the `claude` CLI.
- **Dynamic Configuration**: Automatically maps local models to Claude's internal model tiers (Haiku, Sonnet, Opus) to prevent fallback errors.

## Installation

### Quick Install (One-liner)
Run the following command to install `maude` directly to `~/bin/maude`:

```bash
curl -fsSL https://raw.githubusercontent.com/cloudxabide/devops/main/Files/home_bin_maude -o ~/bin/maude && chmod +x ~/bin/maude
```

### Manual Install
1. Clone this repository.
2. Copy `Scripts/maude` to `~/bin/maude`.
3. Make it executable: `chmod +x ~/bin/maude`.

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
- `-e, --endpoint <HOST:PORT>`: Specify the inference endpoint (default: `localhost:11434`).
- `-m, --model <MODEL>`: Specify the model name.
- `--install`: Download the latest version of the script to `~/bin/maude`.
- `--`: Separate `maude` arguments from arguments passed to the `claude` CLI.

## Prerequisites

- **Claude Code CLI**: Must be installed and available in your PATH.
- **Local LLM Server**: An OpenAI-compatible API (e.g., Ollama).
- **Dependencies**: `curl` and `python3` (used for parsing model lists).
