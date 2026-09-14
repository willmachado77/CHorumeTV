# CHorumeTV

> Servidor caseiro autocontrolado para mídia, música e biblioteca digital.

## Estado do documento

- Versão: 0.1.0
- Data: 2026-09-14
- Estado do projeto: MVP instalado; endurecimento inicial pendente de implantação
- Licença recomendada para documentação: CC BY 4.0

## 1. Propósito

O CHorumeTV é um servidor caseiro educacional e de uso pessoal, construído para organizar, preservar e disponibilizar acervo próprio ou conteúdo distribuído legalmente. A primeira entrega prioriza segurança, reprodutibilidade, baixo consumo de recursos e compreensão técnica do sistema.

O projeto também é um artefato de formação e portfólio em infraestrutura, DevOps e segurança operacional. Por isso, decisões, comandos, evidências de validação e mudanças relevantes devem ser registrados de maneira que uma pessoa consiga entender o motivo de cada escolha e reproduzir o ambiente sem receber segredos, endereços privados ou dados pessoais.

## 2. Objetivos

### Objetivo principal

Disponibilizar uma plataforma doméstica baseada em Debian e Docker para reprodução de filmes, séries, música e audiolivros, com biblioteca digital estruturada e acesso remoto seguro por domínio próprio.

### Objetivos específicos

- Executar Jellyfin em contêiner para organizar e servir mídia.
- Manter qBittorrent exclusivamente para conteúdo legal, com semeadura contínua e sem duplicação desnecessária de dados.
- Preparar estrutura para e-books, PDFs e quadrinhos; futuramente, integrar Kavita.
- Restringir administração a canais privados e rastreáveis.
- Expor ao público somente o serviço de mídia necessário, por HTTPS, em etapa posterior.
- Documentar o ambiente em Git e publicar documentação sanitizada no GitHub.

## 3. Escopo do MVP

O MVP foi dimensionado para o hardware disponível. Ele não é um servidor de transcodificação pesada nem uma solução de alta disponibilidade.

| Aspecto | Decisão do MVP |
| --- | --- |
| Sistema | Debian 13 minimal |
| Orquestração | Docker Compose |
| Mídia | Jellyfin |
| Transferência legal | qBittorrent |
| Acesso administrativo | SSH, posteriormente Tailscale |
| Acesso público | Caddy + HTTPS + domínio, somente após hardening e testes |
| Biblioteca digital | Estrutura de arquivos agora; Kavita em fase posterior |
| Reprodução | Direct Play prioritário, até 1080p quando possível |
| Público inicial | Um usuário por vez e dispositivos conhecidos |

### Limitações conhecidas

O equipamento utiliza CPU Intel Core 2 Duo E7500, 4 GB de RAM, HDD único de 500 GB, rede Fast Ethernet de 100 Mbps e vídeo integrado antigo. Ele é suficiente para aprendizado, organização de acervo e reprodução direta em clientes compatíveis, mas não para conversão de vídeo pesada ou múltiplos usuários simultâneos.

O único HDD não constitui backup. Mídias e documentos insubstituíveis só devem ser considerados protegidos depois da implantação de cópia externa e teste de restauração.

## 4. Arquitetura aprovada

```text
Clientes autorizados
        │ HTTPS (futuro domínio)
        ▼
      Caddy
        │ rede Docker interna
        ▼
     Jellyfin ───────────────► /data/media
        │                     /data/biblioteca
        │
Tailscale ─► SSH e administração privada

qBittorrent ─────────────────► /data/torrents
```

Princípios:

1. Interfaces administrativas não serão publicadas diretamente na internet.
2. Cada serviço receberá apenas os volumes e permissões de que precisa.
3. O proxy reverso será a única entrada pública planejada.
4. Segredos jamais serão enviados ao repositório; arquivos `.env` reais ficarão fora do Git.
5. Ações destrutivas exigem confirmação explícita e validação do alvo.

## 5. Estrutura de dados

Inspirada na filosofia do TRaSH Guides e adaptada ao projeto:

```text
/srv/chorumetv/data
├── torrents
│   ├── livros
│   ├── filmes
│   ├── musicas
│   └── series
├── media
│   ├── filmes
│   ├── series
│   ├── musicas
│   └── audiolivros
└── biblioteca
    ├── ebooks
    ├── quadrinhos
    └── documentos
```

Downloads e mídia final devem permanecer no mesmo filesystem. Isso permite movimentações atômicas e hardlinks quando serviços futuros de organização forem adotados, evitando a criação de cópias físicas desnecessárias. Hardlinks economizam espaço; não substituem backup.

Dentro dos contêineres, o caminho raiz será `/data`, preservando uma visão consistente entre serviços.

## 6. Decisões arquiteturais registradas

### ADR-001 — Debian minimal sem interface gráfica

**Decisão:** Debian 13 minimal, com SSH e utilitários padrão; sem ambiente gráfico.

**Motivo:** reduz consumo de memória, pacotes instalados, atualizações e superfície de ataque. A administração será remota por SSH/Tailscale.

### ADR-002 — Reprodução direta como padrão

**Decisão:** priorizar arquivos reproduzíveis diretamente pelo cliente.

**Motivo:** o hardware não oferece capacidade útil de transcodificação por hardware. Arquivos H.264/AVC com áudio AAC ou AC3, em MP4/MKV e normalmente até 1080p, oferecem maior compatibilidade. Legendas em texto, como SRT, são preferíveis.

### ADR-003 — Uma raiz de dados no mesmo filesystem

**Decisão:** usar `/srv/chorumetv/data` como raiz do acervo.

