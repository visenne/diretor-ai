# DIRETOR — Agente de IA para Cinema

Agente de produção cinematográfica com Claude (roteiro), Nano Banana 2 (storyboard) e Veo 3.1 Quality (vídeo).

---

## Deploy no Vercel (passo a passo)

### 1. Crie uma conta no Vercel
Acesse **vercel.com** e crie uma conta gratuita (pode entrar com GitHub).

### 2. Instale o Vercel CLI (opcional, mas mais rápido)
```bash
npm install -g vercel
```

### 3. Faça o deploy

**Via CLI:**
```bash
cd diretor-vercel
vercel
```
Responda as perguntas:
- "Set up and deploy?" → **Y**
- "Which scope?" → sua conta
- "Link to existing project?" → **N**
- "Project name?" → **diretor-ai** (ou o que quiser)
- "In which directory is your code?" → **./** (Enter)
- "Override settings?" → **N**

**Via GitHub (alternativa):**
1. Suba esta pasta para um repositório GitHub
2. Em vercel.com → "Add New Project" → importe o repositório
3. Clique em "Deploy" (sem alterar nada)

### 4. Configure a variável de ambiente ANTHROPIC_API_KEY

No painel do Vercel, após o deploy:
1. Vá em **Settings → Environment Variables**
2. Clique em **Add New**
3. Name: `ANTHROPIC_API_KEY`
4. Value: sua chave da Anthropic (começa com `sk-ant-...`)
   - Obtenha em: **console.anthropic.com → API Keys**
5. Selecione **Production, Preview, Development**
6. Clique **Save**
7. Vá em **Deployments → ⋯ → Redeploy** para aplicar a variável

### 5. Acesse o agente
O Vercel fornece uma URL no formato `seu-projeto.vercel.app`.
Abra no navegador e o agente estará funcionando.

---

## Configuração das APIs de mídia (dentro do agente)

Após acessar o agente pelo Vercel, clique em **"APIs & Modelos"** no topo e insira:

| API | Onde obter | Para que serve |
|-----|-----------|---------------|
| Google AI Studio (`AIzaSy...`) | aistudio.google.com/apikey | Nano Banana 2 (storyboard) + Veo 3.1 (vídeo) |
| ElevenLabs (opcional) | elevenlabs.io → Profile → API Key | Narração dos personagens |

> **Nota:** As chaves do Google AI Studio são inseridas diretamente no navegador (não precisam de variável de ambiente no Vercel) pois a API do Google tem CORS habilitado.

---

## Estrutura do projeto

```
diretor-vercel/
├── api/
│   └── claude.js          # Proxy serverless para Claude API (resolve CORS)
├── public/
│   └── index.html         # Frontend completo do agente
├── package.json
├── vercel.json            # Configuração de rotas
└── README.md
```

---

## Variáveis de ambiente necessárias

| Variável | Obrigatória | Descrição |
|----------|------------|-----------|
| `ANTHROPIC_API_KEY` | ✅ Sim | Chave da API Claude (console.anthropic.com) |

---

## Modelos utilizados

| Etapa | Modelo | Provider |
|-------|--------|----------|
| Roteiro & iteração | `claude-sonnet-4-20250514` | Anthropic |
| Storyboard (imagens) | `gemini-3.1-flash-image-preview` (Nano Banana 2) | Google AI Studio |
| Síntese de vídeo | `veo-3.1-generate-preview` | Google AI Studio |
