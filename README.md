# Jcode on Android — Termux

Run **Jcode** directly on an Android phone using **Termux**.

Jcode is a terminal coding agent that supports multiple AI providers, direct APIs, OAuth authentication, OpenRouter, and OpenAI-compatible endpoints.

> Tested setup: Android + ARM64 / aarch64 + Termux from F-Droid.

---

## Features

- Run Jcode directly on Android
- ARM64 / aarch64 support
- Termux support
- Interactive Jcode TUI
- Direct API providers
- Google Gemini
- OpenRouter
- OpenAI / ChatGPT
- Claude
- GitHub Copilot
- OpenAI-compatible APIs
- Local OpenAI-compatible servers
- Custom provider profiles
- No PC required

---

## Requirements

| Requirement | Recommended |
|---|---|
| Android | Android 10+ |
| CPU | ARM64 / aarch64 |
| Terminal | Termux |
| Termux source | F-Droid |
| Internet | Required |
| Storage | At least 1 GB free |

---

# 1. Install Termux

Install Termux from F-Droid:

**Termux:**  
https://f-droid.org/packages/com.termux/

> Do not mix the F-Droid and Google Play Termux installations.

Open Termux after installation.

---

# 2. Update Termux

```bash
pkg update && pkg upgrade -y
````

---

# 3. Install Jcode Dependencies

Jcode's Linux ARM64 binary requires the Termux `glibc` runtime.

Install the glibc repository:

```bash
pkg install glibc-repo -y
```

Install the required packages:

```bash
pkg install glibc patchelf curl -y
```

---

# 4. Install Jcode

Install Jcode using the official installer:

```bash
curl -fsSL https://jcode.sh/install | bash
```

If `jcode` is not immediately available, add Jcode's local binary directory:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

Verify the installation:

```bash
jcode --version
```

---

# 5. Start Jcode

Launch the Jcode TUI:

```bash
jcode
```

The interactive Jcode terminal interface should open.

---

# 6. Connect an AI Provider

Inside Jcode, open the provider login menu:

```text
/login
```

Jcode will display the available authentication methods.

You can also start provider authentication from the shell:

```bash
jcode login
```

Choose the provider you want to use.

---

# Google Gemini

Google Gemini can be used directly with a Gemini API key.

## Get a Gemini API Key

Open Google AI Studio:

[https://aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)

Official Gemini API documentation:

[https://ai.google.dev/gemini-api/docs](https://ai.google.dev/gemini-api/docs)

Create an API key in Google AI Studio.

Then open Jcode:

```bash
jcode
```

Inside Jcode:

```text
/login
```

Select **Gemini API** when available and complete the authentication setup.

For applications using Google's Gemini API, the documented environment variable is:

```bash
export GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
```

> Never commit a real API key to GitHub.

---

# OpenRouter

[OpenRouter](https://openrouter.ai/) provides access to many AI models through a unified API.

## Get an OpenRouter API Key

OpenRouter:

[https://openrouter.ai/](https://openrouter.ai/)

API keys:

[https://openrouter.ai/settings/keys](https://openrouter.ai/settings/keys)

Jcode provides an OpenRouter provider profile.

Authenticate with:

```bash
jcode login --provider openrouter
```

Or from the Jcode TUI:

```text
/login
```

After authentication, use:

```text
/model
```

to choose a model.

OpenRouter is useful when you want access to multiple model providers without creating a separate integration for every provider.

---

# OpenCode

[OpenCode](https://opencode.ai/) is a separate terminal coding agent and AI provider ecosystem.

OpenCode is **not required** to run Jcode.

You can install and use Jcode independently.

## OpenCode Resources

Website:

[https://opencode.ai/](https://opencode.ai/)

GitHub:

[https://github.com/anomalyco/opencode](https://github.com/anomalyco/opencode)

If OpenCode is already installed, Jcode may detect supported credentials from recognized OpenCode credential files.

Jcode and OpenCode remain separate applications with separate configuration.

Jcode configuration:

```text
~/.jcode/
```

OpenCode credentials:

```text
~/.local/share/opencode/auth.json
```

If Jcode asks whether you want to import OpenCode credentials and you want to configure Jcode independently, cancel the import and use:

```text
/login
```

---

# Custom OpenAI-Compatible API

Jcode can connect to compatible API endpoints such as:

* OpenRouter
* OmniRoute
* Local AI servers
* Self-hosted gateways
* Cloud AI gateways
* Other OpenAI-compatible services

Create a custom provider:

```bash
printf '%s' "$MY_API_KEY" | jcode provider add my-api \
  --base-url https://example.com/v1 \
  --model your-model-id \
  --api-key-stdin \
  --set-default
```

Replace:

```text
https://example.com/v1
```

with the actual API base URL.

Replace:

```text
your-model-id
```

with the model ID supplied by the API provider.

Test the provider:

```bash
jcode --provider-profile my-api auth-test
```

Run a prompt:

```bash
jcode --provider-profile my-api run "Hello"
```

---

# OmniRoute

OmniRoute is an AI gateway/router rather than an AI model.

It can expose an OpenAI-compatible API and route requests to supported AI providers.

A local OmniRoute installation may use:

```text
http://127.0.0.1:20128/v1
```

> Only use this endpoint when OmniRoute is actually running locally on your device.

Configure Jcode as an OpenAI-compatible provider:

```bash
printf '%s' "$OMNIROUTE_API_KEY" | jcode provider add omniroute \
  --base-url http://127.0.0.1:20128/v1 \
  --model YOUR_MODEL_ID \
  --api-key-stdin \
  --set-default