**Motivo:** preserva consistência de caminhos, simplifica permissões e permite hardlinks/movimentações atômicas entre download e biblioteca final.

### ADR-004 — Administração privada; exposição pública mínima

**Decisão:** SSH, qBittorrent e painéis de gerenciamento ficarão restritos à LAN/Tailscale. Apenas Jellyfin será candidato à exposição pública, por Caddy e HTTPS.

**Motivo:** reduz de forma substancial a superfície disponível a ataques externos.

## 7. Registro retroativo de implantação

| Etapa | Resultado | Evidência/observação |
| --- | --- | --- |
| Análise do hardware | Concluída | Plataforma adequada a MVP de Direct Play; limitações documentadas |
| Mídia de instalação | Concluída | ISO Debian `debian-13.7.0-amd64-netinst.iso` validada por SHA-512 oficial |
| Pendrive bootável | Concluído | Rufus, modo DD Image; alvo USB de aproximadamente 30 GB confirmado |
| Instalação Debian | Concluída | Debian 13 minimal, boot pelo HDD interno |
| Hostname | Concluído | `chorumetv` |
| Usuário operacional | Concluído | `necromind` |
| Particionamento | Concluído | Disco interno de aproximadamente 500 GB, esquema guiado, filesystem ext4, swap de 4 GB |
| Rede local | Concluída | DHCP funcional; endereço privado omitido desta documentação pública |
| SSH | Concluído | Serviço ativo e conexão remota validada com chave ED25519 registrada no cliente |
| Atualizações | Concluída | `apt update` executado; sem pacotes pendentes naquele momento |
| Administração delegada | Concluída | `sudo` instalado; `necromind` incluído no grupo `sudo`; validação de privilégios concluída |

## 8. Linha de base validada

- Sistema operacional: Debian GNU/Linux 13 (trixie), arquitetura x86_64.
- Kernel observado: série 6.12 Debian.
- Armazenamento: aproximadamente 454 GiB em `/`, ext4, com cerca de 430 GiB livres após instalação.
- Memória: aproximadamente 3,8 GiB; swap de 4 GiB.
- Rede: interface Ethernet ativa com DHCP na LAN privada.
- SSH: ativo.
- Sudo: instalado e validado para `necromind`.

Uma consulta de inventário pode emitir aviso sobre UUID de produto indisponível por limitação do firmware antigo. Isso não afeta a operação do servidor.

## 9. Política de segurança operacional

### Regras obrigatórias

1. Não publicar portas ou serviços sem decisão documentada.
2. Não usar `root` para trabalho cotidiano; usar `sudo` a partir de conta nominal.
3. Não inserir senhas, tokens, chaves privadas, IPs públicos, nomes de domínio ainda não publicados ou backups pessoais no repositório.
4. Antes de ativar firewall, validar a regra que preserva a sessão administrativa.
5. Antes de remover, formatar ou sobrescrever dados, confirmar dispositivo, caminho e efeito esperado.
6. Testar acesso remoto por chave SSH antes de desabilitar autenticação por senha.
7. Atualizações, mudanças de firewall, portas, volumes Docker e contas de acesso devem gerar registro documental e commit.

### Próxima implementação de hardening

Autorizada e ainda pendente de execução:

- instalar UFW, `unattended-upgrades` e fail2ban;
- estabelecer política padrão de negar entrada;
- permitir SSH apenas a partir da LAN local;
- habilitar atualizações automáticas de segurança;
- habilitar proteção contra tentativas repetidas de autenticação.

## 10. Política de documentação e Git

### Estrutura do repositório planejada

```text
chorumetv/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── SECURITY.md
├── docs/
│   ├── arquitetura.md
│   ├── instalacao-debian.md
│   ├── hardening.md
│   ├── operacao.md
│   ├── rede-e-acesso.md
│   └── adr/
├── infra/
│   ├── compose/
│   └── env/
└── scripts/
```

### Convenções

- Commits em português, com escopo: `docs:`, `infra:`, `security:`, `feat:`, `fix:`.
- Cada documento deve informar data de revisão e estado: planejado, validado, depreciado ou substituído.
- Comandos devem trazer contexto, pré-requisitos, efeito e validação esperada.
- Instruções específicas ao ambiente pessoal devem ser separadas de exemplos genéricos para a comunidade.
- Evidências de sucesso e falha devem ser anotadas sem dados sensíveis.

## 11. Próximas fases

1. Publicar este documento e estrutura inicial do repositório.
2. Aplicar hardening inicial autorizado.
3. Criar chave SSH, testar e então reduzir autenticação por senha.
4. Definir reserva DHCP e estratégia de endereçamento interno.
5. Preparar `/srv/chorumetv/data`, usuários/grupos e permissões de serviço.
6. Instalar Docker Engine e Docker Compose com fonte oficial.
7. Publicar Jellyfin e qBittorrent somente na rede privada.
8. Validar reprodução local, semeadura e consumo de recursos.
9. Implantar Tailscale para administração privada.
10. Implantar domínio, Caddy e HTTPS somente após controles e testes.
11. Definir e testar backup externo e recuperação.

## 12. Manutenção recorrente

Foi criada uma revisão mensal, no primeiro domingo de cada mês às 10h (horário de Brasília), para revisar atualizações, disco, backups, serviços, firewall, acessos remotos, logs e pendências documentais.

---

Este documento registra o estado inicial e deve evoluir por meio de revisões versionadas. Ele não contém credenciais, IP público, domínio, hashes de chaves, nem detalhes que aumentem desnecessariamente a exposição do servidor.
