# Rede Docker — 2026-09-14

## Objetivo

Estabelecer uma rede compartilhada para os serviços do CHorumeTV antes da implantação de contêineres.

## Rede criada

- Nome: `chorumetv_backend`.
- Driver: `bridge`.
- Escopo: `local` ao host Docker.
- Rótulos: `com.chorumetv.managed=true` e `com.chorumetv.purpose=backend`.

## Segurança

Criar uma rede Docker não publica portas. O acesso externo dependerá exclusivamente de portas declaradas nos serviços; nesta etapa nenhuma porta foi declarada nem o UFW foi alterado.

A rede não usa o modo `internal`, permitindo saída controlada para atualizações, metadados e downloads legais. Interfaces administrativas permanecerão sem exposição pública.

## Próxima etapa

Criar os arquivos Compose e a política de publicação de portas para Jellyfin e qBittorrent.
