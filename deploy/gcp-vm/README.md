# MiroFish on OpenClaw GCP VM

Production layout used for the SitioUno/OpenClaw private GCP deployment:

- VM: private Compute Engine instance in `openclaw-vpc/openclaw-subnet`
- UI: static Vite build served by Nginx on port 80
- API: Flask app behind Gunicorn on `127.0.0.1:5001`
- Browser route: `http://<tailscale-ip>/`
- Persistent app data: `/opt/mirofish/app/backend/uploads`
- Secrets: `/etc/mirofish/mirofish.env`, mode `0600`, not committed

Build command:

```bash
cd /opt/mirofish/app
npm ci
npm ci --prefix frontend
cd backend && uv sync --frozen && uv pip install --python .venv/bin/python gunicorn
cd ../frontend && VITE_API_BASE_URL=/api npm run build
```

Service commands:

```bash
sudo systemctl status mirofish-backend nginx --no-pager
curl -fsS http://127.0.0.1/health
```
