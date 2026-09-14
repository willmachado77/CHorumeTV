# Docker Engine — 2026-09-14

## Escopo

Instalação do Docker Engine no host Debian 13 `chorumetv`, antes da criação dos contêineres de mídia.

## Componentes e origem

- Docker Engine 29.8.0 e Docker Compose v5.5.1.
- Pacotes obtidos do repositório estável oficial `download.docker.com` para Debian `trixie` e arquitetura `amd64`.
- Chave pública APT instalada em `/etc/apt/keyrings/docker.asc`.

## Validação

O serviço Docker iniciou e foi habilitado pelo systemd. O teste oficial `hello-world` validou a comunicação cliente-daemon, o download de imagem e a execução de contêiner. O contêiner foi removido com `--rm` e a imagem de teste foi removida após a validação.

## Segurança adotada

- `necromind` não pertence ao grupo privilegiado `docker`; operações administrativas usam `sudo docker`.
- Nenhuma porta de contêiner foi publicada e nenhuma regra do UFW foi alterada.
- A rede e qualquer publicação de portas serão revisadas antes da implantação dos serviços, pois portas publicadas pelo Docker podem contornar regras do UFW.

## Próxima etapa

Definir a política de rede dos contêineres antes de implantar Jellyfin e qBittorrent.
