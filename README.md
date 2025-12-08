# Monitoramento de Site com AWS, CloudWatch, SNS e Grafana

Projeto acadêmico focado em **observabilidade, monitoramento e alertas automáticos de disponibilidade de sites na nuvem**, usando serviços da **AWS integrados ao Grafana**.

Sistema caiu? O alerta chega. Sem desculpa. Sem atraso.

---

## Objetivo do Projeto

Criar uma solução que:

- Monitore se um site está **online ou offline**
- Registre métricas automaticamente na AWS
- Gere **alertas automáticos**
- Exiba tudo em **tempo real no Grafana**

👉 Se o site cair, alguém fica sabendo na hora.

---

## O Problema que Este Projeto Resolve

Sites caem. Sempre.

O problema não é cair. O problema é:

- Ninguém perceber
- Descobrir tarde demais
- Não ter histórico
- Não ter dados
- Não ter alerta

Esse projeto elimina o escuro. Tudo vira dado.

---

## Arquitetura da Solução (Visão Geral)

Fluxo do sistema:

1. O site é monitorado
2. A AWS coleta os dados
3. O CloudWatch analisa as métricas
4. Um alarme é gerado
5. O SNS envia o alerta
6. O Grafana exibe tudo em tempo real

Nada mágico. Só engenharia bem feita.

---

## Tecnologias Utilizadas

- **AWS EC2** – Hospedagem
- **AWS CloudWatch** – Métricas e alarmes
- **AWS SNS** – Notificações
- **AWS Lambda (opcional)** – Automação
- **Grafana** – Visualização
- **Linux + Shell Script / Python**
- **GitHub** – Versionamento

---

## O Que o Sistema Monitora

- Status do site (UP/DOWN)
- Tempo de resposta
- Falhas consecutivas
- Histórico de quedas
- Picos de instabilidade

Aqui não tem opinião. Só número.

---

## Como Funcionam os Alertas

Quando o site:

- Cai
- Fica lento
- Falha repetidamente

O sistema:

- Cria o alarme no CloudWatch
- Dispara via SNS
- Alguém é avisado na hora

Monitoramento sem alerta é decoração.

---

## Visualização no Grafana

Painel mostra:

- Status em tempo real
- Tempo de resposta
- Histórico de falhas
- Disponibilidade por período

Cliente vê problema depois. Você vê antes.

---

## Ambiente de Testes

- Conta AWS educacional
- Testes locais
- Simulação de queda
- Simulação de lentidão
- Disparo real de alertas

Projeto de verdade. Nada de teatrinho.

---

## Integrantes do Grupo

Grupo 04:

- Cleison Silva Dos Santos  
- Guido Eduardo Tavares  
- Leandro Vieira Goulart  
- Marco Matheus Mira Machado  
- Otávio Vinicius Cruz Da Silva  
- Rodrigo De Carvalho  

---

## O Que Foi Aprendido

- Observabilidade na prática
- Métricas reais na AWS
- Alarmes automáticos
- Integração de serviços
- Monitoramento profissional

Depois disso, projeto sem monitoramento parece piada.

---

## Próximos Passos

- Monitorar múltiplos sites
- Alertas no WhatsApp/Telegram
- Load Balancer
- Redundância
- Central de logs

Sistema bom cresce.

---

## Estrutura do Repositório

```bash
/docs        -> documentação
/scripts     -> scripts de monitoramento
/lambda      -> funções AWS
/grafana     -> dashboards
/prints      -> evidências
README.md
