# OpenClaw Graphiti Staging Without Zep

Date: 2026-05-06

This branch records the OpenClaw/Sitio Uno staging deployment of MiroFish using
Graphiti + Neo4j instead of Zep Cloud.

## Branches Imported From Upstream Community Work

The following upstream alternatives were preserved in the SiteOneTech fork:

- `experiment/graphiti-backend-pr276`
- `experiment/selfhosted-graphiti-pr334`

The active staging branch is:

- `experiment/graphiti-backend-pr276-openclaw-staging`

PR #276 was selected for staging because it adds a pluggable graph backend with
Graphiti support while changing less application surface than the larger
self-hosted Graphiti branch.

## Deployment

VM:

- GCP VM: `mirofish-simulator-01`
- Tailscale IP: `100.119.34.35`
- Production UI: `http://100.119.34.35/`
- Graphiti staging UI: `http://100.119.34.35:8082/`

Runtime paths:

- Production app: `/opt/mirofish/app`
- Graphiti staging app: `/opt/mirofish-graphiti/app`
- Staging environment: `/etc/mirofish/mirofish-graphiti.env`
- Neo4j password file: `/etc/mirofish/graphiti-neo4j.secret`

System services:

- `mirofish-backend.service` - production backend on `127.0.0.1:5001`
- `mirofish-graphiti-backend.service` - staging backend on `127.0.0.1:5002`
- `nginx` - production on `80`, staging on `8082`

Containers:

- `mirofish-graphiti-neo4j` using `neo4j:5.26-community`
- `mirofish-graphiti-ollama` using `ollama/ollama:latest`

Local embedder:

- OpenAI-compatible endpoint: `http://127.0.0.1:11434/v1`
- Model: `nomic-embed-text`
- Dimension: `768`

## Configuration

The staging environment removes `ZEP_*` variables and uses:

```bash
GRAPH_BACKEND=graphiti
GRAPHITI_URI=bolt://127.0.0.1:7687
GRAPHITI_USER=neo4j
GRAPHITI_DATABASE=neo4j
GRAPHITI_LLM_CLIENT_MODE=generic
GRAPHITI_USE_RESPONSE_FORMAT=false
GRAPHITI_EMBEDDER_API_KEY=ollama
GRAPHITI_EMBEDDER_BASE_URL=http://127.0.0.1:11434/v1
GRAPHITI_EMBEDDER_MODEL=nomic-embed-text
GRAPHITI_EMBEDDER_DIM=768
GRAPH_SEARCH_RERANKER=rrf
GRAPH_SEARCH_APP_RERANKER=lexical
GRAPHITI_ENABLE_CROSS_ENCODER=false
```

`GRAPHITI_PASSWORD`, LLM API keys, and other secrets must remain outside git.

## Corrections Applied

1. **Python compatibility:** `camel-oasis==0.2.5` requires Python `<3.12`. The
   staging deployment uses Python `3.11.15` installed with `uv`.
2. **Neo4j package conflict:** `camel-oasis` pins `neo4j==5.23.0`, while
   Graphiti requires `neo4j>=5.26.0`. Staging installs the base dependencies and
   then forces `neo4j==5.26.0` plus `graphiti-core==0.28.2`.
3. **Provider response format:** DeepSeek-compatible chat completions rejected
   `response_format=json_schema`. The branch adds a prompt-JSON fallback gated by
   `GRAPHITI_USE_RESPONSE_FORMAT=false`.
4. **Language/UI:** The branch pulls the multilingual UI from `main`, restores
   the language selector, and defaults staging to Spanish.
5. **Isolation:** The staging app runs in `/opt/mirofish-graphiti` on port `8082`
   so production on port `80` remains unchanged.

## Verification

Health checks:

```bash
curl -fsS http://127.0.0.1:5002/health
curl -fsS http://127.0.0.1:8082/health
curl -fsS http://100.119.34.35:8082/health
```

Embedding check:

```bash
curl -fsS http://127.0.0.1:11434/v1/embeddings \
  -H 'Content-Type: application/json' \
  -d '{"model":"nomic-embed-text","input":"hello"}'
```

Neo4j check:

```bash
docker exec mirofish-graphiti-neo4j cypher-shell \
  -u neo4j -p "$(sudo cat /etc/mirofish/graphiti-neo4j.secret)" \
  'RETURN 1 AS ok'
```

Graphiti smoke test:

```bash
sudo -u mirofish bash -lc '
  set -a
  source /etc/mirofish/mirofish-graphiti.env
  set +a
  cd /opt/mirofish-graphiti/app
  backend/.venv/bin/python backend/scripts/graphiti_smoke_test.py \
    --chunk "Alice works at Sitio Uno GCP." \
    --query "Where does Alice work?"
'
```

Validated result:

- Backend: `graphiti`
- Embedder: `nomic-embed-text`
- Zep Cloud: not used
- UI: multilingual selector present, default Spanish

