# 📘 Documentação Interna – Labels e Versionamento  

Este documento descreve a **topologia**, **status** e **versionamento** das Labels da organização, incluindo **Conecta, MemeDex, WebDex e ATOMDex (Polygon e Solana)**.  

---

## 🔎 Labels e Componentes  

| Label    | API / Backend            | Frontend              | Admin / Dashboard     | Banco de Dados | Contratos                   | Status | Versão Atual |
|----------|--------------------------|-----------------------|-----------------------|----------------|-----------------------------|--------|--------------|
| **Conecta** | `conecta-api` (AdonisJS) | `conecta-frontend` (Angular) | `conecta-admin` (Angular) | PostgreSQL | Polygon (Solidity) | Ativo  | v4 (manut.) / v5 (prod.) |
| **MemeDex** | `memedex-api` (AdonisJS) | `memedex-frontend` (Angular) | —                     | PostgreSQL     | Polygon (Solidity) | Ativo  | v4 (manut.) / v5 (prod.) |
| **WebDex**  | `webdex-api` (AdonisJS)  | `webdex-frontend` (Angular)  | —                     | PostgreSQL     | Polygon (Solidity) | Ativo  | v4 (manut.) / v5 (prod.) |
| **ATOMDex Polygon** | `atomdex-polygon-api` (AdonisJS) | `atomdex-polygon-frontend` (Angular) | `atomdex-polygon-admin` (Angular) | PostgreSQL | Polygon (Solidity) | Ativo  | v4 (manut.) / v5 (prod.) |
| **ATOMDex Solana** | `atomdex-solana/frontend` (Elysia (Bun/TypeScript)) | `atomdex-solana/fronend` (Next.js) | — | — | Solana (Rust) | Inativo | v1 (dev.) |

---

## 🎓 Stack Tecnológica  

- **Backend**: AdonisJS (Node.js)  
- **Frontend/Admin**: Angular  
- **Banco de Dados**: PostgreSQL  
- **Contratos**:  
  - Solana → Anchor / Rust  
  - Polygon → Solidity / EVM  
- **Versionamento**: GitFlow + Semantic Versioning  
- **Deploy**: PM2 + CI/CD (ajustado por projeto)  

---

# 📋 Estrutura de Branches e Versionamento

Este projeto segue uma estratégia de **branches** e **tags** para gerenciamento de versões paralelas, permitindo manter múltiplas versões com estabilidade e organização.

---

## 🌿 Estrutura de Branches

`main` # Branch principal (produção - última versão - ex: v5.x)<br>
├── `develop` # Branch principal de desenvolvimento (última versão - ex: v5.x)<br>
│<br>
├── `maintenance/v4.x` # Branch de manutenção da versão 4.x<br>
│   └── `feature/v4.x/*` # Branches de funcionalidades para v4.x<br>
│<br>
└── `maintenance/v3.x` # Branch de manutenção da versão 3.x<br>
    └── `feature/v3.x/*` # Branches de funcionalidades para v3.x<br>

- **`develop`** → Linha principal de desenvolvimento da versão mais recente. Originada a partir da `branch` main. 
- **`maintenance/vX.x`** → Apenas hotfixes e releases estáveis. Originada a partir da `tag` vX.x.
- **`feature/vX.x/*`** → Desenvolvimento de novas funcionalidades para a versão. Originada a partir da `branch` maintenance/vX.x.

---

## 🏷️ Estratégia de Tags

As **tags** seguem o padrão Semantic Versioning (MAJOR.MINOR.PATCH):

- `v4.0.0` → Release inicial da versão 4
- `v4.0.1` → Hotfix para a versão 4
- `v4.1.0` → Nova funcionalidade na versão 4
- `v5.0.0` → Release da versão 5

> 🔑 Sempre criar **tags** a partir de uma branch de **manutenção**.

---

## 🔄 Fluxo de Trabalho

### 🔹 Criar branch de manutenção e desenvolvimento

`git checkout -b maintenance/vX.x vX.x.x`<br>
`git checkout -b development/vX.x maintenance/vX.x`<br>

---

### 🔹 Desenvolvimento de novas features

`git checkout development/vX.x`<br>
`git checkout -b feature/nova-funcionalidade`<br>

---

### 🔹 Correções críticas (hotfix)

Quando há uma falha crítica em produção:

`git checkout maintenance/vX.x`<br>
`git checkout -b hotfix/correcao-urgente`<br>

Após implementar e testar o hotfix, ele deve ser aplicado **tanto na branch de manutenção quanto na de desenvolvimento** para manter a consistência:

# 1. Merge do hotfix na manutenção
`git checkout maintenance/vX.x`<br>
`git merge hotfix/correcao-urgente`<br>

# 2. Merge da manutenção (já com hotfix) na develop correspondente
`git checkout development/vX.x`<br>
`git merge maintenance/vX.x`<br>

📌 **Explicação passo a passo da troca de branches para merge:**

1. Use `git checkout <branch>` para mudar de branch.
2. Faça o merge usando `git merge <branch-que-será-juntada>`.
3. Repita o processo na branch `development/vX.x`, mas desta vez juntando a manutenção.

---

## 🚀 Lançamento de Versões

`git checkout maintenance/vX.x`<br>
`git tag -a vX.x.x -m "Nova versão x.x.x"`<br>
`git push origin maintenance/vX.x --tags`<br>

---

## ✅ Boas Práticas

- **`maintenance/vX.x`** → Somente hotfixes e versões estáveis
- **`development/vX.x`** → Desenvolvimento de novas funcionalidades
- **Tags** → Sempre criadas a partir de branches de manutenção
- **Merges** → Hotfixes feitos em `maintenance` devem ser **sempre propagados para a `develop` correspondente**
