# Voop Catalog API — Public Docs (Mintlify)

Esta pasta contém a documentação pública da API REST do catálogo Voop, formatada
para o [Mintlify](https://mintlify.com).

## Estrutura

```
api-public/
├── mint.json                  # Config Mintlify + nav + OpenAPI source URL
├── introduction.mdx           # Landing page
├── authentication.mdx         # API keys, scopes, ambientes
├── rate-limits.mdx            # Limites por tier, headers, retry
├── errors.mdx                 # Formato de erro padrão
├── idempotency.mdx            # contentHash, externalId, dedup
├── concepts/
│   ├── items.mdx              # Estrutura de Item, tipos, identificação
│   ├── media.mdx              # Pipeline de ingestão de imagens
│   ├── webhooks.mdx           # Eventos, HMAC, retries
│   └── bulk-import.mdx        # JSONL streaming
├── recipes/
│   ├── initial-load-100k.mdx  # Carga inicial massiva
│   ├── incremental-sync.mdx   # Sync contínuo (push + cron)
│   ├── handle-webhooks.mdx    # Servidor Node.js de exemplo
│   └── erp-integration.mdx    # Integração com ERPs brasileiros
└── api-reference/
    └── introduction.mdx       # Referência interativa (gerada da OpenAPI)
```

## Desenvolvimento local

```bash
# Instalar Mintlify CLI (uma vez)
npm i -g mintlify

# Rodar preview local
cd infra/docs/api-public
mintlify dev
```

Abre em `http://localhost:3000` com hot reload.

## Deploy

### Opção A: Mintlify hosted (recomendado)

1. Conecte este repositório no painel Mintlify (`mintlify.com/dashboard`)
2. Defina `infra/docs/api-public` como root da doc
3. Configure custom domain `docs.voop.work` apontando para `cname.mintlify.app`
4. Cada push em `main` re-deploya automaticamente

### Opção B: Self-hosted (Cloud Run + Mintlify static export)

```bash
mintlify build
docker build -t voop-docs .
gcloud run deploy voop-docs --image voop-docs --region southamerica-east1
```

## Spec OpenAPI

A spec é gerada **automaticamente** pelo backend (Express) a partir dos schemas
Zod existentes — não há arquivo OpenAPI versionado neste repo. O Mintlify ingere
do endpoint público:

```
https://api.voop.work/api/v1/catalog/openapi.json
```

Implementação: `backend/src/modules/catalog/public-api/openapi.ts`.

## Convenções de escrita

- Títulos em **português**
- Exemplos de código com `bash` ou `js` quando possível (mais lidos)
- Sempre incluir o cabeçalho `Authorization` em exemplos
- Usar `<Tip>`, `<Warning>`, `<Note>` para destacar pegadinhas reais
- Receitas (`recipes/*`) devem ser **end-to-end runnable**, não pseudo-código
