# Estrutura de dados — 2026-09-14

## Objetivo

Registrar a estrutura persistente criada antes da instalação dos contêineres.

## Raiz de dados

A raiz é `/srv/chorumetv/data`, no filesystem ext4 local do servidor.

## Estrutura implantada

- `torrents/incompletos`, `torrents/livros`, `torrents/filmes`, `torrents/musicas` e `torrents/series`
- `media/filmes`, `media/series`, `media/musicas` e `media/audiolivros`
- `biblioteca/ebooks`, `biblioteca/quadrinhos` e `biblioteca/documentos`

## Permissões

Os diretórios operacionais pertencem a `root:chorumetv`, usam modo `2775` e bit setgid. Assim, novos arquivos herdam o grupo `chorumetv`.

O usuário administrativo `necromind` pertence ao grupo `chorumetv` e foi validado com criação e remoção de um arquivo temporário.

## Próxima etapa

Instalar o Docker Engine e montar os serviços com caminhos consistentes sob `/data` nos contêineres.
