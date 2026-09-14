# Preparação do Jellyfin — 2026-09-14

## Escopo

Preparação da configuração do Jellyfin sem iniciar contêiner, baixar imagem ou expor serviço.

## Compose

O arquivo `infra/compose/jellyfin.compose.yaml` foi validado com `docker compose config --quiet`.

- Imagem oficial: `jellyfin/jellyfin:10.11`.
- Serviço executará como UID 1000 e GID 989 (`necromind:chorumetv`).
- A porta HTTP é vinculada somente a `127.0.0.1:8096`.
- A mídia é montada em modo somente leitura.
- `no-new-privileges` e remoção de capacidades Linux foram configurados.

## Dados persistentes

Foram criados `/srv/chorumetv/appdata/jellyfin/config` e `/srv/chorumetv/appdata/jellyfin/cache` com proprietário `necromind:chorumetv` e modo `2770`.

## Próxima etapa

Revisar o Compose final e autorizar separadamente o primeiro início do contêiner.
