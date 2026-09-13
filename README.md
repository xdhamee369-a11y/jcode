# Jcode Android Termux

> Run **Jcode** on Android through Termux using a native ARM64 environment — without proot, Docker, Ubuntu, or a Linux container.

<p align="center">
  <strong>Jcode + Termux + Android ARM64</strong>
</p>

<p align="center">
  Tested on OnePlus 10R / Android 15 / ARM64
</p>

---

## Overview

This repository documents a reliable way to install and use **Jcode** on Android with Termux.

Jcode is an AI coding-agent harness that can connect to multiple AI providers, including direct APIs and OpenAI-compatible gateways.

Official Jcode documentation:

https://jcode.sh/docs

Official Jcode repository:

https://github.com/1jehuang/jcode

This guide focuses on:

* Android ARM64
* Termux
* Termux glibc
* Jcode
* Google Gemini API
* OpenRouter
* OpenCode
* OpenAI-compatible APIs
* OmniRoute
* Git repositories
* `AGENTS.md`
* API-key security
* Troubleshooting

---

# Tested Environment

| Component    | Tested           |
| ------------ | ---------------- |
| Device       | OnePlus 10R      |
| Android      | Android 15       |
| CPU          | ARM64            |
| Architecture | `aarch64`        |
| Termux       | F-Droid          |
| Termux       | `0.119.0-beta.3` |
| Jcode        | `v0.84.0`        |
| glibc        | `2.44`           |
| patchelf     | `0.19.1`         |

Your versions may differ.

The important architecture requirement is:

```text
aarch64
```

---

# Architecture

The intended setup is:

```text
Android
   │
   ▼
OnePlus 10R
   │
   ▼
Termux
   │
   ▼
Termux glibc
   │
   ├── glibc
   └── patchelf
   │
   ▼
Jcode
   │
   ├── Google Gemini API
   ├── OpenRouter
   ├── OpenCode-compatible services
   ├── OmniRoute
   └── Other OpenAI-compatible APIs
```

---

# Why glibc Is Required

Android normally uses **Bionic libc**.

The Jcode Linux ARM64 release expects a glibc-compatible runtime when running through Termux.

Jcode's official Termux instructions therefore require:

```bash
pkg install glibc patchelf
```

before running the Jcode installer.

This order matters.

If Jcode is installed first and glibc/patchelf are added later, you can encounter:

```text
cannot execute: required file not found
```

even though the Jcode launcher itself exists.

---

# Requirements

Recommended:

* 64-bit ARM Android device
* 4 GB RAM or more
* 1 GB+ free storage
* Internet connection
* Maintained Termux installation

---

# 1. Install Termux

Use a maintained Termux distribution.

F-Droid:

https://f-droid.org/packages/com.termux/

Do not mix Termux applications and repositories from unrelated sources.

---

# 2. Check Architecture

Open Termux.

Run:

```bash
uname -m
```

Expected:

```text
aarch64
```

Also run:

```bash
termux-info
```

Look for:

```text
Packages CPU architecture:
aarch64
```

If you do not get `aarch64`, stop and verify your device architecture.

---

# 3. Update Termux

Run:

```bash
pkg update
```

Then:

```bash
pkg upgrade -y
```

---

# 4. Enable the glibc Repository

If this works:

```bash
pkg install glibc
```

you can continue.

If you receive:

```text
E: Unable to locate package glibc
```

install the glibc repository:

```bash
pkg install glibc-repo -y
```

Then update:

```bash
pkg update
```

---

# 5. Install Jcode Dependencies

Run:

```bash
pkg install glibc patchelf curl git -y
```

Verify:

```bash
glibc --version
```

```bash
patchelf --version
```

```bash
curl --version
```

All three commands should work.

---

# 6. Install Jcode

Use the official Jcode installer:

```bash
curl -fsSL https://jcode.sh/install | bash
```

Official installation documentation:

https://jcode.sh/docs

---

# 7. Fix PATH

Jcode normally installs its launcher under:

```text
~/.local/bin/jcode
```

For the current Termux session:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

Refresh the shell cache:

```bash
hash -r
```

Check:

```bash
which jcode
```

Expected:

```text
/data/data/com.termux/files/home/.local/bin/jcode
```

