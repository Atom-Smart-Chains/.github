# 📘 Documentação Interna – Labels e Versionamento  

Este documento descreve a **topologia**, **status** e **versionamento** das Labels da organização, incluindo **Conecta, MemeDex, WebDex e ATOMDex (Polygon e Solana)**.  

---

## 🔎 Labels e Componentes  

| Label    | API / Backend            | Frontend              | Admin / Dashboard     | Banco de Dados | Contratos                   | Status | Versão Atual |
|----------|--------------------------|-----------------------|-----------------------|----------------|-----------------------------|--------|--------------|
| **Conecta >=v5** | [conecta-api](https://github.com/Atom-Smart-Chains/conecta-api) (AdonisJS) | [conecta-frontend-v5](https://github.com/Atom-Smart-Chains/conecta-frontend-v5) (Angular) | [conecta-admin](https://github.com/Atom-Smart-Chains/conecta-admin) (Angular) | PostgreSQL | Polygon (Solidity) | Ativo  | v5 (prod.) |
| **Conecta <=v4** | [conecta-api](https://github.com/Atom-Smart-Chains/conecta-api) (AdonisJS) | [conecta-frontend-v4](https://github.com/Atom-Smart-Chains/conecta-frontend-v4) (Angular) | [conecta-admin](https://github.com/Atom-Smart-Chains/conecta-admin) (Angular) | PostgreSQL | Polygon (Solidity) | Ativo  | v3/4 (prod.) |
| **MemeDex** | [memedex-api](https://github.com/Atom-Smart-Chains/memedex-api) (AdonisJS) | [memedex-frontend](https://github.com/Atom-Smart-Chains/memedex-frontend) (Angular) | —                     | PostgreSQL     | Polygon (Solidity) | Ativo  | v3/4/5 (prod.) |
| **Savings168** | [savings168-api](https://github.com/Atom-Smart-Chains/savings168-api) (AdonisJS) | [savings168-frontend](https://github.com/Atom-Smart-Chains/savings168-frontend) (Angular) | —                     | PostgreSQL     | Polygon (Solidity) | Ativo  | v4/5 (prod.) |
| **WebDex >=v5**  | [webdex-api](https://github.com/Atom-Smart-Chains/webdex-api) (AdonisJS)  | [webdex-frontend-v5](https://github.com/Atom-Smart-Chains/webdex-frontend-v5) (Angular)  | —                     | PostgreSQL     | Polygon (Solidity) | Ativo  | v5 (prod.) |
| **WebDex <=v4**  | [webdex-api](https://github.com/Atom-Smart-Chains/webdex-api) (AdonisJS)  | [webdex-frontend-v4](https://github.com/Atom-Smart-Chains/webdex-frontend-v4) (HTML/TypeScript/SCSS)  | —                     | PostgreSQL     | Polygon (Solidity) | Ativo  | v3/4 (prod.) |
| **ATOMDex Polygon** | [atomdex-polygon-api](https://github.com/Atom-Smart-Chains/atomdex-polygon-api) (AdonisJS) | [atomdex-polygon-frontend](https://github.com/Atom-Smart-Chains/atomdex-polygon-frontend) (Angular) | `atomdex-polygon-admin` (Angular) | PostgreSQL | Polygon (Solidity) | Ativo  | v4/5 (prod.) |
| **ATOMDex Solana** | [atomdex-solana/frontend/src/elysia](https://github.com/Atom-Smart-Chains/atomdex-solana/tree/develop/front-end/src/elysia) (Elysia (Bun/TypeScript)) | [atomdex-solana/frontend/src/elysia](https://github.com/Atom-Smart-Chains/atomdex-solana/tree/develop/front-end) (Next.js) | — | — | [atomdex-solana/frontend/contratos](https://github.com/Atom-Smart-Chains/atomdex-solana/tree/develop/front-end) Solana (Rust) | Inativo | v1 (dev.) |

> ℹ️ As APIs (`*-api`) são compartilhadas entre versões (v3, v4, v5).  
> Apenas os frontends mudam conforme a evolução da versão.

---

## 🎓 Stack Tecnológica  

- **Backend**: AdonisJS (Node.js) & Elysia (Bun/TypeScript)
- **Frontend/Admin**: Angular & Next.js
- **Banco de Dados**: PostgreSQL  
- **Contratos**:  
  - Solana → Anchor / Rust  
  - Polygon → Solidity / EVM  
- **Versionamento**: GitFlow + Semantic Versioning  
- **Deploy**: PM2 + CI/CD (ajustado por projeto)  

---

# 📋 Estrutura de Branches e Versionamento

Este projeto segue uma estratégia de **branches** e **tags** para gerenciamento de versões paralelas, permitindo manter múltiplas versões com estabilidade e organização.  
Esse modelo garante que versões antigas possam ser preservadas para histórico ou manutenção crítica, enquanto a versão mais recente continua evoluindo.

---

## 🌿 Estrutura de Branches

```text
main                # Produção - última versão (ex: v5.x)
├── develop         # Desenvolvimento da versão atual (v5.x)
│   └── feature/*   # Funcionalidades para v5.x
│
├── maintenance/v4.x    # Manutenção da versão 4.x
│   ├── develop/v4.x    # Desenvolvimento ativo para v4.x
│   │   └── feature/v4.x/*   # Funcionalidades para v4.x
│   └── hotfix/v4.x/*   # Correções críticas em produção
│
└── maintenance/v3.x    # Manutenção da versão 3.x
    ├── develop/v3.x    # Desenvolvimento ativo para v3.x
    │   └── feature/v3.x/*   # Funcionalidades para v3.x
    └── hotfix/v3.x/*   # Correções críticas em produção
```

- **`develop`** → Linha principal de desenvolvimento da versão atual (originada a partir de `main`).  
- **`develop/vX.x`** → Linha de desenvolvimento de versões antigas ainda mantidas (originada a partir de `maintenance/vX.x`).  
- **`maintenance/vX.x`** → Apenas hotfixes e releases estáveis. Originada a partir da `tag` vX.x.  
- **`feature/* ou feature/vX.x/*`** → Desenvolvimento de novas funcionalidades para a versão.  
- **`hotfix/vX.x/*`** → Correções críticas aplicadas diretamente em produção e propagadas para o desenvolvimento correspondente.  

---

## 🏷️ Estratégia de Tags

As **tags** seguem o padrão Semantic Versioning (MAJOR.MINOR.PATCH):

- `v4.0.0` → Release inicial da versão 4  
- `v4.0.1` → Hotfix para a versão 4  
- `v4.1.0` → Nova funcionalidade na versão 4  
- `v5.0.0` → Release da versão 5  

> 🔑 Sempre criar **tags** a partir de **commits estáveis em branches de manutenção (`maintenance/vX.x`) ou da `main` (no caso da última versão)**.

---

## 🔄 Fluxo de Trabalho

### 🔹 Criar branch de manutenção e desenvolvimento

```bash
git checkout -b maintenance/vX.x vX.x.x
git checkout -b develop/vX.x maintenance/vX.x
```

---

### 🔹 Desenvolvimento de novas features

```bash
git checkout develop/vX.x
git checkout -b feature/nova-funcionalidade
```

---

### 🔹 Correções críticas (hotfix)

Quando há uma falha crítica em produção:

```bash
git checkout maintenance/vX.x
git checkout -b hotfix/correcao-urgente
```

Após implementar e testar o hotfix, ele deve ser aplicado **tanto na branch de manutenção quanto na de desenvolvimento** para manter a consistência:

1. Merge do hotfix na manutenção:
   ```bash
   git checkout maintenance/vX.x
   git merge hotfix/correcao-urgente
   ```

2. Merge da manutenção (já com hotfix) na develop correspondente:
   ```bash
   git checkout develop/vX.x
   git merge maintenance/vX.x
   ```

📌 **Explicação passo a passo da troca de branches para merge:**
1. Use `git checkout <branch>` para mudar de branch.  
2. Faça o merge usando `git merge <branch-que-será-juntada>`.  
3. Repita o processo na branch `develop/vX.x`, mas desta vez juntando a manutenção.  

---

## 🚀 Lançamento de Versões

```bash
git checkout maintenance/vX.x
git tag -a vX.x.x -m "Nova versão x.x.x"
git push origin maintenance/vX.x --tags
```

---

## ✅ Boas Práticas

- **`main`** → Sempre representa a última versão estável em produção.  
- **`maintenance/vX.x`** → Somente hotfixes e versões estáveis.  
- **`develop`** → Desenvolvimento da última versão.  
- **`develop/vX.x`** → Desenvolvimento de versões antigas mantidas.  
- **`feature/*`** → Desenvolvimento de novas funcionalidades.  
- **`hotfix/*`** → Correções críticas feitas diretamente em produção.  
- **Tags** → Criadas a partir de commits estáveis em branches de manutenção ou da `main`.  
- **Merges** → Hotfixes sempre devem ser propagados também para a branch `develop` correspondente.  
