# Teste de QI — Guia de Deploy

## Estrutura do projeto

```
teste-qi/
├── index.html              ← Frontend completo do teste
├── api/
│   └── enviar-resultado.js ← API: gera relatório (Claude) + envia email (SendGrid)
├── vercel.json             ← Configuração da Vercel
├── package.json
└── .env.example            ← Modelo das variáveis de ambiente
```

---

## Passo a passo completo

### 1. Criar conta na Vercel (grátis)
1. Acesse https://vercel.com e clique em **Sign Up**
2. Crie conta com GitHub, GitLab ou e-mail

### 2. Subir o projeto no GitHub
1. Acesse https://github.com e crie um repositório novo (ex: `teste-qi`)
2. Faça upload de todos os arquivos desta pasta para o repositório

### 3. Fazer deploy na Vercel
1. No painel da Vercel, clique em **Add New > Project**
2. Selecione o repositório `teste-qi` do GitHub
3. Clique em **Deploy** (sem alterar nada)
4. Aguarde — em ~1 minuto seu site estará no ar!

### 4. Obter chave da API da Anthropic (Claude)
1. Acesse https://console.anthropic.com/
2. Vá em **API Keys** > **Create Key**
3. Copie a chave (começa com `sk-ant-...`)

### 5. Configurar o SendGrid (e-mail grátis até 100/dia)
1. Acesse https://sendgrid.com e crie conta grátis
2. Vá em **Settings > API Keys > Create API Key**
   - Nome: `teste-qi`
   - Permissão: **Full Access**
   - Copie a chave (começa com `SG.`)
3. Vá em **Settings > Sender Authentication > Single Sender Verification**
   - Cadastre o e-mail que vai enviar os resultados (ex: `resultado@gmail.com`)
   - Confirme no e-mail recebido

### 6. Adicionar variáveis de ambiente na Vercel
1. No painel da Vercel, abra seu projeto
2. Vá em **Settings > Environment Variables**
3. Adicione as três variáveis:

| Nome | Valor |
|------|-------|
| `ANTHROPIC_API_KEY` | `sk-ant-sua-chave-aqui` |
| `SENDGRID_API_KEY` | `SG.sua-chave-aqui` |
| `EMAIL_REMETENTE` | `resultado@seuemail.com` |

4. Clique em **Save**
5. Vá em **Deployments** e clique em **Redeploy** para aplicar

### 7. Pronto! ✅
Seu teste está 100% funcional. Acesse a URL fornecida pela Vercel (ex: `https://teste-qi.vercel.app`).

---

## Fluxo completo

```
Usuário faz o teste (25 questões)
        ↓
Tela de pagamento Pix — QR Code + Chave Pix
        ↓
Usuário preenche nome + e-mail + confirma pagamento
        ↓
API /api/enviar-resultado
  ├── Claude gera relatório personalizado (HTML)
  └── SendGrid envia o e-mail para o cliente
        ↓
Tela de sucesso
```

---

## Dúvidas frequentes

**O e-mail está indo para spam?**
Configure o SPF/DKIM no SendGrid (Sender Authentication > Domain Authentication) usando seu domínio próprio.

**Como testar localmente?**
1. Instale a Vercel CLI: `npm i -g vercel`
2. Copie `.env.example` para `.env.local` e preencha as chaves
3. Execute: `vercel dev`

**Posso usar meu domínio próprio?**
Sim! Na Vercel, vá em Settings > Domains e adicione seu domínio.