---

# 8. Verify Jcode

Run:

```bash
jcode --version
```

Then:

```bash
jcode
```

If the Jcode TUI appears, the installation is working.

---

# 9. First-Run Credential Import

Jcode may detect existing OpenCode credentials such as:

```text
~/.local/share/opencode/auth.json
```

If you want Jcode to use them, approve the source.

If you want a clean Jcode setup and want to configure your own API:

```text
/cancel
```

or skip the credential import according to the prompt.

Do not approve credentials simply because they were detected.

---

# 10. Provider Login

From Termux:

```bash
jcode login
```

Or inside Jcode:

```text
/login
```

Jcode provides different provider options depending on the installed release.

Common categories include:

* Google Gemini
* OpenAI
* Anthropic
* OpenRouter
* OpenCode Zen
* GitHub Copilot
* Azure OpenAI
* OpenAI-compatible APIs

---

# Google Gemini API

## 11. Create a Gemini API Key

Official Google AI Studio API-key page:

https://aistudio.google.com/app/apikey

Official Gemini API documentation:

https://ai.google.dev/gemini-api/docs

Google states that a Gemini API key is required to authenticate Gemini API requests.

---

## 12. Create the Key

Open:

https://aistudio.google.com/app/apikey

Then:

1. Sign in with Google.
2. Open the API Keys page.
3. Create an API key.
4. Copy the key.
5. Keep it private.

Google currently uses authorization keys for newly created AI Studio keys and recommends securing/restricting keys.

---

## 13. Gemini API Key Security

Never put the real key in this repository.

Use:

```text
YOUR_GEMINI_API_KEY
```

as the documentation placeholder.

Never commit:

```text
AIza...
```

or any other real API credential.

Do not publish API keys in:

* GitHub
* README files
* screenshots
* `.env` files
* shell scripts
* Git commits
* issue reports

If a key is accidentally published, revoke/rotate it immediately.

---

# 14. Configure Gemini in Jcode

Start Jcode:

```bash
jcode
```

Inside the TUI:

```text
/login
```

Select the Gemini API provider.

Paste your Google AI Studio API key when Jcode asks for it.

Then select a currently available Gemini model:

```text
/model
```

Do not hard-code an obsolete Gemini model name into this repository. Model availability changes over time.

---

# 15. Gemini Environment Variable

For a temporary shell session, Gemini's documented environment variable is:

```bash
export GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
```

Google documents `GEMINI_API_KEY` as the standard environment variable for Gemini API authentication.

Then:

```bash
jcode
```

For persistent use, prefer Jcode's provider credential storage instead of putting secrets directly into `.bashrc`.

---

# 16. Gemini Test

After configuring Gemini, run a simple request through Jcode:

```bash
jcode run "Reply exactly: GEMINI_JCODE_OK"
```

Expected:

```text
GEMINI_JCODE_OK
```

If it fails, check provider authentication:

```bash
jcode auth-test --all-configured
```

---

# OpenRouter

## 17. What Is OpenRouter?

OpenRouter is a model gateway that provides access to models from multiple providers through a unified API.

This is useful when you want:

```text
Jcode
  ↓
OpenRouter
  ↓
Multiple models/providers
```

OpenCode also officially supports OpenRouter. Its current documentation describes connecting an OpenRouter account and selecting models through `/models`.

---

## 18. OpenRouter API Key

Official OpenRouter website:

https://openrouter.ai/

API keys:

https://openrouter.ai/settings/keys

Create an API key and keep it private.

Do not put the key in GitHub.

---

## 19. Configure OpenRouter in Jcode

Inside Jcode:

```text
/login
```

Select:

```text
OpenRouter
```

Enter your OpenRouter API key.

Then:

```text
/model
```

Choose a model available through your OpenRouter account.

---

# OpenCode

## 20. What Is OpenCode?

OpenCode is another AI coding-agent terminal application.

Official website:

https://opencode.ai/

Official documentation:

https://opencode.ai/docs

OpenCode supports many LLM providers and uses `/connect` followed by `/models` for provider setup and model selection.

---

## 21. OpenCode and Jcode Are Separate

Do not confuse:

```text
Jcode
```

with:

