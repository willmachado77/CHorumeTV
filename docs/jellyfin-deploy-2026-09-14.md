# Deploy inicial do Jellyfin — 2026-09-14

## Serviço

O contêiner `chorumetv-jellyfin` foi iniciado com a imagem oficial `jellyfin/jellyfin:10.11` e permaneceu saudável após a configuração inicial.

## Acesso

A porta está vinculada somente a `127.0.0.1:8096` no servidor. O acesso administrativo inicial ocorre por túnel SSH local; nenhuma porta foi aberta no UFW ou no roteador.

## Bibliotecas

- Filmes: `/media/filmes`.
- Séries: `/media/series`.
- Músicas: `/media/musicas`.
- Audiolivros: `/media/audiolivros`.

As quatro montagens de mídia foram validadas como somente leitura no contêiner.

## Política de reprodução

Para o usuário `necromind`, foram desativadas transcodificação de vídeo, transcodificação de áudio, conversão sem recodificação e transcodificação forçada de mídia remota. O servidor opera em Direct Play; clientes devem suportar o arquivo original.

## Próxima etapa

Preparar qBittorrent para downloads legais e semeadura persistente, mantendo a interface administrativa restrita.
