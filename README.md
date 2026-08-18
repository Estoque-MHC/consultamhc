# Consulta de Estoque MHC

Página de consulta de estoque (por material ou por endereço) publicada automaticamente
no GitHub Pages. Ninguém precisa subir planilha pelo celular — o repositório gera a
página sozinho toda vez que alguém troca as planilhas na pasta `dados/`.

## Como funciona

1. Alguém exporta do ERP as duas planilhas: **"Saldo por endereço"** e **"Estoque detalhado"**.
2. Essa pessoa sobe os dois arquivos `.xlsx` na pasta [`dados/`](./dados) deste repositório
   (pelo próprio site do GitHub, sem precisar instalar nada — veja o passo a passo abaixo).
3. Isso dispara automaticamente um workflow do GitHub Actions
   ([`.github/workflows/publicar.yml`](./.github/workflows/publicar.yml)), que roda o script
   [`gerar_estoque_mhc.py`](./gerar_estoque_mhc.py). Esse script lê as duas planilhas, monta
   os dados e gera o arquivo `index.html` — a página publicada.
4. Em ~1 minuto, quem acessar o link do GitHub Pages já vê os dados novos. Nenhum celular
   precisa importar nada.

## Configuração inicial (só uma vez)

### 1. Criar o repositório

No GitHub, crie um repositório **público** novo (ex: `mhc-consulta-estoque`) — precisa ser
público porque o GitHub Pages grátis só publica repositórios públicos.

### 2. Subir estes arquivos

Suba todo o conteúdo desta pasta pro repositório, mantendo a mesma estrutura:

```
.github/workflows/publicar.yml
consulta_estoque_mhc_template.html
gerar_estoque_mhc.py
index.html
dados/            (pasta vazia por enquanto)
```

Pelo site do GitHub: no repositório vazio, clique em **"uploading an existing file"**,
arraste todos esses arquivos/pastas e confirme o commit direto na branch `main`.

### 3. Dar permissão de escrita pro workflow

O workflow precisa poder commitar o `index.html` atualizado sozinho:

- Vá em **Settings → Actions → General**
- Em **"Workflow permissions"**, marque **"Read and write permissions"**
- Salve

### 4. Ativar o GitHub Pages

- Vá em **Settings → Pages**
- Em **"Build and deployment" → Source**, escolha **"Deploy from a branch"**
- Em **Branch**, escolha **`main`** e pasta **`/ (root)`**
- Salve

O GitHub mostra o link da página (algo como `https://SEU-USUARIO.github.io/mhc-consulta-estoque/`).
Esse é o link que vai pros celulares.

## Uso do dia a dia (depois de configurado)

Toda vez que tiver planilha nova do ERP:

1. Abra a pasta [`dados/`](./dados) no site do GitHub.
2. **Apague os dois arquivos `.xlsx` antigos** que estão lá (evita o script ficar em dúvida
   sobre qual planilha é a mais recente).
3. Clique em **"Add file" → "Upload files"** e arraste os dois arquivos novos exportados do
   ERP (não precisa renomear).
4. Role até embaixo e clique em **"Commit changes"** (direto na branch `main`).
5. Espere ~1 minuto — vá na aba **Actions** do repositório pra acompanhar o progresso (uma
   bolinha amarela girando vira ✅ verde quando termina).
6. Pronto — quem abrir o link do GitHub Pages já vê os dados novos, sem fazer nada.

Se o workflow falhar (❌ vermelho na aba Actions), clique nele pra ver o log — o erro mais
comum é a pasta `dados/` ter mais de um arquivo que parece a mesma planilha (apague os
antigos) ou nenhum arquivo reconhecível (confira se exportou as planilhas certas do ERP).

## Uso manual (sem repositório)

O arquivo `consulta_estoque_mhc_template.html` também funciona sozinho, sem nenhuma dessas
automações: abra ele direto no navegador e use a tela de upload manual (arrastar os dois
`.xlsx`). Nesse modo, o navegador guarda os dados localmente (localStorage) e não pede
upload de novo da próxima vez que abrir *nesse mesmo aparelho* — útil pra testar ou pra uso
individual em um computador, mas não substitui a publicação automática pra múltiplos
celulares.

## Arquivos deste repositório

| Arquivo | O que é |
| --- | --- |
| `consulta_estoque_mhc_template.html` | O app em si (HTML/CSS/JS autocontido). Não é a página publicada diretamente — é o *molde* que o script preenche com os dados. |
| `gerar_estoque_mhc.py` | Lê as duas planilhas da pasta `dados/` e gera o `index.html` a partir do template. |
| `.github/workflows/publicar.yml` | Roda o script automaticamente a cada push na pasta `dados/`. |
| `dados/` | Onde ficam as duas planilhas mais recentes exportadas do ERP. |
| `index.html` | A página publicada de fato pelo GitHub Pages — gerada automaticamente, não edite à mão. |