```text
OpenCode
```

They are separate coding-agent applications.

This repository is primarily for:

```text
Jcode + Termux + Android
```

OpenCode is included here as an optional alternative/integration reference.

---

## 22. OpenCode Providers

OpenCode supports many providers and also supports custom OpenAI-compatible providers.

OpenCode's standard provider flow is:

```text
/connect
```

then:

```text
/models
```

For OpenRouter, OpenCode documents connecting the OpenRouter account and selecting a model afterward.

---

# OpenAI-Compatible APIs

## 23. What Does OpenAI-Compatible Mean?

An OpenAI-compatible API exposes an API format similar to OpenAI's API.

This allows Jcode to connect to:

* AI gateways
* Local model servers
* Self-hosted inference
* OmniRoute
* OpenRouter-compatible endpoints
* Other compatible services

Conceptually:

```text
Jcode
  ↓
OpenAI-compatible API
  ↓
Model provider
```

---

# OmniRoute

## 24. OmniRoute Architecture

OmniRoute can act as a routing/gateway layer:

```text
Jcode
  ↓
OmniRoute
  ├── OpenAI
  ├── Anthropic
  ├── Gemini
  ├── DeepSeek
  ├── Kimi
  └── Other configured providers
```

Use OmniRoute only after your direct provider configuration works.

This makes troubleshooting much easier.

---

## 25. Local OmniRoute

If OmniRoute is running locally on your Android device and exposes:

```text
http://127.0.0.1:20128/v1
```

then the endpoint can be used as an OpenAI-compatible base URL.

Do not use this URL unless OmniRoute is actually running on the phone at that address.

Example:

```bash
printf '%s' "$OMNIROUTE_API_KEY" | jcode provider add omniroute \
  --base-url "http://127.0.0.1:20128/v1" \
  --model "YOUR_MODEL_ID" \
  --api-key-stdin \
  --set-default
```

---

# 26. Remote OmniRoute

For a remote OmniRoute server:

```bash
printf '%s' "$OMNIROUTE_API_KEY" | jcode provider add omniroute \
  --base-url "https://YOUR-OMNIROUTE-DOMAIN/v1" \
  --model "YOUR_MODEL_ID" \
  --api-key-stdin \
  --set-default
```

Replace:

```text
YOUR-OMNIROUTE-DOMAIN
```

with your actual server.

Replace:

```text
YOUR_MODEL_ID
```

with a model exposed by OmniRoute.

---

# 27. OpenAI-Compatible Provider

Generic example:

```bash
printf '%s' "$MY_API_KEY" | jcode provider add my-api \
  --base-url "https://example.com/v1" \
  --model "YOUR_MODEL_ID" \
  --api-key-stdin \
  --set-default
```

The `--api-key-stdin` approach avoids putting the secret directly into the command line.

---

# Provider Strategy

## Recommended Order

For a new Jcode installation:

```text
1. Jcode
      ↓
2. Direct Gemini API
      ↓
3. Test
      ↓
4. OpenRouter
      ↓
5. OmniRoute / other gateway
```

Do not configure five providers simultaneously.

First establish one known-good provider.

---

# 28. Recommended Setup

A practical configuration:

```text
PRIMARY
Google Gemini API
        ↓
       Jcode
```

Optional:

```text
SECONDARY
OpenRouter
        ↓
Multiple models
        ↓
       Jcode
```

Advanced:

```text
Jcode
  ↓
OmniRoute
  ↓
Multiple providers
```

This gives you a simple baseline before adding routing complexity.

---

# 29. Git Setup

Install Git:

```bash
pkg install git -y
```

Create a workspace:

```bash
mkdir -p ~/projects
cd ~/projects
```

Clone a project:

```bash
git clone YOUR_REPOSITORY_URL
```

Enter it:

```bash
cd YOUR_REPOSITORY
```

Launch Jcode:

```bash
jcode
```

---

# 30. AGENTS.md

Create:

```bash
nano AGENTS.md
```

Recommended:

