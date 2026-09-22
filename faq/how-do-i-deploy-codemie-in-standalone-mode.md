# How do I deploy CodeMie in standalone mode?

CodeMie Standalone packages the frontend, backend, and nginx into a single Docker image, paired
with a separate PostgreSQL container via Docker Compose. It is intended for proof-of-concept
work, demos, and local development — not production, which should use the AWS, Azure, or GCP
Kubernetes or On VM deployment guides.

The general flow:

1. Build the image with `standalone/build-image.sh --image-tag localhost/codemie:local` from a
   local checkout of the `codemie` backend repository (with `codemie-ui` as a sibling directory,
   or via explicit `--backend-root`/`--frontend-root` paths).
2. Copy `standalone/.env.standalone.example` to `standalone/.env.standalone` and fill in an LLM
   provider (Azure OpenAI or AWS Bedrock).
3. Start the stack with `docker compose -f standalone/docker-compose.standalone.yml up -d`.

CodeMie Standalone authenticates with the platform's built-in local provider by default — there
is no Keycloak container in the stack. By default it also runs without Elasticsearch
(`RETRIEVAL_BACKEND=none`), so Knowledge Bases, Data Sources, and code indexing are unavailable.
An optional `standalone-analytics` Compose profile adds ClickHouse and an OpenTelemetry Collector
for CLI Analytics.

## Sources

- [CodeMie Standalone Deployment Guide](https://docs.codemie.ai/admin/deployment/standalone/overview)
- [CodeMie Standalone Deployment Guide — Step by Step](https://docs.codemie.ai/admin/deployment/standalone/deployment-guide)
