### Installation instruction
This repository provides a sample Dockerfile for claude and a compose file to create a claude sandbox.
You can use it within your coding repository.

1. Copy `docker-compose.yml` and `Dockerfile.claude` into the root of your repository
2. Build the image:
```bash
docker compose build claude-sandbox
```

3. Add this function to your `~/.zshrc` (or `~/.bashrc`):

The function checks if the container is already running and attaches to it, or starts a fresh one. Your Claude login is persisted in `~/.claude-docker` so you only need to authenticate once.

```
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

4. Reload your shell, f.e.:
```bash
source ~/.zshrc
```

5. Start the container (once, to log in):
```bash
claude-sandbox
```

From now on, `claude-sandbox` reuses the existing container if it exists, or creates a new one — the login is always preserved via `~/.claude-docker`.