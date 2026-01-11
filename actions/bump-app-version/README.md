# Version Management Workflow

Workflow reutilizável do GitHub Actions para **gerenciamento de versão de aplicações** após deploy bem-sucedido.

## Objetivo

Este workflow orquestra:

1. Bump de versão do `package.json`
2. Criação de tag Git para a versão atual
3. Commit e push da nova versão
4. Integração com releases do Sentry

Ele foi desenhado para ser chamado por outros workflows via `workflow_call` ou executado manualmente via
`workflow_dispatch`.

## Como funciona

O fluxo geral é:

1. Lê a versão atual da aplicação
2. Cria uma tag Git com essa versão
3. Incrementa a versão (patch/minor)
4. Publica a nova versão no repositório

## Triggers suportados

### `workflow_dispatch`

Permite execução manual, com definição explícita dos parâmetros.

### `workflow_call`

Permite reutilização por outros workflows ou repositórios.

## Inputs

| Nome            | Obrigatório | Descrição                                 |
|-----------------|-------------|-------------------------------------------|
| `commit_branch` | sim         | Branch onde o commit de versão será feito |
| `bump_type`     | não         | Tipo de bump (`patch`, `minor`)           |

## Requisitos

- Repositório com `package.json`
- Permissão de escrita em `contents`
- Secrets configurados para integração com Sentry

## Observações

- A tag criada sempre representa a versão **já deployada**
- A nova versão gerada representa o **próximo release**
- O workflow evita loops de execução quando bem configurado
