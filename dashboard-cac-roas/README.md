# Dashboard InPeace — CAC e ROAS

Página estática (um único `index.html`) que lê as métricas do n8n da InPeace.

## Como publicar
Suba `index.html` para o repositório e publique com GitHub Pages, Vercel ou Netlify (sem build).

## De onde vêm os dados
Workflow **"Dashboard InPeace — CAC e ROAS"** em https://n8n.inpeaceapp.com:

- **Coleta** (todo dia às 6h30, fuso de São Paulo): leads da tabela *Marketing LEADS* do Jestor,
  oportunidades **ganhas** do *CRM - Vendas*, gasto diário por campanha das 9 contas Meta da InPeace
  (USD convertido para BRL) e Google Ads (planilha do script, quando ligada).
- Para rodar a coleta na hora, abra:
  `https://n8n.inpeaceapp.com/webhook/dash-inpeace-atualizar?key=CHAVE`
- **Endpoint** usado pela página:
  `https://n8n.inpeaceapp.com/webhook/dash-inpeace?key=CHAVE&de=AAAA-MM-DD&ate=AAAA-MM-DD`
  (com `&diag=1` devolve o diagnóstico da última coleta).

A chave está no `index.html` (constante `CHAVE`) e no nó **Calcular métricas**. O endpoint devolve
só números agregados, sem nome, telefone ou e-mail. Quem tiver o link da página consegue ver os números.

## Regras
- **Canal** = `utm_source` do lead, normalizado no nó **Calcular métricas** (`REGRAS_CANAL`).
  `form` → Meta Ads · `google`/`google-ads` → Google Ads · `whats - site`/`organico` → Orgânico ·
  `whats` → WhatsApp · resto → Outros · vazio → Sem origem.
- **Venda** = oportunidade na fase *Ganho*; data = Data de Fechamento ou, se vazia, entrada na fase atual.
- **Venda → lead**: mesmo telefone (DDD + 8 últimos dígitos) ou e-mail. Sem lead = "Sem origem".
- **CAC** = investimento ÷ vendas · **CAC ÷ MRR** = CAC ÷ MRR novo (número exibido no card) · **ROAS** = MRR novo ÷ investimento.
- A *Marketing LEADS* começa em nov/2026: antes disso só vale o CAC geral.
