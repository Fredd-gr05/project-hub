# PROJECT HUB - Product Specification

**Versão**: 1.0 MVP - GitHub Codespaces
**Data**: 29 de Dezembro de 2025
**Desenvolvedor**: Iris/Nexus
**Prazo**: 14 dias (60h/mês Codespaces free)
**Ambiente**: GitHub Codespaces + Vercel Free
**Custo**: R$ 0 (exceto AI ~R$ 5-15/mês)

---

## 1. VISÃO GERAL

### 1.1 Problema Validado

Desenvolvedor solo trabalha com múltiplos projetos simultâneos:
- **Apps/WebApps** (React, Vue, Next.js)
- **ETL/Pipelines** (Python, Jupyter)
- **Consultoria Financeira** (clientes com escopos distintos)

**Dados dispersos em**:
- VSCode (scripts)
- Jupyter Notebooks
- Google Docs/Drive
- VPS (produção)
- Pastas locais
- PostgreSQL (schemas)

**Consequência**:
- ⏱️ 5-10 minutos para encontrar informação de 1 projeto
- 🔴 Perda de contexto entre tarefas
- ❌ Informações perdidas/esquecidas
- 🚨 Dificuldade em visualizar status global

### 1.2 Solução Proposta

**Project Hub**: Canvas visual infinito (similar a Miro) que centraliza informações de projetos com:

✅ **Cards customizados por tipo** (ETL, WebApp, Consultoria)
✅ **Links diretos** para arquivos dispersos (VSCode, Jupyter, Google Docs, VPS)
✅ **Status visual** (cores por status: Planejamento, Dev, Produção, Pausado)
✅ **AI Assistant** (Gemini) para sugerir templates e organização
✅ **Busca global** rápida
✅ **Armazenamento centralizado** (Supabase Postgres)

### 1.3 Público-Alvo

- **Primário**: Desenvolvedor solo (v1 - MVP)
- **Secundário**: Consultores/devs freelancers (v2+)

### 1.4 Diferencial vs. Concorrentes

| Recurso | Miro | Notion | ClickUp | **Project Hub** |
|---------|------|--------|---------|--------|
| Canvas Infinito | ✅ Pago | ❌ | Básico | ✅ Grátis |
| Templates Técnicos | ❌ | Limitado | Limitado | ✅ Específicos |
| AI Integrada | ❌ | Pago | Pago | ✅ Gratuita (Gemini) |
| Custo | 💰💰💰 | 💰💰 | 💰💰 | 💰 Grátis |
| Customizável | ❌ | Parcial | Parcial | ✅ 100% (Code) |

---

## 2. CONTEXTO TÉCNICO & INFRAESTRUTURA

### 2.1 Ambiente Disponível

**VPS srv876135** (Recomendação Worm):
- RAM: 7.8 GB (680 MB em uso) ✅ Ótima folga
- CPU: Suficiente
- Traefik v3: Configurado para múltiplos sites
- Docker: Ativo com containers existentes

**Supabase (detectabi)** Backend:
- Postgres: Core database
- Auth: Nativo
- Storage: MinIO
- Realtime: WebSockets
- Status: Ativo, pronto para usar

### 2.2 Decisão: Stack Híbrido

**MVP (Prototipagem - Semanas 1-2)**:
- GitHub Codespaces (desenvolvimento online, zero instalação)
- Vercel Free (deploy automático)
- LocalStorage + Supabase (dados)

**Produção (Semanas 3+)**:
- srv876135 + Docker + Traefik
- Supabase como backend
- HTTPS automático via Let's Encrypt

---

## 3. ARQUITETURA TÉCNICA

### 3.1 Stack Definida

