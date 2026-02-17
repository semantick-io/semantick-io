<figure>
  <img style="width:100%" alt="SemanticK: The assistant that actually remembers." src="https://github.com/user-attachments/assets/fb992169-dea9-4d23-8174-8a42151a8b4f" />
  <figcaption>SemanticK: The assistant that actually remembers.</figcaption>
</figure>


# Self-Hosting SemanticK

[SemanticK](https://semantick.io/landing?lang=en) is designed to be easily self-hostable, giving you full control over your data and the flexibility to use any AI model you choose.

## Quick Start with Docker

The fastest way to get SemanticK running is using our official Docker image.

### Running the Image

Run the following command in your terminal:

```bash
docker run -d --name semantick --restart always -p 5000:5000 -v ./data:/app/data ghcr.io/semantick-io/app:latest
```

For Ollama support:

Run this first so SemanticK can talk to Ollama (only needed for Linux users):
```bash
sudo mkdir -p /etc/systemd/system/ollama.service.d
sudo tee /etc/systemd/system/ollama.service.d/override.conf <<EOF
[Service]
Environment="OLLAMA_HOST=0.0.0.0"
Environment="OLLAMA_ORIGINS=*"
EOF
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

The run this:
```
docker run -d --name semantick --restart always -p 5000:5000 --add-host=host.docker.innternal:host-gateway -v ./data:/app/data ghcr.io/semantick-io/app:latest
```

### Configuration Details

| Parameter | Description |
| :--- | :--- |
| `-p 5000:5000` | Access the UI at `http://localhost:5000` |
| `-v ./data:/app/data` | Persists your chat history in a local `data` directory |
| `-e SECRET_KEY` | **Optional.** But for better security it's recommended that you enter a random string here to override the default secret key. That will be used as a secret key to encrypt your API keys in the browser storage |
| `-e SERPERAPI_API_KEY` | **Optional.** The [Serper Google search API key](https://serper.dev) for the web search plugin |

## After Installation

1. Open your browser and go to `http://localhost:5000`.
2. Click on the **Model Name** (top center) or the **Settings** icon.
3. Select **Add BYOK Model**.
4. Configure your preferred AI provider (OpenAI, Gemini, Anthropic, or any OpenAI-compatible API).

## Privacy and Security

- **Complete Offline Support**: If you are using local LLM with BYOK, you don't even need to connect to the internet to run SemanticK and still let your local LLM to use all our built-in plugins that do not require internet access like sessions search, charting and more!
- **No Token Limits**: You are only limited by your own API provider's quotas.
- **Local Encryption**: All API keys you enter are encrypted using your `SECRET_KEY`

---

Visit [https://semantick.io](https://semantick.io?lang=en) to try out our cloud version demo.
