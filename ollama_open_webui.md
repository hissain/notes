### Download info

1. Install ollama: https://github.com/ollama/ollama/blob/main/docs/linux.md
2. Install open-webUI: https://docs.openwebui.com/getting-started/

## Open-WebUI
Step 1: Pull the Open WebUI Image
Start by pulling the latest Open WebUI Docker image from the GitHub Container Registry.
```
docker pull ghcr.io/open-webui/open-webui:main
```
Step 2: Run the Container
Run the container with default settings. This command includes a volume mapping to ensure persistent data storage.
```
docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main
```
Optional, with GPU
```
docker run -d -p 3000:8080 --gpus all -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:cuda
```

Start open-webui in localhost (accessible from LAN)

```open-webui serve```

## Ollama

To install Ollama, run the following command:
```
curl -fsSL https://ollama.com/install.sh | sh
```

Start ollama server
```
ollama serve
```

Start ollama server in local PC (accessible from LAN)
```
OLLAMA_HOST=127.0.0.1:11434 ollama serve
```
