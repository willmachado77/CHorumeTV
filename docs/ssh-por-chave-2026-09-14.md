# SSH por chave — 2026-09-14

## Objetivo

Eliminar autenticação remota por senha no CHorumeTV, reduzindo tentativas de força bruta e removendo a possibilidade de login SSH direto como `root`.

## Chave administrativa

Foi criada no computador administrativo `mindhacksec` uma chave ED25519 exclusiva para o CHorumeTV, protegida por passphrase.

Somente a chave pública foi instalada para o usuário `necromind` no servidor.

## Configuração aplicada

Arquivo:

```text
/etc/ssh/sshd_config.d/10-chorumetv-hardening.conf
```

Diretivas:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
AuthenticationMethods publickey
```

## Validação

Antes da aplicação, a sintaxe foi validada com:

```bash
sudo /usr/sbin/sshd -t
```

O serviço foi recarregado, permaneceu ativo e um novo login foi testado explicitamente com:

```bash
ssh -i ~/.ssh/id_ed25519_chorumetv \
  -o IdentitiesOnly=yes \
  -o PreferredAuthentications=publickey \
  -o PasswordAuthentication=no \
  necromind@SERVIDOR \
  'whoami && hostnamectl --static'
```

O teste retornou o usuário `necromind` e o hostname `chorumetv`.

## Regra operacional

Antes de remover ou substituir uma chave autorizada, adicionar e testar a chave substituta em uma segunda sessão SSH. Não reabilitar senha como atalho operacional.