```markdown
# Project Instructions

## General

- Inspect the repository before changing code.
- Do not delete files without explaining why.
- Do not modify unrelated files.
- Keep changes minimal.
- Run relevant tests after modifications.

## Security

- Never expose API keys.
- Never commit credentials.
- Never print secrets into logs.
- Never commit `.env` files.

## Android / Termux

This project may be developed on Android using Termux.

Do not assume:

- sudo
- systemd
- Docker
- Ubuntu
- x86_64
- standard Linux filesystem paths
```

---

# 31. Android Storage

If you need shared Android storage:

```bash
termux-setup-storage
```

Grant the Android permission.

Then:

```bash
ls ~/storage/shared
```

For development, prefer:

```text
~/projects/
```

for Git repositories rather than active repositories in shared storage.

---

# 32. Prevent Termux From Being Killed

Run:

```bash
termux-wake-lock
```

Also configure Android battery settings.

On OnePlus/OxygenOS, use settings similar to:

```text
Settings
→ Apps
→ Termux
→ Battery
→ Unrestricted
```

Exact menu names can differ.

---

# 33. Useful Commands

Architecture:

```bash
uname -m
```

Termux information:

```bash
termux-info
```

Jcode location:

```bash
which jcode
```

Jcode version:

```bash
jcode --version
```

Start:

```bash
jcode
```

Login:

```bash
jcode login
```

Provider diagnostics:

```bash
jcode auth-test --all-configured
```

---

# 34. Useful Jcode TUI Commands

Inside Jcode:

```text
/login
```

Provider authentication.

```text
/model
```

Model selection.

```text
/help
```

Available commands.

```text
/config
```

Configuration.

```text
/usage
```

Usage information.

```text
/quit
```

Exit.

The exact command set can change between releases.

---

# 35. PATH Problem

If:

```text
jcode: command not found
```

run:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

Then:

```bash
hash -r
```

Then:

```bash
which jcode
```

If that works, make it persistent:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
```

Reload:

```bash
source ~/.bashrc
```

---

# 36. `cannot execute: required file not found`

If Jcode reports:

```text
cannot execute: required file not found
```

check:

```bash
glibc --version
```

```bash
patchelf --version
```

```bash
uname -m
```

Expected architecture:

```text
aarch64
```

If Jcode was installed before glibc and patchelf, reinstall Jcode.

---

# 37. Clean Jcode Reinstall

Run:

```bash
curl -fsSL https://raw.githubusercontent.com/1jehuang/jcode/master/scripts/uninstall.sh | bash -s -- --yes
```

Then:

```bash
rm -f "$HOME/.local/bin/jcode"
```

Then reinstall:

```bash
curl -fsSL https://jcode.sh/install | bash
```

Reload:

```bash
export PATH="$HOME/.local/bin:$PATH"
hash -r
```

Verify:

```bash
jcode --version
```

Jcode's official uninstall script removes binaries while preserving configuration, authentication, and sessions.

---

# 38. Dynamic Linker Diagnostics

If the error remains:

```bash
file "$HOME/.jcode/builds/stable/jcode"
```

Then:

```bash
patchelf --print-interpreter "$HOME/.jcode/builds/stable/jcode"
```

Check:

```bash
ls -l "$PREFIX/glibc/lib/ld-linux-aarch64.so.1"
```

Check:

```bash
ls -lh "$HOME/.jcode/builds/stable/jcode"
```

Finally:

```bash
uname -m
```

Do not randomly install additional libraries before checking these results.

---

# 39. Mirror Problem

If Termux reports:

```text
No mirror or mirror group selected.
```

but then says the mirror is available and package installation succeeds, it is not necessarily an error.

If package downloads actually fail:

```bash
termux-change-repo
```

Select a working repository.

Then:

```bash
pkg update
```

---

# 40. Security

Never commit:

```text
Gemini API keys
OpenRouter API keys
OpenAI API keys
Anthropic API keys
OmniRoute API keys
OAuth tokens
auth.json
.env
private SSH keys
```

Use placeholders:

```text
YOUR_GEMINI_API_KEY
YOUR_OPENROUTER_API_KEY
YOUR_OMNIROUTE_API_KEY
```

---

# 41. `.gitignore`

Create:

```bash
nano .gitignore
```

Use:

```gitignore
.env
.env.*
*.key
*.pem
*credentials*
*secret*

auth.json
*_auth.json
*_oauth.json

