### Installation instruction

1. Build the image:
```bash
docker compose build claude-sandbox
```

2. Add this function to your `~/.zshrc` (or `~/.bashrc`):
```bash
 claude-sandbox() {
    local CONTAINER="claude-sandbox"

    if docker ps -q -f "name=^${CONTAINER}$" | grep -q .; then
      docker exec -it "$CONTAINER" claude
    else
      docker run --rm -it \
        --name "$CONTAINER" \
        -v ~/.claude-docker:/root/.claude \
        -v "$(pwd):/workspace" \
        -w /workspace \
        claude-code
    fi
  }
```

3. Reload your shell, f.e.:
```bash
source ~/.zshrc
```

4. Start the container (once, to log in):
```bash
claude-sandbox
```

From now on, `claude-sandbox` reuses the existing container if it exists, or creates a new one — the login is always preserved via `~/.claude-docker`.