```

Test it:

```bash
jcode --provider-profile omniroute auth-test
```

---

# Choosing a Provider

## Simplest Setup

Use a direct provider:

```text
Google Gemini
OpenAI
Claude
```

## Multiple Models

Use:

```text
OpenRouter
```

## Multi-Provider Routing

Use:

```text
OmniRoute
```

## Local Models

Use:

```text
Ollama
LM Studio
```

## Existing OpenCode Setup

OpenCode can remain installed separately.

Jcode does not require OpenCode.

---

# Jcode Commands

Start Jcode:

```bash
jcode
```

Run a single prompt:

```bash
jcode run "Explain this project"
```

Authenticate:

```bash
jcode login
```

Test configured providers:

```bash
jcode auth-test --all-configured
```

Manage providers:

```bash
jcode provider
```

Resume a previous session:

```bash
jcode --resume SESSION_NAME
```

---

# Useful Jcode TUI Commands

Inside Jcode:

```text
/login
```

Configure authentication.

```text
/model
```

Choose a model.

```text
/account
```

Manage or switch accounts.

```text
/config
```

Open configuration.

```text
/usage
```

View usage information.

```text
/help
```

Show available commands.

---

# Project Configuration

Jcode configuration is stored under:

```text
~/.jcode/
```

The main configuration file is:

```text
~/.jcode/config.toml
```

Project-level agent instructions can be placed in:

```text
AGENTS.md
```

Example:

```text
my-project/
├── AGENTS.md
├── src/
└── README.md
```

Jcode can use `AGENTS.md` instructions when working inside the project.

---

# API Key Security

Never put a real API key in:

* `README.md`
* GitHub issues
* Git commits
* Screenshots
* Public configuration files
* Public source code

Use placeholders in documentation:

```text
YOUR_API_KEY
```

Example:

```bash
export GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
```

Never publish a real credential such as:

```text
sk-xxxxxxxxxxxxxxxx
```

If an API key is accidentally exposed, revoke it immediately and create a new key.

---

# Recommended Installation

For a fresh Termux installation, the essential setup is:

```bash
pkg update && pkg upgrade -y
pkg install glibc-repo -y
pkg install glibc patchelf curl -y
curl -fsSL https://jcode.sh/install | bash
export PATH="$HOME/.local/bin:$PATH"
jcode --version
jcode
```

Then configure your preferred provider:

```text
/login
```

---

# Troubleshooting

## `glibc` Cannot Be Found

Install the Termux glibc repository:

```bash
pkg install glibc-repo -y
```

Then install the required runtime:

```bash
pkg install glibc patchelf -y
```

---

## `jcode: command not found`

Add Jcode's local binary directory:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

Then run:

```bash
jcode --version
```

---

## `required file not found`

If Jcode was installed before `glibc` was available, the Jcode binary may fail to start.

Install the required runtime:

```bash
pkg install glibc patchelf -y
```

Then reinstall Jcode:

```bash
curl -fsSL https://jcode.sh/install | bash
```

Verify:

```bash
jcode --version
```

---

## Provider Authentication Fails

Open the login menu:

```text
/login
```

Select the correct provider and authenticate again.

To test configured providers:

```bash
jcode auth-test --all-configured
```

---

## OpenCode Credentials Appear During Setup

OpenCode credentials are optional.

If you want a completely independent Jcode configuration, cancel the import:

```text
/cancel
```

Then configure your own provider:

```text
/login
```

There is no need to uninstall OpenCode.

---

# Updating Jcode

Run the official installer again:

```bash
curl -fsSL https://jcode.sh/install | bash
```

Verify the installed version:

```bash
jcode --version
```

---

# Official Resources

## Jcode

[https://jcode.sh/](https://jcode.sh/)

## Jcode Documentation

[https://jcode.sh/docs](https://jcode.sh/docs)

## Jcode GitHub

[https://github.com/1jehuang/jcode](https://github.com/1jehuang/jcode)

## Termux

[https://termux.dev/](https://termux.dev/)

## Termux on F-Droid

[https://f-droid.org/packages/com.termux/](https://f-droid.org/packages/com.termux/)

## Google AI Studio

[https://aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)

## Google Gemini API

[https://ai.google.dev/gemini-api/docs](https://ai.google.dev/gemini-api/docs)

## OpenRouter

[https://openrouter.ai/](https://openrouter.ai/)

## OpenRouter API Keys

[https://openrouter.ai/settings/keys](https://openrouter.ai/settings/keys)

## OpenCode

[https://opencode.ai/](https://opencode.ai/)

## OpenCode GitHub

[https://github.com/anomalyco/opencode](https://github.com/anomalyco/opencode)

---

# Android + Termux + Jcode

```text
Android
   │
   ▼
Termux
   │
   ├── glibc
   ├── patchelf
   └── curl
        │
        ▼
      Jcode
        │
        ├── Google Gemini
        ├── OpenAI
        ├── Claude
        ├── OpenRouter
        ├── OmniRoute
        ├── Ollama
        ├── LM Studio
        └── OpenAI-compatible APIs
```

---

# Credits

* Jcode
* Termux
* Google Gemini
* OpenRouter
* OpenCode
* Open-source AI community

---

# License

This README documents running Jcode on Android through Termux.

For Jcode's software license, see the upstream repository:

[https://github.com/1jehuang/jcode](https://github.com/1jehuang/jcode)

---

**Jcode runs directly on Android through Termux — no PC required.**

```
```
