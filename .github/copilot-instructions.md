# Instruções para Agentes IA - Sistema de Monitoramento AWS

## 🎯 Visão Geral do Projeto

Este é um **sistema educacional de monitoramento de disponibilidade** que implementa observabilidade em nuvem AWS. O projeto detecta automaticamente falhas de site, classifica gravidade com IA e envia alertas via integração com CloudWatch, SNS e Grafana.

**Público-alvo:** Alunos AWS re/Start | **Tipo:** Projeto educacional full-stack | **Fase:** Desenvolvimento

---

## 🏗️ Arquitetura Principal

```
EventBridge (Agendador) 
  → AWS Lambda (Teste de disponibilidade HTTP)
    → CloudWatch (Armazena métricas)
      → IA (Análise de padrões e classificação)
        → Alarmes AWS
          → SNS (Notificação)
            → Grafana (Dashboard tempo real)
```

**Fluxo crítico:** O Lambda executa verificações periódicas de disponibilidade (teste HTTP), persiste métricas no CloudWatch, a IA analisa padrões para classificar gravidade, alarmes disparam automaticamente ao cruzar limiares, notificações são enviadas via SNS.

**Por que essa arquitetura:** Separação clara entre coleta (Lambda), armazenamento (CloudWatch), análise (IA), e visualização (Grafana) permite escalabilidade e facilita testes independentes de cada camada.

---

## 📁 Estrutura de Diretórios

```
.github/
  copilot-instructions.md (este arquivo)
docs/
  Etapa 1 - Monitoramento de Disponibilidade AWS.docx
  Etapa 2 - Arquitetura do projeto.docx
  Etapa 3 - Desenvolvimento técnico.docx
  Etapa 4 - Documentação ABNT.docx
Site/
  index.html (Homepage do projeto - Bootstrap 5, documentação)
  metrics.html (Página de métricas - Chart.js para gráficos)
assets/
  banner-tecnologias.png
  arquitetura-fluxo.png
```

**Convenção importante:** 
- `Site/` contém frontend estático (testbed e dashboard de apresentação)
- `docs/` documenta cada etapa SCRUM do projeto
- Não há código Lambda/infraestrutura no Git (gerenciado via console AWS ou IAC separado)

---

## 🔑 Componentes Críticos

### 1. **Front-end (Site/)**
- `index.html` - Homepage com documentação técnica e visão geral
- `metrics.html` - Visualização de métricas com Chart.js
- **Stack:** HTML5 + Bootstrap 5 + Chart.js
- **Padrão:** Ambas as páginas seguem estrutura Navbar + Hero + Seções de conteúdo
- **Integração:** Métricas são exemplos estáticos (em produção, viriam da API GraphQL do Grafana)

### 2. **Monitoramento (AWS)**
- **Lambda Function:** Executa `curl` ou `http.get()` em intervalo regular (EventBridge trigger)
- **CloudWatch Metrics:** Metrica customizada `SiteAvailability` (0=down, 1=up)
- **CloudWatch Logs:** Armazena detalhes de cada verificação
- **Alarmes:** Disparam se disponibilidade < 1 por 3+ minutos (configurado como threshold)

### 3. **IA (Análise e Alertas)**
- **Propósito:** Classificar gravidade (baixa/média/crítica) baseado em padrões
- **Entrada:** Métricas do CloudWatch + histórico de falhas
- **Saída:** Mensagens naturais de alerta e sugestões de ação
- **Exemplo esperado:** 
  ```
  ⚠️ Gravidade: Alta
  O site está indisponível há 3 minutos
  Possível falha HTTP
  Ação: Verificar conectividade do servidor hospedado
  ```

### 4. **SNS & Notificações**
- Tópico SNS recebe alertas do CloudWatch Alarms
- Subscrições: Email (para validação manual), Slack/Teams (opcional)

### 5. **Grafana (Dashboard)**
- Conecta-se à AWS como data source (CloudWatch)
- Dashboard padrão: Gráfico de disponibilidade 24h + tabela de alertas
- Métrica chave: `SiteAvailability` (percentual)

---

## 🔄 Fluxos Críticos de Desenvolvimento

