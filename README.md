# Payment_token
network tokenization related 

refer:
1. https://www.mastercard.com/gateway/expertise/insights/network-tokenization-faqs.html


# Run local agent
ref:
- https://insiderllm.com/guides/run-first-local-llm/
- webUI: https://insiderllm.com/guides/open-webui-setup-guide/
> **Ollama (native Windows, port 11434) ←→ Docker Open WebUI (frontend, port 3000)**
---
## 1. Install Ollama directly on Windows (native, NOT docker)
1. Download Ollama official installer and install
2. Start Ollama service in CMD:
```bash
ollama serve
```
> Keep this CMD window open, this is your LLM backend service.
3. Pull the model you want (example qwen3.5):
```bash
ollama pull qwen3.5:latest
```
4. Verify Ollama works:
```bash
curl http://localhost:11434/api/tags
```
If returns JSON with your model name → Ollama ready.
> 📌 Ollama model storage path: `C:\Users\<YourName>\.ollama\models`

## 2. Run Open WebUI inside Docker Desktop
1. Make sure Docker Desktop is running, and **`Enable host.docker.internal` is turned on in Docker settings**
2. Run the Open WebUI container command (with host gateway for Ollama access):
```bash
docker run -d -p 3000:8080 ^
-v "%USERPROFILE%\openwebui-data:/app/backend/data" ^
--add-host=host.docker.internal:host-gateway ^
--name open-webui ^
ghcr.io/open-webui/open-webui:main
```
## 3. Configure Ollama API connection inside Open WebUI
1. Open browser: `http://localhost:3000`
2. Go to Settings → **Connections (AI group)**
3. Fill Ollama API URL to:
```
http://host.docker.internal:11434
```
> ❗ Why NOT localhost:11434?
> localhost inside docker container points to container itself, cannot reach Windows host Ollama. `host.docker.internal` is Docker’s special address to access your Windows host machine.
4. Toggle on Ollama API switch → Save.
5. Refresh page, your downloaded local Ollama models will appear in model dropdown.

## ⚠️ Common Pitfall Recap
2. Don’t use `http://localhost:11434` in OpenWebUI connections (docker can’t resolve it)
3. Keep `ollama serve` CMD open when you use the model
4. If you delete docker container, data is safe because we use Windows bind mount volume

整体感觉：不好用，又慢又笨
<img width="2980" height="1571" alt="image" src="https://github.com/user-attachments/assets/eba336f5-46e2-4818-84bc-b9455fdbc1e5" />