| Camada | Tecnologia | Versão | Por quê |
|--------|-----------|--------|----------|
| Frontend | Next.js | 15 | SSR rápido, App Router, API routes |
| Canvas | tldraw SDK | 2.x | Infinito, customizável, open-source |
| UI | Tailwind CSS | 4 | Rápido, responsivo, utilities-first |
| Tipagem | TypeScript | 5.7 | Type safety, menos bugs |
| Database | Supabase Postgres | 15 | Auth + RT + Storage |
| AI | Gemini 2.0 Flash | API | Rápido, barato |
| Auth | Supabase Auth | Nativo | Email/senha, JWT |
| Hosting MVP | Vercel | Free | Deploy automático, SSL gratis |
| Hosting Prod | srv876135 Docker | Ativo | Controle total |

---

## 4. ESCOPO MVP - MoSCoW PRIORIZADO

### 4.1 MUST HAVE (Semana 1-2) - CRÍTICO

#### M1: Canvas Infinito com tldraw
- [x] Integrar tldraw SDK em página Next.js
- [x] Canvas carrega vazio com grid de background
- [x] Usuário consegue: Dragging/pan (scroll + click drag), Zoom (scroll wheel)
- [x] Estado persiste em LocalStorage (fallback Supabase)

#### M2: Cards de Projeto Customizados (3 tipos)

**Tipo 1: ETL**
```json
{
  "id": "uuid",
  "type": "ETL",
  "name": "ETL Cliente A",
  "status": "Produção",
  "description": "Pipeline de dados de vendas",
  "links": {
    "vscode_path": "vscode://file/opt/scripts/etl.py",
    "jupyter_url": "http://jupyter.vps:8888/lab",
    "google_docs": "https://docs.google.com/...",
    "vps_path": "ssh://user@vps:/opt/data/"
  }
}
```

**Tipo 2: WebApp**
```json
{
  "id": "uuid",
  "type": "WebApp",
  "name": "Dashboard XYZ",
  "status": "Desenvolvimento",
  "links": {
    "github": "https://github.com/user/dashboard",
    "deploy_url": "https://dashboard.vercel.app",
    "env_vars_path": "vscode://file/home/user/.env.local"
  }
}
```

**Tipo 3: Consultoria**
```json
{
  "id": "uuid",
  "type": "Consultoria",
  "name": "Projeto Empresa XYZ",
  "status": "Planejamento",
  "links": {
    "google_drive": "https://drive.google.com/drive/folders/...",
    "database_url": "vscode://file/home/user/cliente.sql",
    "kpis_tracking": "https://docs.google.com/spreadsheets/..."
  }
}
```

#### M3: CRUD Completo de Projetos
- Create: Botão "+" abre modal
- Read: Carrega cards ao montar página
- Update: Double-click no card para editar
- Delete: Botão X com confirmação

#### M4: Busca Global & Filtro
- Input de busca filtra por nome/cliente em tempo real
- Buttons de filtro por status (Todos, Planejamento, Dev, Prod, Pausado)
- Mostra contador: "Todos (5) | Dev (2) | Prod (2)"

### 4.2 SHOULD HAVE (Se sobrar tempo - Semana 2)

#### S1: AI Assistant com Gemini
- Botão "🤖 Gerar com AI" no modal
- Envia prompt para Gemini
- Exibe sugestões em modal
- Botão "Usar Template" preenche campos

#### S2: Templates Pré-definidos
- Carregar template por tipo
- Pré-preenchimento de campos
- Editar antes de salvar

### 4.3 WON'T HAVE (Deixar para v2)
- ❌ Colaboração em tempo real
- ❌ Integração com GitHub/Google Drive APIs
- ❌ Autenticação multi-usuário
- ❌ Mobile app responsiva

---

## 5. BANCO DE DADOS (Postgres - Supabase)

### 5.1 Schema SQL

