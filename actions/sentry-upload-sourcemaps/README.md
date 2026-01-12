# sentry-upload-sourcemaps

GitHub Action composta para **injetar e enviar sourcemaps de aplicações JavaScript/Node para o Sentry**, associando-os automaticamente à versão definida no `package.json`.

Funciona sem mágica: lê `name` e `version`, injeta os sourcemaps no build e faz o upload para o projeto correto no Sentry.

---

## 📌 O que esta Action faz

1. Lê o `name` e `version` do `package.json`
2. Injeta referências de sourcemap nos arquivos compilados (`./dist`)
3. Envia os sourcemaps para o Sentry usando: `package-name@version`


---

## 📦 Requisitos

- Node.js disponível no runner
- Build já gerado em `./dist`
- `package.json` na raiz do repositório
- Token do Sentry configurado como secret

---

## 🔐 Secrets necessários

| Nome                | Descrição                       |
|---------------------|---------------------------------|
| `SENTRY_AUTH_TOKEN` | Token de autenticação do Sentry |

---

## ⚙️ Inputs

| Input            | Obrigatório | Descrição                 |
|------------------|-------------|---------------------------|
| `sentry_project` | ✅           | Nome do projeto no Sentry |

---

## 🚀 Exemplo de uso

```yaml
name: Upload sourcemaps to Sentry

on:
push:
 branches: [main]

jobs:
sentry:
 runs-on: ubuntu-latest

 steps:
   - uses: actions/checkout@v4

   - uses: actions/setup-node@v4
     with:
       node-version: 20

   - run: npm ci
   - run: npm run build

   - uses: ./.github/actions/sentry-upload-sourcemaps
     with:
       sentry_project: my-sentry-project
     secrets:
       SENTRY_AUTH_TOKEN: ${{ secrets.SENTRY_AUTH_TOKEN }}
```

## Observações

A action usa `npx sentry-cli`, então não precisa instalar nada globalmente, além da declaração no `package.json`.

Se o `./dist` não existir, o upload falha. 
