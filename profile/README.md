# 👔 New Griff — Business Intelligence & Controladoria

Bem-vindo ao hub central de tecnologia da **New Griff**. Esta organização reúne os repositórios, ferramentas analíticas e painéis executivos de Business Intelligence da marca.

---

## 🏛️ Topologia e Arquitetura de Deploy

```mermaid
flowchart TD
    subgraph DADOS["💾 Camada de Dados Analítica"]
        DW[("Data Warehouse / Base Analítica")]
    end

    subgraph DEPLOY_AUTO["⚙️ Esteira de Integração Contínua (Auto-Poll)"]
        GIT["GitHub origin/main"]
        POLL["Cron Poller (deploy-poll-newgriff.sh)<br/>Compara HEAD do checkout com origin/main"]
        SCRIPT["Deploy Script (/var/www/newgriff-dashboard/deploy.sh)<br/>Build Standalone + PM2 Reload"]
    end

    subgraph RUNTIME["🖥️ Servidor de Produção (VPS)"]
        PM2["PM2 Runtime: newgriff-dashboard<br/>Porta 3102 (Standalone)"]
        CLOUDFLARE["Cloudflare Zero Trust (Túnel n8n-tunnel)<br/>ng.alvo7app.com"]
    end

    DW --> PM2
    GIT -->|Verificação a cada 2 min| POLL
    POLL -->|Detecta divergência| SCRIPT
    SCRIPT --> PM2
    PM2 --> CLOUDFLARE
```

---

## 📦 Repositórios

### 📊 [NewGriff](https://github.com/NewGriff-Alvo7App/NewGriff)
- **Função:** Plataforma completa de BI, Controladoria Financeira e Dashboard Executivo em Next.js 16.
- **Stack:** Next.js 16 (App Router, Standalone), TypeScript, Tailwind CSS, PM2 (`newgriff-dashboard`, porta 3102).
- **Acesso Público:** `ng.alvo7app.com` (via Cloudflare Zero Trust).
- **Ambientes:**
  - *Desenvolvimento (Workspace):* `/home/admapps/NewGriff` (porta 3103 em dev).
  - *Produção (Runtime):* `/var/www/newgriff-dashboard` (porta 3102).

---

## 🚀 Padrões de Desenvolvimento & Deploy

- **Branch Base:** `main`. Push direto na branch `main` é protegido pelo hook `pre-push`.
- **Deploy Automático:** O sistema de auto-polling sincroniza a `main` com produção em até 2 minutos após o merge do PR.
- **Commits:** Padronização com [Conventional Commits](https://www.conventionalcommits.org/).
