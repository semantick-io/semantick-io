# Self-Hosting SemanticK

SemanticK is designed to be easily self-hostable, giving you full control over your data and the flexibility to use any AI model you choose.

## Quick Start with Docker

The fastest way to get SemanticK running is using our official Docker image.

### Running the Image

Run the following command in your terminal:

```bash
docker run -d \
  --name semantick \
  --restart always \
  -p 5000:5000 \
  -v ./data:/app/data \
  -e SECRET_KEY="your-random-secret-key" \
  ghcr.io/semantick-io/app:latest
```

### Configuration Details

| Parameter | Description |
| :--- | :--- |
| `-p 5000:5000` | Access the UI at `http://localhost:5000` |
| `-v ./data:/app/data` | Persists your chat history in a local `data` directory |
| `-e SECRET_KEY` | Enter any random string here that will be used as a secret key to encrypt your API keys in the browser storage |
| `-e SERPERAPI_API_KEY` | **Optional.** The Serper Google search API key for the web search plugin |
| `ghcr.io/semantick-io/app:latest` | The official SemanticK image |

## After Installation

1. Open your browser and go to `http://localhost:5000`.
2. Click on the **Model Name** (top center) or the **Settings** icon.
3. Select **Add BYOK Model**.
4. Configure your preferred AI provider (OpenAI, Gemini, Anthropic, or any OpenAI-compatible API).

## Privacy and Security

By default, the self-hosted version runs in **Self-Hosted Mode**:
- **No Token Limits**: You are only limited by your own API provider's quotas.
- **Local Encryption**: All API keys you enter are encrypted using your `SECRET_KEY`

---

Visit our main page for more details [https://semantick.io](https://semantick.io/landing?lang=en).
