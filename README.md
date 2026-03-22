# appz-blueprints

Community blueprints for [appz-cli](https://github.com/AviHS/appz-cli). Blueprints define project scaffolding and operational tasks for `appz init`.

## Usage

```bash
# Init with default blueprint
appz init nextjs

# Init with a named blueprint
appz init nextjs/ecommerce

# List available blueprints
appz blueprints list
appz blueprints list nextjs
```

## Structure

```
framework/
  blueprint-name/
    blueprint.yaml
    templates/          # optional template files
```

Each framework has a `default/` blueprint used when no specific blueprint is named.

## Blueprint Format

```yaml
version: 1

meta:
  name: "My Blueprint"
  description: "What this blueprint sets up"
  framework: "nextjs"
  create_command: "npx create-next-app@latest"  # optional
  package_manager: "npm"                         # optional

config:
  variable_name: "default_value"

setup:
  - desc: "Install dependencies"
    add_dependency: ["tailwindcss", "postcss"]
    dev: true

  - desc: "Create config file"
    write_file:
      path: "tailwind.config.js"
      content: |
        module.exports = { content: ['./src/**/*.{js,ts,jsx,tsx}'] }

  - desc: "Set environment variable"
    set_env:
      API_URL: "{{variable_name}}"

  - desc: "Run setup command"
    run: "npx tailwindcss init -p"

tasks:
  dev:
    - run: "npm run dev"
  build:
    - run: "npm run build"
```

## Setup Step Types

| Step | Description |
|------|-------------|
| `run` | Execute a command (locally, or remotely if `hosts` configured) |
| `run_locally` | Execute a command locally (even when `hosts` are configured) |
| `add_dependency` | Install packages (with `dev: true` for dev deps) |
| `write_file` | Create/overwrite a file (`path` + `content` or `template`) |
| `patch_file` | Insert/replace in existing files (`after`, `before`, `replace`) |
| `set_env` | Upsert key-value pairs in `.env` |
| `mkdir` | Create a directory |
| `cp` | Copy files (`src` + `dest`) |
| `rm` | Remove a file or directory |

## Contributing

1. Fork this repo
2. Add your blueprint under `framework/blueprint-name/blueprint.yaml`
3. Update `registry.json` with the new entry
4. Submit a PR