```sql
CREATE SCHEMA project_hub;

CREATE TABLE project_hub.projects (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  type TEXT NOT NULL CHECK (type IN ('ETL', 'WebApp', 'Consultoria')),
  name TEXT NOT NULL,
  status TEXT NOT NULL CHECK (status IN ('Planejamento', 'Desenvolvimento', 'Produção', 'Pausado')),
  description TEXT,
  links JSONB DEFAULT '{}'::jsonb,
  canvas_x FLOAT DEFAULT 0,
  canvas_y FLOAT DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

ALTER TABLE project_hub.projects ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view own projects"
  ON project_hub.projects FOR SELECT
  USING (auth.uid() = user_id);

CREATE POLICY "Users can insert own projects"
  ON project_hub.projects FOR INSERT
  WITH CHECK (auth.uid() = user_id);

CREATE POLICY "Users can update own projects"
  ON project_hub.projects FOR UPDATE
  USING (auth.uid() = user_id);

CREATE POLICY "Users can delete own projects"
  ON project_hub.projects FOR DELETE
  USING (auth.uid() = user_id);
```

---

## 6. USER STORIES PRIORIZADAS

### Epic 1: Canvas & Visualização

**US1.1**: Como usuário, quero ver canvas infinito para organizar projetos espacialmente
- AC1: Canvas carrega vazio ao abrir
- AC2: Consigo mover por scroll/drag
- AC3: Zoom in/out com mouse wheel
- AC4: Estado salva automaticamente

**US1.2**: Como usuário, quero ver cards de projetos no canvas
- AC1: Card mostra ícone do tipo + nome + status
- AC2: Card colorido por status (verde=prod, azul=dev, cinza=pause)
- AC3: Consigo clicar para detalhes

### Epic 2: Gestão de Projetos

**US2.1**: Como usuário, quero criar novo projeto selecionando tipo
- AC1: Botão "+" abre modal
- AC2: Escolho tipo → formulário específicocalrega
- AC3: Preencho campos obrigatórios
- AC4: Clico salvar → card aparece no canvas

**US2.2**: Como usuário, quero editar projeto existente
- AC1: Double-click no card → abre modal
- AC2: Todos campos editáveis
- AC3: Salvar atualiza em tempo real

**US2.3**: Como usuário, quero deletar projeto
- AC1: Botão X no card
- AC2: Confirm dialog com mensagem
- AC3: Deletar remove do canvas

### Epic 3: Busca & Filtro

**US3.1**: Como usuário, quero buscar projetos por nome/cliente
- AC1: Input de busca global no topo
- AC2: Filtra em tempo real (debounce 300ms)
- AC3: Highlight cards que batem

**US3.2**: Como usuário, quero filtrar por status
- AC1: Buttons de filter
- AC2: Mostra contagem por status
- AC3: Combina com busca

---

## 7. WIREFRAMES

### Tela Principal
```
┌─────────────────────────────────────┐
│ Logo Project Hub [+Novo] [🔍Buscar] │
├─────────────────────────────────────┤
│ [Todos] [Dev] [Prod] [Pausado]      │
│                                     │
│     Canvas Infinito (tldraw)        │
│     ┌─────────┐  ┌─────────┐        │
│     │ 📊 ETL  │  │ 🌐 WebApp│       │
│     │ Status  │  │ Status  │        │
│     └─────────┘  └─────────┘        │
│                                     │
└─────────────────────────────────────┘
```

---

## 8. PLANO DE AÇÃO - 14 DIAS

### Semana 1: MVP Básico
- **Dia 1-2**: Setup GitHub Codespaces + Next.js
- **Dia 3-4**: Integrar tldraw Canvas
- **Dia 5-6**: CRUD de projetos (Create/Read/Update/Delete)
- **Dia 7**: Busca e filtro básico

### Semana 2: Refinamento & Deploy
- **Dia 8-9**: Supabase integração
- **Dia 10-11**: AI Assistant (Gemini)
- **Dia 12**: Testes e bug fixes
- **Dia 13-14**: Deploy Vercel + Documentação

---

## 9. KPIs DE SUCESSO

- ✅ Canvas renderiza sem lag (<100ms)
- ✅ CRUD completo funcionando
- ✅ Busca filtra <500ms
- ✅ Deploy online em Vercel
- ✅ Prototipo testado por usuário (você mesmo)


