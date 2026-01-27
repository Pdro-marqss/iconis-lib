# Processo de Release — @iconis/icons

Este documento descreve o fluxo padrão para:
- testar a lib localmente
- gerar build
- validar o conteúdo do pacote
- versionar e publicar no npm
- checar autenticação/registry
- entender a numeração de versões (semver)

---

## Estrutura (o que vai pro Git vs npm)

### Vai para o Git (repositório)
- `src/`
- `package.json`
- `package-lock.json`
- `README.md`
- `LICENSE`
- `tsconfig.json`
- `.gitignore`
- `PROCESSO-RELEASE.md`

### NÃO vai para o Git
- `node_modules/`
- `dist/`
- `*.tgz` (gerado pelo `npm pack`)

> Regra: Git guarda **source**, npm guarda **artefato compilado**.

---

## Pré-check (npm / registry / login)

### Conferir registry (deve ser o oficial)
```bash
npm config get registry
```
Deve retornar: 
- https://registry.npmjs.org/

Se estiver diferente, rode o comando:
```bash
npm config set registry https://registry.npmjs.org/
```

### Login e verificação de usuário

```bash
npm login
npm whoami
```

Caso o token esteja expirado/revogado, refaça:

```bash
npm logout
npm login
```

---

## Comandos importantes

### `npm run build`
Gera o build da lib usando `tsup`.

Resultados esperados:
- `dist/index.js` (ESM)
- `dist/index.cjs` (CommonJS)
- `dist/index.d.ts` (tipos para autocomplete)
- `dist/index.d.cts` (tipos para CJS)

Quando usar:
- antes de testar local
- antes de publicar (o publish também roda o build via `prepublishOnly`)

---

### `npm pack`
Gera um arquivo `.tgz` igual ao que será publicado no npm.

Quando usar:
- para validar se o pacote está correto (o que entra no publish)
- para instalar localmente em outro projeto sem publicar no npm

O arquivo gerado (exemplo):
- `iconis-icons-0.1.0.tgz`

> Importante: o `.tgz` é temporário e não deve ir para o Git.

---

### `npm view @iconis/icons`
Mostra dados do pacote no registry (versão, dist-tags, etc).
Serve para checar se:
- o pacote existe
- a versão publicada está correta

Exemplos:

```bash
npm view @iconis/icons
npm view @iconis/icons versions
```

---

### `npm version patch|minor|major`
Atualiza a versão no `package.json` e cria uma tag/commit (se estiver usando git).

- `patch` → correções pequenas / docs (ex: `0.1.0` → `0.1.1`)
- `minor` → novos ícones/funcionalidades sem quebrar (ex: `0.1.0` → `0.2.0`)
- `major` → mudança que quebra API (ex: `0.1.0` → `1.0.0`)

Também cria commit e tag (quando o repositório está com git):
- commit: `vX.Y.Z`
- tag: `vX.Y.Z`

---

### `npm publish --access public`
Publica a versão atual no npm.

Observações:
- Pacotes scoped públicos precisam do `--access public`
- O `prepublishOnly` roda antes e garante o build

---

## Como testar localmente (sem publicar)

### Opção A — usando `npm pack` (recomendado)
1) Na lib:
```bash
npm run build
npm pack
```

## Resumo dos passos — Release rápido

```bash
npm whoami
npm run build
npm pack
npm version patch
npm publish --access public
npm view @iconis/icons
```