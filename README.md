# ReceiptAI

Tire uma foto da nota fiscal e tenha o gasto organizado automaticamente.

O ReceiptAI é um app para MEIs, freelancers e pequenos negócios que não querem perder tempo digitando recibo. O usuário fotografa a nota e um modelo de visão extrai valor, estabelecimento, data e categoria, que viram uma despesa no painel.

![ReceiptAI](/scan.png)

## Telas

| Dashboard | Histórico |
|---|---|
| ![Dashboard](/dashboard.png) | ![Histórico](/history.png) |

| Insights | Mobile |
|---|---|
| ![Insights](/insights.png) | <img src="/mobile.png" width="260"> |

## Como a IA entra no fluxo

```
Foto do recibo (JPG/PNG/WEBP, até 10 MB)
   ↓
Upload para o Supabase Storage
   ↓
LLM multimodal (Llama 4 Scout via Groq) lê a imagem
   ↓
Saída estruturada em JSON: valor, estabelecimento, categoria, data e texto bruto
   ↓
Validação e normalização dos campos no back-end
   ↓
Despesa criada e exibida no dashboard e nos insights
```

- **Extração com LLM de visão**: a imagem vai direto pro modelo, sem OCR tradicional no meio. O prompt define o formato de saída e as regras de categorização para notas brasileiras (mercado, posto, SaaS, contas de consumo etc.).
- **Saída estruturada**: `response_format: json_object` e `temperature: 0` para respostas previsíveis e fáceis de validar.
- **Validação**: categorias fora da lista são descartadas, valores são normalizados e falhas de leitura marcam o recibo como `FAILED`, sem quebrar o fluxo do usuário.

## Funcionalidades

- Cadastro e login com JWT
- Scan de recibo pela câmera ou upload
- Histórico de gastos com status de processamento
- Dashboard com totais por categoria
- Insights mensais

## Tecnologias

- **Next.js** (App Router) e **TypeScript**
- **Groq API** com **Llama 4 Scout** (visão)
- **Supabase** (Postgres e Storage)
- **Tailwind CSS**
- Deploy no **Render**

## Como rodar localmente

```bash
cp .env.example .env.local   # preencha JWT_SECRET, Supabase e GROQ_API_KEY
npm install
npm run dev
```

O schema do banco está em `supabase/schema.sql`. A chave do Groq é gratuita em [console.groq.com](https://console.groq.com).

## Autor

**Arthur Roberto Alves** · [LinkedIn](https://www.linkedin.com/in/arthur-roberto-alves-bb83b0354/)
