# Hardening inicial — 2026-09-14

## Objetivo

Estabelecer uma linha de base defensiva para o CHorumeTV antes da instalação de Docker, Jellyfin, qBittorrent ou exposição por domínio.

## Controles implantados

### Firewall UFW

Política aplicada:

```text
Entrada: negar por padrão
Saída: permitir por padrão
Roteamento: desabilitado
```

Única regra de entrada:

```text
SSH/TCP 22 permitido somente pela rede local
```

O UFW foi ativado somente depois da criação e confirmação da regra SSH. Uma segunda conexão SSH pela LAN foi testada com sucesso após a ativação.

### Fail2ban

O serviço foi instalado, habilitado no boot e validado como ativo.

O jail `sshd` está ativo e monitora o serviço SSH pelo journal do systemd. No momento da validação, não havia falhas de autenticação nem endereços banidos.

### Atualizações automáticas

Foram instalados `unattended-upgrades` e seus timers do APT.

Configuração criada em:

```text
/etc/apt/apt.conf.d/20auto-upgrades
```

Conteúdo:

```text
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```

Uma simulação com `unattended-upgrade --dry-run --debug` terminou com sucesso, sem atualizações elegíveis ou remoções pendentes.

## Validações executadas

```bash
sudo ufw status verbose
sudo fail2ban-client status
sudo fail2ban-client status sshd
sudo unattended-upgrade --dry-run --debug
```

## Próxima etapa

