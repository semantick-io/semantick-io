<figure>
  <img style="width:100%" alt="SemanticK: The assistant that actually remembers." src="https://github.com/user-attachments/assets/fb992169-dea9-4d23-8174-8a42151a8b4f" />
  <figcaption>SemanticK: The assistant that actually remembers.</figcaption>
</figure>


# Self-Hosting SemanticK

[SemanticK](https://semantick.io/landing?lang=en) is designed to be easily self-hostable, giving you full control over your data and the flexibility to use any AI model you choose.

## Quick Start with Docker

The fastest way to get SemanticK running is using our official Docker image.

### Running the Image

To enable all features, including the **Linux Sandbox (Code Execution)** and **Google Drive Integration**, run the following command:

```bash
docker run -d --name semantick --restart always \
  -p 5000:5000 \
  -v ./data:/app/data \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -e GOOGLE_CLIENT_ID="your-google-client-id" \
  -e GOOGLE_CLIENT_SECRET="your-google-client-secret" \
  ghcr.io/semantick-io/app:latest
```

For Ollama support (Linux users):

Run this first to allow the Docker container to communicate with Ollama on the host:

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
| `-v /var/run/docker.sock:/var/run/docker.sock` | **Required** for the Linux Sandbox (Code Execution) to work. Allows SemanticK to create sibling containers for isolated code execution. |
| `-e SECRET_KEY` | **Optional.** Recommended random string to encrypt API keys in browser storage. |
| `-e SERPERAPI_API_KEY` | **Optional.** For the web search plugin ([serper.dev](https://serper.dev)). |
| `-e GOOGLE_CLIENT_ID` | **Optional.** For Google Drive integration. |
| `-e GOOGLE_CLIENT_SECRET` | **Optional.** For Google Drive integration. |
| `-e PASSCODES` | **Optional.** A comma-separated list of passcodes to password-protect your instance. |
| `-e TERMINAL_IMAGE` | **Optional.** The image used for the sandbox. Defaults to `python:3.12-alpine`. For better support, use `ghcr.io/semantick-io/app-terminal:latest`. |

## Advanced Features

### Linux Sandbox (Code Execution)

SemanticK uses Docker to run code in an isolated environment. This requires mounting the host's Docker socket (`/var/run/docker.sock`). 

When a user executes code or uses a tool that requires a terminal, SemanticK will:
1. Create a sibling container named `semantick-<id>`.
2. Execute the code within that container.
3. Automatically clean up or stop the container after inactivity.

**Security Note:** Mounting `docker.sock` gives the container significant privileges over the host. Ensure you trust the environment where you are running SemanticK.

### Google Drive Integration

To enable Google Drive integration:
1. Create a project in the [Google Cloud Console](https://console.cloud.google.com/).
2. Enable the **Google Drive API**.
3. Create **OAuth 2.0 Client IDs** (Web application).
4. Add the following to the **Authorized redirect URIs** (adjust host/protocol to match your deployment):
   - `http://localhost:5000/signin-google` (regular Google sign-in)
   - `http://localhost:5000/signin-google-drive` (Google Drive permission grant)
5. Provide the `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET` as environment variables.

Once configured, you can mount your Google Drive directly into the Linux Sandbox for seamless file manipulation.

Notes:
- If you serve SemanticK behind a reverse proxy or on HTTPS, use the external origin in the redirect URIs (e.g. `https://your.domain/signin-google` and `https://your.domain/signin-google-drive`).
- Both redirect URIs are required: the app performs a standard Google login first, and only asks for Drive scope when you choose to sync Google Drive.

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
