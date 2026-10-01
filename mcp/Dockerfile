# Build context: whatsapp-mcp-server/ of github.com/verygoodplugins/whatsapp-mcp (at a release tag)
FROM ghcr.io/astral-sh/uv:latest AS uv

FROM python:3.13-slim
RUN apt-get update \
 && apt-get install -y --no-install-recommends ffmpeg \
 && rm -rf /var/lib/apt/lists/*
COPY --from=uv /uv /usr/local/bin/uv
WORKDIR /app
COPY . .
RUN uv sync --frozen --no-dev && rm -rf /root/.cache
CMD ["/app/.venv/bin/python", "main.py"]