### Implementar Nova Verificação de Disponibilidade
1. Criar função Lambda (ou estender a existente)
2. Definir métrica customizada no CloudWatch (ex: `SiteResponseTime`)
3. Adicionar alarme baseado na nova métrica
4. Configurar SNS topic como ação do alarme
5. Adicionar painel correspondente no Grafana
6. **Documentar** etapa em `docs/Etapa 3 - Desenvolvimento técnico.docx`

### Adicionar Nova Página Web
1. Criar `.html` em `Site/` seguindo padrão: Navbar + Hero + seções
2. Referenciar Bootstrap 5 (CDN) e Chart.js onde necessário
3. Vincular navbar entre `index.html` e nova página
4. Atualizar README.md com navegação
5. Testar responsividade mobile

### Simular Falha (Teste)
1. Parar/matar o servidor web hospedado
2. Aguardar próxima execução do Lambda (EventBridge)
3. Confirmar CloudWatch registra métrica = 0
4. Validar alarme dispara em 3+ minutos
5. Verificar alerta foi enviado via SNS
6. Documentar resultado em `docs/Etapa 3`

---

## 📋 Convenções Específicas do Projeto

### Nomes e Padrões
- **Métricas CloudWatch:** CamelCase (ex: `SiteAvailability`, `SiteResponseTime`)
- **Alarmes:** Formato `{SiteName}-{MetricName}-{Threshold}` (ex: `MainSite-Availability-Down`)
- **Variáveis de ambiente (Lambda):** UPPERCASE com underscores
- **HTML IDs/Classes:** kebab-case (ex: `availability-chart`, `metrics-section`)

### Documentação
- **Em código:** Comentários técnicos apenas em partes não óbvias
- **Projeto:** Etapas SCRUM documentadas em `docs/` (formato .docx)
- **README:** Atualizar ao adicionar tecnologias ou mudanças arquiteturais

### Idioma
- **Código:** Pode ser inglês ou português (projeto educacional, flexível)
- **Comentários/Docs:** Português (público educacional BR)
- **Mensagens de alerta:** Português natural (usuário final)

---

## 🚀 Configuração e Comandos Típicos

Nota: Código Lambda e infraestrutura não estão versionados no Git.

### Para desenvolver Site/
```bash
# Servidor local simples (validação HTML)
cd Site/
python -m http.server 8000  # Ou Live Server no VS Code
# Acessar: http://localhost:8000
```

### Para validar integração AWS
1. Acessar AWS Console → CloudWatch → Métricas customizadas
2. Verificar presença de `SiteAvailability`
3. Consultar Logs do Lambda para erros
4. Testar alarme manualmente via Console (ativar/desativar)

---

## ⚠️ Armadilhas Comuns

1. **Métrica não aparece no CloudWatch:** Verificar permissões IAM do Lambda (ação `cloudwatch:PutMetricData`)
2. **Alarme não dispara:** Validar threshold e período de avaliação (padrão: 3 DataPoints fora de threshold)
3. **IA não analisa corretamente:** Passar histórico completo de falhas, não apenas última falha
4. **Grafana mostra dados atrasados:** CloudWatch tem ~5 min de delay; recarregar dashboard
5. **SNS não envia emails:** Validar subscrição (check email de confirmação)

---

## 📚 Referências Internas

- [README.md](../README.md) - Visão geral e motivação do projeto
- [Site/index.html](../Site/index.html) - Template de homepage
- [Site/metrics.html](../Site/metrics.html) - Template de dashboard visual
- `docs/` - Documentação SCRUM detalhada por etapa

---

## 🎓 Contexto SCRUM do Projeto

O projeto é estruturado em 4 etapas (não sprints técnicos, mas entregas lógicas):
1. **Etapa 1:** Definir monitoramento de disponibilidade
2. **Etapa 2:** Arquitetar solução AWS
3. **Etapa 3:** Implementar Lambda, CloudWatch, Alarmes, IA
4. **Etapa 4:** Documentar conforme ABNT

Ao modificar código ou infraestrutura, **sempre atualizar a documentação correspondente** em `docs/Etapa X`.

---

## 💡 Dicas para Agentes IA

- **Preserve idioma português** em comments e docs para coerência educacional
- **Sempre valide integração AWS** após modificar Lambda ou métricas
- **Teste falhas localmente** antes de propor mudanças em produção
- **Documente "por quê"** não apenas "como" (projeto educacional requer clareza)
- **Reuse componentes Bootstrap/Chart.js** existentes em Site/ para manter consistência visual
