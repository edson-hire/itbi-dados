# Buscador de Transações ITBI — Guia de Publicação

## O que há neste pacote

```
site/
├── index.html          <- a página de busca
├── manifest.json       <- lista dos anos disponíveis (você edita ao adicionar anos)
├── lib/                <- biblioteca de consulta remota (não mexer)
│   ├── index.js
│   ├── sqlite.worker.js
│   └── sql-wasm.wasm
└── dados_2026/         <- base de 2026 em pedaços de até 20MB
    ├── config.json
    ├── itbi_2026.db.000
    ├── itbi_2026.db.001
    └── itbi_2026.db.002
```

## Publicação (uma vez só, ~15 minutos)

1. **Criar conta no GitHub**: https://github.com/signup (gratuito).

2. **Criar o repositório**: clique no `+` (canto superior direito) → "New repository".
   - Nome: `itbi-busca` (ou o que preferir)
   - Visibilidade: **Public** (necessário para o site gratuito; os dados já são públicos)
   - Clique em "Create repository".

3. **Subir os arquivos**: na página do repositório, clique em
   "uploading an existing file" (ou "Add file" → "Upload files").
   - Arraste TODO o conteúdo da pasta `site/` (incluindo as subpastas
     `lib/` e `dados_2026/`). Dica: arraste as pastas inteiras — o GitHub
     preserva a estrutura.
   - Clique em "Commit changes" e aguarde o upload terminar.

4. **Ativar o site**: no repositório, vá em "Settings" → "Pages" (menu lateral).
   - Em "Source", selecione "Deploy from a branch".
   - Em "Branch", selecione `main` e pasta `/ (root)`. Salve.
   - Aguarde 1-2 minutos. O endereço do site aparece no topo dessa mesma página,
     no formato: `https://SEU-USUARIO.github.io/itbi-busca/`

5. **Esse é o link que você compartilha com o time.** Qualquer pessoa abre
   e usa, sem login, sem instalação.

## Atualização mensal (~5 minutos)

1. Baixe a planilha mais recente do ano corrente no site da Prefeitura:
   https://prefeitura.sp.gov.br/web/fazenda/w/acesso_a_informacao/31501
2. Envie o arquivo para o Claude e peça: "gere a base atualizada de ITBI".
   O Claude devolve a pasta `dados_2026/` atualizada (mesmos nomes de arquivo).
3. No GitHub, entre na pasta `dados_2026/` do repositório → "Add file" →
   "Upload files" → arraste os novos arquivos (eles substituem os antigos
   automaticamente por terem o mesmo nome) → "Commit changes".

## Adicionar um ano novo (ex: dados históricos de 2025)

1. Envie a planilha do ano para o Claude e peça a base preparada.
2. Suba a pasta `dados_2025/` gerada para o repositório (mesmo processo do passo 3 acima).
3. Edite o arquivo `manifest.json` no próprio GitHub (clique no arquivo → ícone
   de lápis) e adicione a linha do ano novo:

```json
{
  "anos": {
    "2026": "dados_2026/config.json",
    "2025": "dados_2025/config.json"
  }
}
```

4. "Commit changes". Pronto — o ano novo já aparece no filtro da ferramenta.

## Observações

- **Espaço**: o GitHub comporta muitos anos sem problema, mas o site publicado
  tem um limite recomendado de ~1GB (uns 8-10 anos de dados). Se um dia você
  quiser as duas décadas inteiras, dá para dividir em dois repositórios —
  me peça ajuda quando chegar lá.
- **Velocidade**: cada busca baixa só alguns KB, não a base inteira.
  A primeira busca de cada ano demora 1-3 segundos; as seguintes são mais rápidas.
- **Busca por endereço**: funciona por palavras (ex: "faria lima" encontra
  "AV BRIG FARIA LIMA"). Palavras incompletas funcionam como prefixo
  (ex: "paulis" encontra "PAULISTA").
- **Busca por SQL**: digite o número completo ou só o começo (com ou sem
  pontos/traço).
