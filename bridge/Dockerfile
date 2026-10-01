# Build context: whatsapp-bridge/ of github.com/verygoodplugins/whatsapp-mcp (at a release tag)
FROM golang:1.26-trixie AS build
WORKDIR /src
COPY . .
RUN CGO_ENABLED=1 go build -trimpath -ldflags="-s -w" -o /out/whatsapp-bridge .

FROM debian:trixie-slim
RUN apt-get update \
 && apt-get install -y --no-install-recommends ca-certificates curl tzdata \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY --from=build /out/whatsapp-bridge /app/whatsapp-bridge
# Session (whatsapp.db), messages.db, media and .bridge-token live in /app/store
VOLUME ["/app/store"]
CMD ["/app/whatsapp-bridge"]
