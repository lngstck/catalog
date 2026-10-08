# Caddy mit deSEC-Modul fuer die DNS-01-Challenge.
#
# Im Betrieb "nur im Schulnetz" erreicht die Zertifizierungsstelle den Server
# nie. Ein echtes Zertifikat gibt es dann nur ueber DNS-01, und dafuer braucht
# Caddy ein Modul fuer die DNS-API — das offizielle Image hat keins.
#
# Beide Versionen bilden den Image-Tag (siehe .github/workflows/caddy-image.yml):
# ghcr.io/lngstck/caddy:<CADDY_VERSION>-desec<DESEC_VERSION ohne v>.
ARG CADDY_VERSION=2.11.7
ARG DESEC_VERSION=v1.1.0

# Der Builder laeuft auf der Plattform des Build-Rechners und cross-kompiliert
# fuer das Ziel. Das spart die QEMU-Emulation, die einen Go-Build fuer arm64
# um ein Vielfaches verlangsamen wuerde.
FROM --platform=$BUILDPLATFORM caddy:${CADDY_VERSION}-builder-alpine AS builder
ARG DESEC_VERSION
ARG TARGETOS
ARG TARGETARCH
RUN GOOS=$TARGETOS GOARCH=$TARGETARCH xcaddy build \
    --with github.com/caddy-dns/desec@${DESEC_VERSION}

FROM caddy:${CADDY_VERSION}-alpine
COPY --from=builder /usr/bin/caddy /usr/bin/caddy