---

## 10. HANDOFF PARA IRIS - INSTRUÇÕES

### 10.1 Contexto do Projeto

Este é um **MVP de prototipagem** para validar idéia de centralizador visual de projetos. Desenvolvedor solo com múltiplos projetos (apps, ETLs, consultoria) precisa de uma solução para:
- ✅ Organizar informações dispersas
- ✅ Encontrar dados rapidamente
- ✅ Visualizar status global de projetos

### 10.2 Ambiente de Desenvolvimento

**IDE Online** (Zero Instalação):
- GitHub Codespaces: 60h/mês free (https://github.com/codespaces)
- Abrir repositorio project-hub → Criar Codespace

**Stack**:
- Next.js 15 (App Router)
- tldraw SDK v2
- Tailwind CSS
- TypeScript
- Supabase (Postgres + Auth)
- Gemini 2.0 Flash API

### 10.3 Primeiros Passos

1. **Clonar/Abrir Repositorio**
   ```bash
   git clone https://github.com/Fredd-gr05/project-hub.git
   cd project-hub
   npm install
   ```

2. **Criar arquivo `.env.local`**
   ```
   NEXT_PUBLIC_SUPABASE_URL=https://[seu-project-id].supabase.co
   NEXT_PUBLIC_SUPABASE_ANON_KEY=[sua-chave]
   GEMINI_API_KEY=[sua-chave-gemini]
   ```

3. **Rodar em desenvolvimento**
   ```bash
   npm run dev
   # Acessa http://localhost:3000
   ```

4. **Deploy no Vercel**
   ```bash
   npm install -g vercel
   vercel login
   vercel
   ```

### 10.4 Prioridade de Desenvolvimento

**MUST HAVE PRIMEIRO** (dias 1-7):
1. Canvas infinito funcionando
2. CRUD de projetos (Create/Read/Update/Delete)
3. Busca e filtro básico

**DEPOIS** (dias 8-14):
4. Supabase integração completa
5. AI Assistant com Gemini
6. Testes e refinamentos

### 10.5 Checklist de Entrega

- [ ] Repositório com código funcional
- [ ] `README.md` com instruções de setup
- [ ] Canvas tldraw renderizando
- [ ] CRUD completo (Create/Read/Update/Delete)
- [ ] Busca e filtro funcionando
- [ ] Supabase conectado
- [ ] AI Assistant (Gemini) testado
- [ ] Deploy Vercel online
- [ ] Documentação de API routes

### 10.6 Contato

**Desenvolvedor**: Fredd-gr05 (@GitHub)
**Email**: [seu-email]
**Deadline**: 14 dias (29 de Dezembro de 2025 - 12 de Janeiro de 2026)

---

## 11. NOTAS IMPORTANTES

### 11.1 Decisão de Stack

- **Simplícidade**: Escolhemos tldraw (lightweight) vs Konva/Fabric
- **Supabase**: Combinada Auth + DB + Realtime em uma plataforma
- **Gemini**: Mais econômico que OpenAI para uso inicial
- **Vercel**: Free tier suficiente para MVP, com auto-deploy

### 11.2 Riscos Identificados

| Risco | Mitigação |
|-------|----------|
| Curva tldraw | Usar template oficial, documentos oficiais |
| LocalStorage perdido | Implementar export/import JSON (dias 13-14) |
| Performance canvas | Memoização, virtual scroll se >50 cards |
| Limite API Gemini | Rate limiting no /api/ai, monitorar uso |

### 11.3 Próximas Versões (v2+)

- Colaboração em tempo real (WebSockets Supabase)
- Integração GitHub API (auto-importar repos)
- Mobile responsivo
- Integração com ferramentas externas (Slack, Discord)
- Automação via n8n

---

**Documento criado em 29 de Dezembro de 2025**
**Status**: Pronto para desenvolvimento
**Versão**: 1.0
