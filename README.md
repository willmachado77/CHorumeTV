# CHorumeTV

Servidor caseiro autocontrolado para mídia, música e biblioteca digital, construído com foco em segurança, reprodutibilidade e formação prática em DevOpsSec.

## Estado atual

O MVP está instalado sobre Debian 13 minimal. SSH e administração por `sudo` foram validados. O endurecimento inicial, Docker e os serviços de mídia ainda serão implantados de forma documentada.

## Objetivos

- Organizar filmes, séries, músicas e audiolivros com Jellyfin.
- Manter semeadura de conteúdo legalmente distribuído com qBittorrent.
- Preparar e-books, documentos e quadrinhos para futura integração com Kavita.
- Permitir acesso externo somente após hardening, por domínio próprio e HTTPS.
- Produzir documentação reutilizável para a comunidade e portfólio.

## Princípios

1. Interfaces administrativas não serão expostas diretamente à internet.
2. Administração ocorrerá por SSH e, futuramente, Tailscale.
3. Segredos e dados privados não entram no Git.
4. Mudanças destrutivas exigem confirmação e validação do alvo.
5. Mudanças relevantes geram documentação, validação e commit.

## Documentação

- [Documento fundacional v0.1](docs/documento-fundacional-v0.1.md)
- [Política de segurança](SECURITY.md)
- [Como contribuir](CONTRIBUTING.md)

## Aviso legal

O projeto destina-se a acervo próprio e conteúdo distribuído legalmente. O operador é responsável por respeitar direitos autorais, licenças e leis aplicáveis.