.jcode/
.jcode-home/

provider-*.env
config.local.*

*.log

.vscode/
.idea/
.DS_Store

storage/
```

---

# 42. Check Before Git Push

Run:

```bash
git status
```

Then:

```bash
git diff --cached
```

Search for obvious key strings:

```bash
grep -RniE 'AIza|sk-|api[_-]?key|token|secret' . \
  --exclude-dir=.git \
  --exclude=README.md
```

Review the result before pushing.

---

# 43. Quick Installation

For a fresh ARM64 Termux installation:

```bash
pkg update
pkg upgrade -y
pkg install glibc-repo -y
pkg update
pkg install glibc patchelf curl git -y
termux-wake-lock
curl -fsSL https://jcode.sh/install | bash
export PATH="$HOME/.local/bin:$PATH"
hash -r
jcode --version
```

Start:

```bash
jcode
```

Then configure:

```text
/login
```

---

# 44. Gemini Quick Setup

Create the key:

https://aistudio.google.com/app/apikey

Then configure Gemini in Jcode:

```text
jcode
/login
```

Choose Gemini API.

Enter the key.

Then:

```text
/model
```

Select an available Gemini model.

Test:

```bash
jcode run "Reply exactly: GEMINI_JCODE_OK"
```

---

# 45. OpenRouter Quick Setup

Open:

https://openrouter.ai/

Create an API key:

https://openrouter.ai/settings/keys

Then:

```text
jcode
/login
```

Choose:

```text
OpenRouter
```

Enter the API key.

Then:

```text
/model
```

Select a model.

---

# 46. OpenCode Quick Setup

Official website:

https://opencode.ai/

Official documentation:

https://opencode.ai/docs

OpenCode's provider flow:

```text
/connect
```

Then:

```text
/models
```

OpenCode supports OpenRouter and custom OpenAI-compatible providers.

Remember:

```text
Jcode != OpenCode
```

They are separate coding-agent applications.

---

# 47. Troubleshooting Checklist

Before asking for help, run:

```bash
uname -m
termux-info
which jcode
jcode --version
glibc --version
patchelf --version
```

Provide the output but remove:

```text
API keys
tokens
passwords
private keys
auth.json
```

---

# 48. Recommended Architecture

```text
                    ANDROID
                       │
                       ▼
                ┌─────────────┐
                │   Termux    │
                └──────┬──────┘
                       │
                       ▼
              ┌─────────────────┐
              │ glibc + patchelf│
              └────────┬────────┘
                       │
                       ▼
                ┌─────────────┐
                │    Jcode    │
                │     TUI     │
                └──────┬──────┘
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       Gemini      OpenRouter   OmniRoute
          │            │            │
          ▼            ▼            ▼
       Google       Multiple     Multiple
       Gemini       Models       Providers
```

---

# 49. Recommended Setup Order

Do not configure everything at once.

Use this order:

```text
1. Install Termux
2. Verify aarch64
3. Update Termux
4. Enable glibc-repo
5. Install glibc
6. Install patchelf
7. Install Jcode
8. Verify Jcode
9. Start Jcode TUI
10. Skip unwanted credential imports
11. Configure Gemini API
12. Test Gemini
13. Add OpenRouter if needed
14. Add OmniRoute if needed
15. Clone projects
16. Add AGENTS.md
17. Start development
```

This makes failures easy to isolate.

---

# 50. Official Links

### Jcode

https://jcode.sh/

https://jcode.sh/docs

https://github.com/1jehuang/jcode

### OpenCode

https://opencode.ai/

https://opencode.ai/docs

### Google Gemini

https://ai.google.dev/gemini-api/docs

### Google AI Studio API Keys

https://aistudio.google.com/app/apikey

### OpenRouter

https://openrouter.ai/

### OpenRouter API Keys

https://openrouter.ai/settings/keys

### Termux

https://termux.dev/

### Termux on F-Droid

https://f-droid.org/packages/com.termux/

---

# License

This repository contains installation documentation and configuration examples.

Jcode, OpenCode, Termux, Google Gemini, and OpenRouter are separate projects/services maintained by their respective owners.

Always refer to their official documentation for current releases, provider requirements, pricing, limits, and security changes.
