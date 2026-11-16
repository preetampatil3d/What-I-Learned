# Ollama with Continue in IntelliJ on an M1 Mac

This guide records the setup and troubleshooting steps from our chat for using local Ollama models with the Continue plugin in IntelliJ. The machine discussed is a MacBook Pro with an M1 chip and 16 GB of unified memory.

## What you need to know

- Ollama runs the language model locally. For this setup, **you do not need an API key**.
- Continue connects to the local Ollama service at `http://localhost:11434`.
- Running Ollama in Docker Desktop on macOS is CPU-only; Docker Desktop does not provide Ollama with Mac GPU acceleration. Ollama's native Mac app can use GPU acceleration and may perform better.
- Docker, macOS, IntelliJ, and the model share the Mac's physical memory. Giving Docker a 16 GB limit does not add memory to a 16 GB Mac.
- Qwen2.5-Coder 7B is the larger option discussed (about 4.7 GB download); 3B is smaller (about 1.9 GB download) and may respond faster, with lower capability.

## 1. Install Docker Desktop

1. Download Docker Desktop for Mac from [Docker's official installation page](https://docs.docker.com/desktop/setup/install/mac-install/). Choose the Apple silicon build for an M1 Mac.
2. Open the downloaded installer, move Docker to Applications, and launch Docker Desktop.
3. Wait for Docker Desktop to report that Docker is running.

## 2. Start Ollama in Docker

In Terminal, run:

```bash
docker run -d \
  --name ollama \
  --restart unless-stopped \
  -v ollama:/root/.ollama \
  -p 11434:11434 \
  ollama/ollama
```

The first run normally says it cannot find the image locally, then downloads it. That is expected. The named volume keeps downloaded models when the container is stopped.

Check the container:

```bash
docker ps -a --filter name=ollama
```

If it is stopped, start it:

```bash
docker start ollama
```

If the initial `docker run` reports a canceled context or says the name is already in use, inspect the existing container with the `docker ps -a` command above instead of creating another one. To inspect logs:

```bash
docker logs ollama
```

Confirm the service responds from the Mac:

```bash
curl http://localhost:11434
```

Expected response: `Ollama is running`.

## 3. Download one or both coding models

Download the 7B model:

```bash
docker exec -it ollama ollama pull qwen2.5-coder:7b
```

Download the smaller 3B model:

```bash
docker exec -it ollama ollama pull qwen2.5-coder:3b
```

Check which models are installed:

```bash
docker exec -it ollama ollama list
```

The command needs the `docker` prefix. Running `exec -it ...` by itself makes the shell interpret `exec` rather than Docker.

## 4. Test model generation from Terminal

Test the 7B model (replace `7b` with `3b` to test that one):

```bash
curl http://localhost:11434/api/chat \
  -H 'Content-Type: application/json' \
  -d '{"model":"qwen2.5-coder:7b","messages":[{"role":"user","content":"Reply with only: OK"}],"stream":false}'
```

A successful response contains the model's `OK` reply. If the response says `llama-server process has terminated: signal: killed`, the model process was killed, commonly due to memory pressure. Try the 3B model, close memory-heavy apps, or check Docker Desktop's resource settings.

## 5. Install or locate Continue in IntelliJ

If Continue is not listed in IntelliJ's plugin Marketplace search, use the [Continue listing on JetBrains Marketplace](https://plugins.jetbrains.com/plugin/22707-continue), download a compatible plugin ZIP, then in IntelliJ open **Settings/Preferences → Plugins → gear menu → Install Plugin from Disk** and select that ZIP.

The **Tools → Continue** settings page shown in IntelliJ is for remote config syncing and editor options. Its Remote Config Server URL and User Token fields are not where the local Ollama model is configured; leave them blank for this setup.

## 6. Configure Continue with both models

Continue's macOS global configuration is usually:

```text
~/.continue/config.yaml
```

You can open it in a text editor. If the file does not exist, create it. Put this complete configuration in the file:

```yaml
name: Local Ollama
version: 0.0.1
schema: v1

models:
   - name: Qwen Coder 7B
     provider: ollama
     model: qwen2.5-coder:7b
     apiBase: http://localhost:11434
     roles:
        - chat
        - edit

   - name: Qwen Coder 3B
     provider: ollama
     model: qwen2.5-coder:3b
     apiBase: http://localhost:11434
     roles:
        - chat
        - edit
```

If you already have a configuration, merge the two model entries under its existing `models:` key rather than adding a second `models:` key. The model field must exactly match a model shown by `ollama list`.

Save the file, then reopen or refresh the Continue tool window/model selector. Open **View → Tool Windows → Continue** if you do not see the panel. Select **Qwen Coder 7B** or **Qwen Coder 3B** in Continue's model selector. The Continue chat shortcut on macOS is usually **Cmd+J**.

## 7. Troubleshooting

### Continue reports an error handling the response

1. Confirm the container is running:

   ```bash
   docker ps --filter name=ollama
   ```

2. Confirm Ollama answers at `http://localhost:11434` or run `curl http://localhost:11434`.
3. Confirm the requested model is installed:

   ```bash
   docker exec -it ollama ollama list
   ```

4. Test generation with the API command in section 4. A successful root URL check only proves the server is reachable; it does not prove that the model can generate a response.
5. Verify Continue's YAML uses `provider: ollama`, the exact model tag, and `apiBase: http://localhost:11434`.
6. Save the config and restart IntelliJ. If it still fails, check Continue's logs from its tool window menu and inspect the error for the failed request.

### The model is slow in IntelliJ

Continue may send surrounding code and conversation context with the prompt. The Docker Desktop setup also lacks Mac GPU acceleration. Try a short prompt, or select the 3B model for faster responses. To see whether Ollama has a model loaded:

```bash
docker exec -it ollama ollama ps
```

For better Mac performance, consider installing Ollama's native Mac app and using that service instead of the Docker container.

### Stop or restart Ollama

```bash
docker stop ollama
docker start ollama
```

## Install Ollama natively on Mac without Docker

Use this option if you want Ollama to run directly on macOS and use Mac GPU acceleration.

1. Download Ollama for Mac from [ollama.com/download](https://ollama.com/download).
2. Open the downloaded file, follow the installation steps, and launch Ollama from Applications.
3. In Terminal, download the coding model or models you want:

   ```bash
   ollama pull qwen2.5-coder:7b
   ollama pull qwen2.5-coder:3b
   ```

   Model files downloaded in Docker are separate and must be downloaded again for native Ollama.

4. Check that the service responds and the models are installed:

   ```bash
   curl http://localhost:11434
   ollama list
   ```

5. Continue can keep using `apiBase: http://localhost:11434`. Its `config.yaml` may list both models as in section 6. Save the file, refresh or reopen Continue, and choose the model from its selector.
6. After confirming native Ollama works in Continue, stop the Docker container to avoid having two Ollama services competing for port 11434:

   ```bash
   docker stop ollama
   ```

## Uninstall Ollama from Mac

To uninstall the native Mac app:

1. Quit Ollama from its menu bar icon.
2. Move Ollama from Applications to the Trash.
3. To also permanently delete downloaded models and native Ollama data, run:

   ```bash
   rm -rf ~/.ollama
   ```

   This permanently deletes the native Ollama models. Keep `~/.ollama` if you may use Ollama again.

To remove the Docker installation instead, remove its container:

```bash
docker rm ollama
```

To also delete the models downloaded in Docker, remove its volume:

```bash
docker volume rm ollama
```

Removing the Docker volume permanently deletes the Docker-downloaded models. The native `~/.ollama` data and Docker volume are separate.

## References

- [Ollama official Docker image notes](https://ollama.com/blog/ollama-is-now-available-as-an-official-docker-image)
- [Docker Desktop installation for Mac](https://docs.docker.com/desktop/setup/install/mac-install/)
- [Continue: configure Ollama](https://docs.continue.dev/customize/model-providers/top-level/ollama)
- [Continue: Ollama guide and troubleshooting](https://docs.continue.dev/guides/ollama-guide)
- [Qwen2.5-Coder model sizes](https://ollama.com/library/qwen2.5-coder)
