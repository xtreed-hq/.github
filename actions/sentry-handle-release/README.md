# Handle Sentry Releases

GitHub Composite Action para **criar** e **finalizar** releases no Sentry de forma automatizada.

## O que esta action faz

Esta action encapsula chamadas ao `sentry-cli` para gerenciar releases no Sentry, evitando duplicação de lógica em
workflows.

Dependendo do uso no workflow chamador, ela pode ser utilizada para:

- Criar uma nova release no Sentry
- Finalizar uma release existente no Sentry

## Requisitos

- `SENTRY_AUTH_TOKEN` disponível como secret no workflow
- `sentry-cli` acessível via `npx`
- Organização e projeto existentes no Sentry

## Inputs

| Nome                | Obrigatório | Descrição                                       |
|---------------------|-------------|-------------------------------------------------|
| `sentry_auth_token` | sim         | Token de autenticação no Sentry                 |
| `sentry_org`        | sim         | Slug da organização no Sentry                   |
| `sentry_project`    | sim         | Slug do projeto no Sentry                       |
| `version`           | sim         | Versão da release a ser criada/finalizada       |
| `action`            | sim         | Ação executada na release [`create`/`finalize`] |

## Exemplo de uso

```yaml
- uses: xtreed-hq/.github/actions/handle-sentry-releases
  with:
    sentry_org: my-org
    sentry_project: my-project
    sentry_auth_token: env.AUTH_TOKEN
    action: 'create'
    version: my-package@1.2.3
```

## Observações

- Esta action **não** faz deploy.
- A versão passada deve refletir exatamente o artefato publicado.
- A autenticação com o Sentry é responsabilidade do workflow chamador.
