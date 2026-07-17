# n8n Local Setup

This repository provides a minimal local setup for running [n8n](https://n8n.io/) with Docker Compose and exposing it through [ngrok](https://ngrok.com/) when public webhook access is required.

## Requirements

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) or another Docker engine with Compose support
- [ngrok](https://ngrok.com/) installed and authenticated
- An available port on `5678`

## How it works

The n8n container runs locally on `http://localhost:5678`.

`N8N_HOST` is set from `NGROK_HOST` env var, and the public URLs are derived from that same host inside Docker Compose.

`N8N_MCP_BASE_URL` also derives from `NGROK_HOST`, so MCP-related features resolve against the same public endpoint.

## Setup

1. Start ngrok and expose port `5678`.

	If you use a reserved ngrok domain, run:

	```bash
	ngrok http --url=<your-ngrok-domain> 5678
	```

	If you do not use a reserved domain, start ngrok normally and copy the public URL it creates.

2. Create a `.env` file in the project root.

	```env
	NGROK_HOST=<your-ngrok-domain>
	```

3. Start n8n with Docker Compose.

	```bash
	docker compose up -d
	```

4. Open n8n in your browser.

	```text
	http://localhost:5678
	```

## Useful commands

```bash
docker compose up -d        # Start the services in detached mode
docker compose ps           # Show the running containers and their status
docker compose logs -f n8n  # Stream the n8n container logs in real time
docker compose restart      # Restart the Compose services without removing data
docker compose down         # Stop and remove the Compose services
```

## Notes

- The n8n image is pinned to version `2.30.4` for reproducible local setups.
- Data is stored in the `n8n_data` Docker volume, so it persists across container restarts.
- If ngrok changes its public host, update `NGROK_HOST`, then restart the n8n container.
